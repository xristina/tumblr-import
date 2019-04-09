---
layout: post
title: Monitoring Amazon Virtual Private Cloud instances with Mist.io
date: '2014-06-16T10:15:00+03:00'
tags:
- aws
- monitoring
- cloud computing
tumblr_url: https://blog.mist.io/post/88938984326/monitoring-amazon-virtual-private-cloud-instances
---
Cloud servers are usually associated with publicly accessible virtual machines. But there are often cases where a server is better off running in a private, isolated section of the cloud. Database and test servers are common examples. Amazon provides Virtual Private Cloud (VPC) instances that run in an isolated section of the AWS cloud without a public IP. Network access control lists and security groups can be used to provide strict control over inbound and outbound network traffic to these instances.

Many monitoring systems require a public IP for the monitoring server to connect to the machine and get the monitoring data or check for any alerts. But with Mist.io, you can configure your VPC to send the monitoring data without the need for a public IP. All you need to do is allow outgoing traffic to 25826 UDP port.

As an example, we have set up a VPC instance with a security group that allows outgoing traffic in UDP port 25826 that we want to use as a database server.

![image](/assets/tumblr-images/tumblr_inline_n793lp2RIq1rgqrs8.png)

We can also allow incoming trafic through port 22 that will let Mist.io connect to the server through SSH to set up the monitoring agent, but that is not a requirement. If access through SSH is not available, Mist.io will prompt you with instructions on how to install the agent by running a simple script on the server.

![image](/assets/tumblr-images/tumblr_inline_n794clhJi31rgqrs8.png)

We will assume that our AWS backend is already added in Mist.io. Instructions on how to add it can be found [here](https://mistio.zendesk.com/hc/en-us/articles/200235718-Add-credentials-for-Amazon-EC2). Once we have created our VPC from Amazon’s interface, it will appear in our machines list.

![image](/assets/tumblr-images/tumblr_inline_n794etjkkG1rgqrs8.png)

We will click on the machine and then on the Enable (monitoring) button. If SSH access is allowed, Mist.io will go ahead and install the collectd monitoring agent. Otherwise it will prompt us with instructions on how to manually install it:

![image](/assets/tumblr-images/tumblr_inline_n792z89tkV1rgqrs8.jpg)

Once the agent is installed (either manually or automatically via SSH), monitoring is set up and we’re free to start adding our first alerting rules.

![image](/assets/tumblr-images/tumblr_inline_n793awE1Hq1rgqrs8.png)

That’s it. We have set up monitoring for our Amazon VPC and we’re ready to enable our first alerts. Our machine only allows outgoing traffic through a specific port and Mist.io will let us know if something is wrong with our database server.

**Do you have machines across clouds and want to easily manage them?&nbsp;[Sign up for Mist.io and try it for free!](https://mist.io/)**

