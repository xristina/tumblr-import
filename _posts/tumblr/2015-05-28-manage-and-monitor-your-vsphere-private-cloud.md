---
layout: post
title: Manage and monitor your vSphere private cloud along with the rest of your infrastructure
date: '2015-05-28T18:31:37+03:00'
tags:
- hybrid cloud
- cloud management
- monitoring
- private cloud
- vsphere
tumblr_url: https://blog.mist.io/post/120109600431/manage-and-monitor-your-vsphere-private-cloud
---
<figure data-orig-height="200" data-orig-width="200"><img src="assets/tumblr-images/tumblr_inline_np2g05Jrd81rgqrs8_540.png" data-orig-height="200" style="float: right;" data-orig-width="200"></figure>

As private cloud tools become more mature, hybrid cloud setups [have started to rapidly &nbsp;increase](http://www.computerworld.com/article/2860980/hybrid-cloud-adoption-set-for-a-big-boost-in-2015.html). At Mist.io, we try to ease the pain of infrastructure management by providing a minimalistic, easy to use interface for all of your infrastructure. Mist.io already supported private clouds by being able to manage OpenStack instances. Today, we are proud to announce support for another popular private cloud, [vSphere](http://www.vmware.com/products/vsphere).

Setting up vSphere management takes only a few minutes. All you need to know is your vSphere address, your username and password. Once you log on to [https://mist.io](https://mist.io), click on the Add cloud button and fill in your vSphere’s hypervisor details.

<figure class="tmblr-full" data-orig-height="545" data-orig-width="324"><img src="assets/tumblr-images/tumblr_inline_np2g0jkUEe1rgqrs8_540.png" data-orig-height="545" data-orig-width="324"></figure>

Once you click on Add, Mist.io will try to authenticate with the hypervisor and grab a list of your machines. If it succeeds, your Virtual Machines will be added to the Machines list, along with the rest of your infrastructure, neatly tagged for easy access. You can now start, stop, reboot or destroy machines through Mist.io.

<figure class="tmblr-full" data-orig-height="725" data-orig-width="1027"><img src="assets/tumblr-images/tumblr_inline_np2g12e92v1rgqrs8_540.png" data-orig-height="725" data-orig-width="1027"></figure>

Now that the vSphere cloud is in place, it is time to set up access for Mist.io. There are two ways to do that. If you already have SSH keys that you use to connect to your servers, you can import them. Otherwise, you can create new keypairs through Mist.io and upload the public keys to your servers manually. You’ll probably already have SSH access to your servers. In that case, navigate to the Keys section and click on the Add button.

The Add key form gives you the option of generating a new keypair, uploading a file or pasting the RSA key directly. Paste the contents of your RSA key and click on the Add button.

<figure data-orig-height="354" data-orig-width="284"><img src="assets/tumblr-images/tumblr_inline_np2g295mHi1rgqrs8_540.png" data-orig-height="354" data-orig-width="284"></figure>

Your new key will appear on the Keys list. Now, go back and visit your Machines section. Click on one of your servers. If Mist.io has managed to probe the server succesfully, the Shell button at the bottom will be active. Click on the Enable monitoring button.&nbsp;

Mist.io will install the collectd agent on your server. Once it is finished you should see your first monitoring graphs and after a while the first data should start coming in.

<figure class="tmblr-full" data-orig-height="767" data-orig-width="1199"><img src="assets/tumblr-images/tumblr_inline_np2g3jrRh01rgqrs8_540.png" data-orig-height="767" data-orig-width="1199"></figure>

Being able to see how your machine was performing in the past week is great. But It’s even more important to get notified right away once things start getting out of the accepted norms. Click on the Add Rule button located below the graphs, to set up your first alerting rule. E.g. if you want to get an email once the load exceeds 4, your rule should look like this:

<figure class="tmblr-full" data-orig-height="236" data-orig-width="546"><img src="assets/tumblr-images/tumblr_inline_np2g5vAc0Y1rgqrs8_540.png" data-orig-height="236" data-orig-width="546"></figure>

If you suspect that you have a process that’s leaking memory, you can set up Mist.io to restart that process once your memory usage exceeds 95%.

<figure class="tmblr-full" data-orig-height="257" data-orig-width="1069"><img src="assets/tumblr-images/tumblr_inline_np2g6bbSLa1rgqrs8_540.png" data-orig-height="257" data-orig-width="1069"></figure>

Thats it. You’ve successfully set up monitoring for your container and added your first alerting rules. You can continue to add more monitoring metrics and alerts or write your own custom ones. If you want to add another cloud, hypervisor or docker engine, the process is similar. You can also check out the Mist.io [documentation](https://mistio.zendesk.com/hc/en-us/sections/200064458-Backends) on adding clouds.

**Are you using Host Virtual? Mist.io can help you manage and monitor your virtual machines from anywhere. [Try it for free](https://mist.io).**

