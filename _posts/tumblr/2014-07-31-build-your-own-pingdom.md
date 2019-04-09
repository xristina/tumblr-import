---
layout: post
title: Build your own Pingdom to measure load time from any location
date: '2014-07-31T19:26:00+03:00'
tags:
- pingdom
- cloud computing
- monitoring
tumblr_url: https://blog.mist.io/post/93413334586/build-your-own-pingdom
---
[Pingdom](https://www.pingdom.com/) is a popular and highly useful service that monitors your website and informs you if something goes wrong. But what if your users originate from different parts of the world? Depending on where your server is located, users from different parts of the world will get a very different experience. This is actually one of the [most requested](https://pingdom.uservoice.com/forums/113203-my-pingdom/suggestions/2223355-ability-to-choose-which-probe-servers-to-use) Pingdom features.

![image](/assets/tumblr-images/tumblr_inline_n9l3x21eqe1rgqrs8.png)

So, instead of relying on the location of a 3rd party service, why not use your own machines to check your website’s load time from different parts of the globe? The easiest way to do this is through Mist.io.

All you need to do is to add a [custom metric](http://blog.mist.io/2014-07-08-monitor-everything-from-anywhere-introducing-custom) on one or more servers and set up alerts. Mist.io will let you know once your website’s load time exceeds your specified threshold. If you aren’t already a happy Mist.io user, you’ll need to set up an account with Mist.io and enable monitoring on the server(s) you want to check your website with. It’ll just take a [couple of minutes](http://blog.mist.io/2014-02-27-setting-up-monitoring-and-alerting-for-your-servers-in).

Now, assuming you’ve enabled monitoring on your server, just click on “Add Graph” and you’ll see a popup containing a nested list with all the metrics currently available to the monitoring agent. Select “custom” to add a check for your site. The following snippet of Python code will monitor the load time of [https://mist.io](https://mist.io)

    import urllib, time
    def read():
      nf=urllib.urlopen('https://mist.io/')
      start=time.time()
      page=nf.read()
      end=time.time()
      nf.close()
      return end - start

![image](/assets/tumblr-images/tumblr_inline_n9l40rk4M81rgqrs8.png)

After a few minutes, our load graph will start getting populated with the results of our script.

![image](/assets/tumblr-images/tumblr_inline_n9l4a7sNBO1rgqrs8.png)

It’s pretty awesome to be able to take a look and see how your website performed the past week. It’s even more important to get notified right away once things start getting out of the accepted norms. Time to create a new rule, which will alert us when our load time exceeds one second. Scroll down to the rules section and click on “Add Rule”. Change the metric on the new rule to the one you just created (Mist load time in our example) and set it up to alert you once it‘s over 1 second.

![image](/assets/tumblr-images/tumblr_inline_n9l4727u7O1rgqrs8.png)

That’s it! We’ve set up our own, cheap monitoring solution for our website. The server we used is located at Rackspace’s ORD datacenter in Dallas, Texas which is handy if most of our visitors comes from the U.S. But if our audience is global, the load time from a U.S. server says little about the experience of users from other parts of the globe. All we have to do, is repeat the procedure on one of our servers in Europe or Asia and now we can monitor our website’s load time from all around the world. This is what our check from the Amazon EC2 data center in Ireland looks like:

![image](/assets/tumblr-images/tumblr_inline_n9l47suVSa1rgqrs8.png)

We can see that for our European visitors our website is somewhat slower, but within acceptable limits. If the performance degrades we’ll know it right away in order to take action.

**Do you have machines across clouds and want to easily manage them?&nbsp;[Sign up for Mist.io and try it for free!](https://mist.io/)**

