---
layout: post
title: Migrating from ScaleXtreme to Mist.io
date: '2014-05-15T18:07:00+03:00'
tags:
- scalextreme
- monitoring
tumblr_url: https://blog.mist.io/post/85822051951/migrating-from-scalextreme-to-mistio
---
A week ago, [ScaleXtreme](http://venturebeat.com/2014/05/07/citrix-gets-cloud-management-tech-with-the-acquisition-of-scalextreme/) was acquired by Citrix and they announced that they will be shutting down their product. Existing ScaleXtreme customers, it appears, have 45 days from the date of the transaction to find substitute services. If you were using ScaleXtreme to manage and monitor your server, Mist.io will be a perfect fit for you.

Mist.io offers server management across multiple clouds, detailed near real-time monitoring (10-second interval) and actionable alerts, all through a touch-friendly interface accessible from any web device. If you were using scaleXtreme’s service, drop us an e-mail or take advantage of our [15-day free trial](https://mist.io) and see for yourself.  
  
Setting up monitoring and alerting is only five steps away:

**Step 1: Registration**

Head over to&nbsp;[https://mist.io](https://mist.io)&nbsp;and register on the service. Once you fill in the form, you’ll get a confirmation e-mail. Click on the confirmation link, and you’ll be transferred back and logged in to Mist.io.

![image](/assets/tumblr-images/tumblr_inline_n1nizeL6v21rgqrs8.jpg)

**Step 2: Adding your servers**

If you’re adding cloud based servers, you’ll need your API keys or your username and password, depending on the provider. If you want to add a bare metal server, you’ll need the server’s IP and your username. &nbsp;If you are not sure where to find them, we’ve put together&nbsp;[instructions](https://mistio.zendesk.com/hc/en-us/sections/200064458-Backends)&nbsp;for each provider to help you.

![image](/assets/tumblr-images/tumblr_inline_n1l9efjCPR1rgqrs8.jpg)

To add your servers, click on the Add Backend button, choose your provider (or bare metal server) and fill in the details.

**Step 3: Making your server connectable by Mist.io**

There are two ways to enable monitoring in MIst.io. The easiest way is to provide SSH access to your server. If SSH acess is not possible or desirable, you can choose to manually install the collectd monitoring agent.

**a) Providing SSH access:**

![image](/assets/tumblr-images/tumblr_inline_n1l9gi37A61rgqrs8.jpg)

Mist.io needs to connect to your servers using SSH in order to set up monitoring and enable the web, touch-friendly console. Head over to the keys section and click on the Add button to set up your keypairs. If you already have a private SSH key that you use to connect to the server, you can import it in Mist.io by pasting your private key on the text dialog or by uploading the key file. Otherwise, you can generate a new SSH key through Mist.io and copy the public key to your server. Once you have a keypair set up, Mist.io can deploy it automatically on any new servers created through the service.

**b) Manually installing the agent:**

In some cases however, providing SSH access to Mist.io is not possible. You can choose to manually install the&nbsp;[collectd&nbsp;](http://collectd.org/)monitoring agent. If you try to enable monitoring on a server without SSH access, a dialog will pop up with instructions on how to install the agent manually. Just copy and paste the command on your server.

![image](/assets/tumblr-images/tumblr_inline_n3cu4eiT4Z1rgqrs8.jpg)

**Step 4: Enabling monitoring**

**![image](/assets/tumblr-images/tumblr_inline_n1l9gwizwT1rgqrs8.jpg)**

Once your server is connectable by Mist.io, you’ll see a blue bar&nbsp;appearing next to it on your machine list, indicating your server’s load. Click on the machine to go to the detailed view and then on the Enable button in the Monitoring section. Mist.io will then install the collectd open source agent and let you know once it is done.

**Step 5: Setting up alerting**

Once monitoring is enabled, a graph will appear. Click on the Add Rule button just below the graph to set up your first alerting rule.&nbsp;E.g. if you want to get an email once the load exceeds 4, your rule should look like this:

![image](/assets/tumblr-images/tumblr_inline_n1njnakSwi1rgqrs8.jpg)

Assuming you have a process that’s leaking memory, you can set up Mist.io to restart that process once your memory usage exceeds 95%.

&nbsp; ![image](/assets/tumblr-images/tumblr_inline_n1l9lbiCJh1rgqrs8.jpg)

Thats it. You’ve successfully set up monitoring for your server and added your first alerting rules.

**Do you have machines across clouds and want to easily manage them?&nbsp;[Sign up for Mist.io and try it for free!](https://mist.io/)**

