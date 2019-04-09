---
layout: post
title: Announcing custom SSH ports and manual installation of monitoring
date: '2014-04-01T16:43:00+03:00'
tags: []
tumblr_url: https://blog.mist.io/post/81387950498/announcing-custom-ssh-ports-and-manual
---
We are always looking for ways to make our users’ lives simpler. We want to help increase their uptime without making them jump through hoops to create a “Mist.io friendly” setup. There is not a single right way to do things, needs vary from team to team.

Adhering to the above, Mist.io now supports setups with custom SSH usernames and ports. Furthermore, it is now possible to install the monitoring agent manually without providing Mist.io access to do it for you.

When you add your SSH key to one of your machines, Mist.io will try to connect to the standard SSH port. As of today, if the service cannot connect to your machine it will ask you to provide an alternate username and port.

![image](/assets/tumblr-images/tumblr_inline_n3cu3dGF3s1rgqrs8.jpg)

One of the reasons Mist.io needs SSH access in the first place, is to automatically install and configure the monitoring agent. In some cases however, providing SSH access to Mist.io is not possible. That is not an issue any more. You can now choose to manually install the [collectd](http://collectd.org/)monitoring agent.

![image](/assets/tumblr-images/tumblr_inline_n3cu4eiT4Z1rgqrs8.jpg)

The drawback is that you have to run this on every machine. Notice that ID arguments will differ so you have to get the correct command for each one. Without SSH access you will also be missing out on some nice Mist.io features, like the touch-friendly web console. Furthermore, you will not be able to automatically run a command when a server metric, eg load, memory usage etc, reaches a certain threshold.

With all these new additions it is a perfect time to give Mist.io a try! We are always happy to hear your feedback and help you solve any issues you may have, so do not hesitate to reach out to us on [Twitter](https://twitter.com/mist_io)or at support [at] mist.io.

**Sign up for a [free trial](https://mist.io) and let us worry about your server load. We’ll notify you if something goes wrong.**

