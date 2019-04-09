---
layout: post
title: 'Early Xmas present: Bare metal server support in Mist.io'
date: '2013-12-23T18:47:00+02:00'
tags: []
tumblr_url: https://blog.mist.io/post/70907345253/early-xmas-present-bare-metal-server-support-in
---
It is that time of the year again. The time we take to spend with our family and friends, relax, party and enjoy our time off. Occasions like that, are what drove us to conceive the idea to build Mist.io.&nbsp;We had to keep an eye on multiple servers. When things went wrong we would have to get to our laptop, even when we’d rather be with our loved ones.&nbsp;Now with mist.io you can respond to alerts directly and from anywhere, using any smartphone or tablet.

For these Xmas holidays, we come bearing gifts. We are proud to announce that you can now manage and monitor&nbsp; **any server** &nbsp;through mist.io, including bare metal ones as well as VM’s on providers that are not yet fully supported. There should be no reason to carry your laptop to the xmas dinner table just in case something goes wrong.

![image](/assets/tumblr-images/tumblr_inline_my9d9n8r1H1rgqrs8.jpg)

We want Mist.io to be a valuable tool for system administration and we are confident that with our latest additions we have achieved that.&nbsp;[Give it a try](https://mist.io/)&nbsp;today. Its free for 15 days. No strings attached.

**Adding a bare metal server to Mist.io**

To add a bare metal server, select&nbsp;‘Bare Metal Server'&nbsp;on the drop list with the providers, on mist.io’s home page.

**![image](/assets/tumblr-images/tumblr_inline_my9fr5XWmO1rgqrs8.jpg)**

Αdd the hostname (or ip address), ssh key and sudo user. On pressing ‘Add’ mist.io will try to establish an ssh connection to the server and if it succeeds it will load the machines listing including your newly added server along with existing servers.

![image](/assets/tumblr-images/tumblr_inline_my9fsdPZLi1rgqrs8.jpg)

Your server will be added to the list of machines Mist.io manages.&nbsp;To enable monitoring for your machine,&nbsp;select it from the list and click the ’ **Enable monitoring**'&nbsp; button. Mist.io will download and install the collectd open source monitoring daemon.&nbsp;

A few minutes later graphs for the server’s cpu, memory and disk usage will be displayed.

![image](/assets/tumblr-images/tumblr_inline_my9ftkiSKF1rgqrs8.jpg)

**Do you have machines across clouds and want to easily manage them?&nbsp;[Sign up for Mist.io and try it for free!](https://mist.io)**

