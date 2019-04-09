---
layout: post
title: Servers don’t respect vacation plans
date: '2014-07-18T12:58:00+03:00'
tags:
- termjs
- socketio
- mistio
- cloud computing
tumblr_url: https://blog.mist.io/post/92134718021/servers-dont-respect-vacation-plans
---
Summer is upon us and as the heat rises those weekend escapes are quickly becoming a necessity. But servers seem to have a total lack of respect for our plans. We’ve learned that first hand since our consulting days. Compliant with Murphy’s laws, server alerts tend to appear in the most inconvenient of times.

![image](/assets/tumblr-images/tumblr_inline_n8wtddZHe61rgqrs8.png)

This was one of the problems that we set to solve with Mist.io. There isn’t much use in getting an alert when you’re unable to act upon it and in most cases all you need to get things back and running is to send a few commands. This is why one of the main features of mist.io has always been a touch-friendly console: A safe and easy way to log in to your server and run a few commands from any web device, whether it’s a laptop or a smartphone. Well, we’ve just used the power of websockets to take this a step further!

![image](/assets/tumblr-images/tumblr_inline_n8wszhArtU1rgqrs8.png)

Mist.io now provides a fully fledged interactive terminal [built on javascript](https://github.com/chjj/term.js/) which means that when you get that alert, you can log on to Mist.io from any browser and investigate the issue as if you were sitting in front of your laptop. There is no need to worry about carrying your private keys around or storing them in your phone any more. Your keys are conveniently stored in a central, secure place (either in our servers, or in your Mist.io installation if you’ve installed the [open-source client](https://github.com/mistio/mist.io) locally). When something goes wrong, all you need to do is find a device that can access the web. Any device.

![image](/assets/tumblr-images/tumblr_inline_n8wjnySRHU1rgqrs8.png)

As an added bonus, we’ve switched from using Ajax-calls to websockets through the use of Socket.IO. Socket.IO enables real time, bi-directional communication between web clients and server which results to less calls and a smoother, faster experience for any modern browser. For older browsers, Mist.io will gracefully fall back to Ajax long polling.

We’re very excited with our new terminal and we’d love to read your thoughts as well. As always, if you have any questions or suggestions, don’t hesitate to drop us an email at [support@mist.io](mailto:support@mist.io).

**Do you have machines across clouds and want to easily manage them?&nbsp;[Sign up for Mist.io and try it for free!](https://mist.io/)**

