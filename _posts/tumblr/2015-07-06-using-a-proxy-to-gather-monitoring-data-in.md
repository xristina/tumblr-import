---
layout: post
title: Using a proxy to gather monitoring data in restrictive environments
date: '2015-07-06T13:19:37+03:00'
tags:
- proxy
- monitoring
- cloud management
tumblr_url: https://blog.mist.io/post/123024233456/using-a-proxy-to-gather-monitoring-data-in
---
Mist.io provides a single-stop shop for managing, monitoring and automating your infrastructure. But in order to collect monitoring data, Mist.io needs UDP outbound traffic to port 25826 to be allowed on the server. In most servers, that is already the case, but sometimes outgoing traffic can be blocked by firewalls or the server is hosted in a private network. Altough it is trivial to allow outgoing traffic in firewalled servers or servers with private IP addesses, there are still cases where a restrictive server policy is in place or the server does not have access on a publicly available server like the one at monitor.mist.io. To get around that, you can set up your own proxy server on the same network, and route the monitoring data to Mist.io throught it. Servers that are monitored send the data on the proxy server, which forwards it on monitor.mist.io UDP port 25826.

## Step 1: setting up the proxy server

We’ll use a Linux server, with iptables rules that forward traffic it receives on UDP port 25826 to monitor.mist.io’s port 25826. To set up rerouting, we just need to run the following commands:

    sysctl net.ipv4.conf.eth0.forwarding=1; iptables -t nat -A PREROUTING -p udp --dport 25826 -j DNAT --to-destination 54.183.41.39:25826; iptables -t nat -A POSTROUTING -p udp --dport 25826 -j MASQUERADE; iptables -A INPUT -p udp --dport 25826 -j ACCEPT;

It would be helpful to also add them on a file as /etc/rc.local as well, so they run after system reboots. The first command allows forwarding of traffic through the server, while the others forward traffic received on udp port 25826 to the ip of monitor.mist.io 25826 . Iptables won’t accept the hostname monitor.mist.io and needs an ip address. To double check that you have the right IP, you can also ping monitor.mist.io to get it.

## Step 2: set the monitored servers to use the proxy server

With the proxy server in place we need to setup the monitoring adent to use that, instead of monitor.mist.io. If you’ve already enabled monitoring for that server through Mist.io, the collectd monitoring agent should already have been installed.

**Linux servers**

We need to edit collectd.conf file and replace the ip on the network plugin with the ip of our proxy server (52.28.56.84 in our case)

    root@ip-172-31-30-60:/home/ubuntu# cd /opt/mistio-collectd/ root@ip-172-31-30-60:/opt/mistio-collectd# vi collectd.conf

Replace the ip with the proxy one, and restart collectd:

    LoadPlugin network \<Plugin network\> &nbsp; &nbsp;TimeToLive 128 &nbsp; &nbsp;\<Server "52.28.56.84" "25826"\> &nbsp; &nbsp; &nbsp; &nbsp;SecurityLevel Encrypt &nbsp; &nbsp; &nbsp; &nbsp;Username "c39f8e027e5f90baffbd762ef80ec6f0" &nbsp; &nbsp; &nbsp; &nbsp;Password "fdaed0af7d84ceb6" &nbsp; &nbsp;\</Server\>\<br\>

    root@ip-172-31-30-60:/opt/mistio-collectd# /opt/mistio-collectd/collectd.sh restart

It should now send the monitoring data on our proxy and relay it on Mist.io:

<figure data-orig-width="955" data-orig-height="824" class="tmblr-full"><img src="assets/tumblr-images/tumblr_inline_nquya7CY0A1rgqrs8_540.png" alt="image" data-orig-width="955" data-orig-height="824"></figure>

**Windows servers**

Edit file c:\program files\collectm\config\default.json, find the Network section and replace the hostname with the ip of your proxy. Save the file, and restart the CollectM service:  
From start button, select administration tools –\> services, find CollectM, right click and select All Tasks: Restart. The monitoring data is now going to be sent to monitor.mist.io through the proxy.

<figure data-orig-width="900" data-orig-height="639" class="tmblr-full"><img src="assets/tumblr-images/tumblr_inline_nquy441bxk1rgqrs8_540.png" alt="image" data-orig-width="900" data-orig-height="639"></figure>
