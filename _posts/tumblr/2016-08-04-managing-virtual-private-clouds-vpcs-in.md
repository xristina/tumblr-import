---
layout: post
title: Managing virtual private clouds (VPCs) in protected networks using VPN
date: '2016-08-04T14:43:35+03:00'
tags:
- vpn
- cloud computing
tumblr_url: https://blog.mist.io/post/148444184131/managing-virtual-private-clouds-vpcs-in
---
Mist.io helps you manage and monitor your virtual machines across different clouds by giving you total visibility and control of your hybrid infrastructure. Up until now barriers existed in terms of connectivity under scenarios that required the Mist.io SaaS platform to access VMs sitting in Virtual Private Clouds (VPCs) or bare metal machines and docker containers set up in private networks. With the introduction of support for Virtual Private Networks (VPNs), Mist.io can now access private networks or networks with restricted access from the public Internet, thus enabling users to manage and monitor their machines over a secure connection.

Mist.io’s VPN functionality is based on the OpenVPN protocol, which implements a Virtual Private Network (VPN) in order to create secure point-to-point connections to remote access areas. OpenVPN is capable of accessing private networks by traversing network address translators (NATs) and firewalls, while utilizing the exchange of keys in order to secure its VPN connections (or tunnels).

## Setting up the VPN

To set up a new VPN, visit the Tunnels section from your dashboard menu and click on the Add your tunnels button. (VPN support is implemented in Mist.io’s upcoming UI. To access it, click on the user thumbnail on the top right, and choose Beta UI). Type in a name for your tunnel, and optionally a description. Then, on the CIDRs field, &nbsp;add the private IP range that the cloud that you want to manage resides in. Mist.io will choose two random IPv4 addresses for the endpoints of the VPN tunnel. If you want to exclude some of the addresses of the network to avoid IP conflicts, you can fill them in on the excluded CIDRs field.

<figure data-orig-width="1560" data-orig-height="894" class="tmblr-full"><img src="assets/tumblr-images/tumblr_inline_obduzcYIjB1rgqrs8_540.png" alt="image" data-orig-width="1560" data-orig-height="894"></figure>

Once you’re done, click on the Add button. Mist.io will create the tunnel. Click on it, and Mist.io will provide you with a bash script that you’ll need to run on your VPN client - usually one of the machines or the router of your private network.

<figure data-orig-width="2541" data-orig-height="1311" class="tmblr-full"><img src="assets/tumblr-images/tumblr_inline_obdv6y1Gp11rgqrs8_540.png" alt="image" data-orig-width="2541" data-orig-height="1311"></figure>

When deploying your VPN client, make sure that there are no firewall rules blocking incoming or forwarded Internet traffic. Your VPN client needs to allow incoming data and outgoing data to Mist.io. The UDP port that is used can be seen in the script above. Additionally, make sure that the machine where the VPN client resides can forward packets to your local VMs. Please, ensure your firewall and IPtables rules (if any) have been properly configured.

As soon as you have established your VPN tunnels, you can go ahead and add your infrastructure in Mist.io. Your private network IPs will be accessible for you by the Mist.io service as if they were public. Just go ahead and [add](http://docs.mist.io/category/75-adding-your-infrastructure) your private clouds and perform actions on private VMs like you would normally do!

