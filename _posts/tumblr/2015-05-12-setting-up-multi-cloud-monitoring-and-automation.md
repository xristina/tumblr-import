---
layout: post
title: Setting up multi-cloud monitoring and automation in five minutes
date: '2015-05-12T15:12:38+03:00'
tags:
- monitoring
- cloud management
- multicloud
tumblr_url: https://blog.mist.io/post/118774980111/setting-up-multi-cloud-monitoring-and-automation
---
With the cloud era upon us and new technologies like Docker emerging fast, the IT landscape has drastically shifted the past years. Infrastructure flexibility is increasing, but so does the cost of managing several different technologies on a daily basis. And while the increased flexibility can potentially mean more uptime, it is of paramount importance to be able to keep an eye on all the moving parts underneath.

Whether your infrastructure consists of a couple of servers, Docker containers, public or private clouds, monitoring it effectively can increase uptime and reduce costs. And while there are open-source tools monitoring tools like Nagios, setting them up and maintaining them is complicated and costly.

We’ve faced these problems as engineers several times in the past and that is the reason we created Mist.io. A one-stop shop for infrastructure management and monitoring that is as easy as possible to set up, and uses the open-source collectd daemon to gather data from your server. No complicated setups, no closed-source monitoring agents or overpriced solutions.

Let us look at a typical example of what it takes to set up monitoring with Mist.io: We’ll use one of the most common providers, Amazon EC2, but the steps are similar for the rest. If you use a different provider, or want to monitor bare metal servers or Docker containers, you can refer to the [relevant docs](https://mistio.zendesk.com/hc/en-us/sections/200064458-Backends) on how to add them in Mist.io.

First things first, you’ll need to register to [https://mist.io](https://mist.io) and confirm your email account. Don’t worry, no credit card is required. Once you set your password and log in, you’ll see the Mist.io app.

<figure data-orig-width="425" data-orig-height="182" class="tmblr-full"><img src="assets/tumblr-images/tumblr_inline_no8jx59JIJ1rgqrs8_540.png" data-orig-width="425" data-orig-height="182"></figure>

Start by adding your first cloud. Once you’ve clicked the “Add cloud” button you will be asked for your EC2 API keys. In order to obtain them, you need to login to your [Amazon console](https://console.aws.amazon.com/console/home). On the top right select your user name and choose ‘Security Credentials’ from the drop down menu.

<figure class="tmblr-full" data-orig-height="255" data-orig-width="354"><img src="assets/tumblr-images/tumblr_inline_no8k4eRCEn1rgqrs8_540.png" data-orig-height="255" data-orig-width="354"></figure>

Select 'Access Keys’ and create a new Access key.

<figure class="tmblr-full" data-orig-height="226" data-orig-width="639"><img src="assets/tumblr-images/tumblr_inline_no8k4xhok71rgqrs8_540.png" data-orig-height="226" data-orig-width="639"></figure>

Copy the API key and API secret and paste them on the Add cloud form in Mist.io.

<figure class="tmblr-full" data-orig-height="549" data-orig-width="320"><img src="assets/tumblr-images/tumblr_inline_no8k8gXKqI1rgqrs8_540.jpg" data-orig-height="549" data-orig-width="320"></figure>

Once you click the Add button, Mist.io will try to contact EC2 and verify the credentials. If everything checks out, you will see all your virtual machines listed in the Machines section. With the cloud in place, it is time to set up access for Mist.io. There are two ways to do that. If you already have SSH keys that you use to connect to your servers, you can import them. Otherwise, you can create new keypairs through Mist.io and upload the public keys to your servers manually. Lets assume you already have access to your servers via SSH as that is the most common scenario. Moving to the Keys section, click on the Add button.

The Add key form gives you the option of generating a new keypair, uploading a file or pasting the RSA key directly. Lets paste the contents of our RSA key and click on the Add button.

<figure data-orig-height="354" data-orig-width="284"><img src="assets/tumblr-images/tumblr_inline_no8kgx7mbO1rgqrs8_540.png" data-orig-height="354" data-orig-width="284"></figure>

Your new key will appear on the Keys list. If you only have one key, Mist.io will list it as the default one, which means that it will first try to connect to your machines with that key, unless otherwise specified. Now, go back and visit your Machines section. Click on one of your servers. If Mist.io has managed to probe the server succesfully, the Shell button at the bottom will be active. Click on the Enable monitoring button.

Mist.io will install the collectd agent on your server. Once it is finished you should see your first monitoring graphs and after a while the first data should start coming in.

**Do you have machines across clouds and want to easily manage them? [Sign up for Mist.io and try it for free!](https://mist.io/)**

