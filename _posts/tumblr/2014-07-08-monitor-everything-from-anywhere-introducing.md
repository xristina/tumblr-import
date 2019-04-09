---
layout: post
title: Monitor everything from anywhere! Introducing custom metrics through Mist.io
date: '2014-07-08T20:27:00+03:00'
tags:
- cloud computing
- monitoring
tumblr_url: https://blog.mist.io/post/90766645046/monitor-everything-from-anywhere-introducing
---
Since its very first version, Mist.io has been providing alerting, automation and real-time analytics based on the standard system metrics of any server. These include CPU, network, disk & memory usage and they are good enough for most use cases, but you often need extra focus. You need to monitor the latency of your API, the number of requests processed by your web server, the performance of your database, or some entirely custom metric produced by your application. We decided to make the process of setting up such metrics as straightforward as possible and proudly achieved a major milestone!

![image](/assets/tumblr-images/tumblr_inline_n8elhpJLA41rgqrs8.png)

You can try it out with just a few clicks. Go to your server’s page and click on “Add Graph”. You’ll see a popup containing a nested list with all the metrics currently available to the Collectd monitoring agent. Clicking on a metric will automatically add a new graph and you’ll be able to add rules that trigger alerts or automated responses depending on the metric’s values.

Is your server a multi-user environment that provides ssh or scp access to multiple users? You can easily track how many users have logged in by adding a users graph. Do you need to know when the network connection becomes flaky? Just add a ping (latency) and a ping drop rate graph.

You can also use one of the&nbsp;[available Collectd plugins](https://collectd.org/wiki/index.php/Table_of_Plugins). Once you install them on your server, they’ll become available through the “Select Metric” popup. Or you can write your own Python script to monitor a metric. The following code for example will monitor the load time of your home page.

![image](/assets/tumblr-images/tumblr_inline_n8elim8tbu1rgqrs8.png)

Clicking Deploy, will add a new graph displaying the load time of [https://mist.io](https://mist.io)

![image](/assets/tumblr-images/tumblr_inline_n8elxh6f4D1rgqrs8.png)

It’s pretty awesome to be able to take a look and see how your website and network performed the past week. It’s even more important to get notified right away once things start getting out of the accepted norms. So, after adding the ping graphs, scroll down to the rules section and click on “Add Rule”. Change the metric on the new rule to “Ping droprate” and set it up to alert you once it‘s losing over 5% of ping replies. You can also add a similar rule to be alerted when the ping latency between your server and Google’s public DNS server (8.8.8.8) exceeds 20ms.

![image](/assets/tumblr-images/tumblr_inline_n8elm2jYB61rgqrs8.jpg)

Mist.io will alert you by default if it can no longer communicate with your server and these two rules will also let you know if your network performance degrades.

There is much much more you can monitor now. We have barely scratched the surface of what our new monitoring system can do. Stay tuned, as next week we’ll be posting some powerful examples on how to use it to keep track of your server and applications performance by adding your own, custom metrics.

In the meantime, feel free to play around and if you have any questions, send them over to [support@mist.io](mailto:support@mist.io). We’ll be happy to answer them.

**Do you have machines across clouds and want to easily manage them?&nbsp;[Sign up for Mist.io and try it for free!](https://mist.io/)**

