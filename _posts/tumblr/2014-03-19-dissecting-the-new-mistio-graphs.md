---
layout: post
title: Dissecting the new Mist.io graphs
date: '2014-03-19T19:33:00+02:00'
tags:
- monitoring
- updates
tumblr_url: https://blog.mist.io/post/80079597818/dissecting-the-new-mistio-graphs
---
One of our core value propositions of Mist.io is server monitoring, and today we want to delve for a bit into our user-facing part of it: the monitoring graphs. Once you [enable monitoring](http://blog.mist.io/2014-02-27-setting-up-monitoring-and-alerting-for-your-servers-in)&nbsp;for a server through Mist.io, the service will start collecting data from your server (CPU, RAM, network and disk load) and start sending them over to the user interface which in turn will produce some nice graphs for you.

![image](/assets/tumblr-images/tumblr_inline_n2p1zjf5Aq1rgqrs8.jpg)

Mist.io is all about enabling you to learn about a crisis and respond as fast as possible. That is why the data resolution -the time interval between each data collection- is set to 10 seconds which is six times faster than the minimum data fetch interval on [Amazon CloudWatch](http://aws.amazon.com/cloudwatch/). Using a smallest interval, not only produces more accurate graphs but also enables us to respond faster when one of your server’s metrics exceeds the limit you have set in your rules.

![image](/assets/tumblr-images/tumblr_inline_n2p20xuKFU1rgqrs8.png)

Recently, we’ve redesigned our graphs user interface to make them easier to browse through and read. We’ve switched from horizon, to simpler and faster to read graphs that will give you an overview of your server load, in less time. When looking at the detailed server overview, there are now a number of options over the monitoring graphs. On the left, you have the arrow buttons that enable you to browse the past to pinpoint any server spikes. On the right, you have the option to zoom in and out by choosing if the graph will display data for the last 10 minutes, day, week or month.

Right below the bottom of the graphs you’ll find a row of buttons, each one representing a server metric (Load, Disk and Network usage). Clicking on one of them will display the corresponding graph. To hide a graph, just click on the minus button on its top right.

![image](/assets/tumblr-images/tumblr_inline_n2p21pEyL11rgqrs8.jpg)

One other aspect of Mist.io’s graphs that we’ve recently improved is the speed it takes to generate the graphs and the load it produces on the browser. We’re using the excellent [D3.js](http://d3js.org/)&nbsp;Javascript library to produce our graphs but the default implementation was a bit heavy due to the fact that we are updating the graphs every 10 seconds. We’ve optimized this procedure which resulted in significant speed increases, especially in cases where browser load is an issue (like phones and tablets).

As always, we’re eager to share our solutions with the community, so stay tuned for a more technical blog post regarding D3.js graph optimizations.

**Do you have machines across clouds and want to easily manage them?&nbsp;[Sign up for Mist.io and try it for free!](https://mist.io/)**

