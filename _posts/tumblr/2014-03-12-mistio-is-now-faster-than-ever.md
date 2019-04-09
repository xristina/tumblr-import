---
layout: post
title: Mist.io is now faster than ever!
date: '2014-03-12T15:02:00+02:00'
tags: []
tumblr_url: https://blog.mist.io/post/79356179972/mistio-is-now-faster-than-ever
---
One of our goals is for Mist.io to be accessible from phones and tablets, so we want it to be fast and lightweight. With our latest update we have significantly reduced the browser load for every page and increased the speed in which the pages are served.

![image](/assets/tumblr-images/tumblr_inline_n2brauaQjz1rgqrs8.jpg)

jQuery Mobile has been updated to version 1.4 which is focused on increased performance. Generation of inner markup for elements styled as buttons has been completely removed which produces a cleaner markup. In many cases the framework now just adds classes to the native element during enhancement and they’ve reduced the amount of classes that are added by the framework. Overall, jQM became more lightweight and faster and so did Mist.io.

We also use Ember.js which in turn depends on Handlebars.js for templating. Mist.io now uses precompiled templates in production which saves time on the client and reduces the required runtime size of the minified js library. This translates to faster loading pages and less browser load.

This update was a big step towards making Mist.io faster and accesible by any device. We’re also rolling out some pretty neat features, so stay tuned for some more news from us.

**If you haven’t tried Mist.io out recently, [now is a great time to do so](https://mist.io).**

