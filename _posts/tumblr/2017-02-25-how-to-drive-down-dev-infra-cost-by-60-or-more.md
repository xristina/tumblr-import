---
layout: post
title: How to drive down dev infra cost by 60% (or more)
date: '2017-02-25T03:59:55+02:00'
tags: []
tumblr_url: https://blog.mist.io/post/157671715281/how-to-drive-down-dev-infra-cost-by-60-or-more
---
I will show you how to use Mist.io’s cloud scheduler to automatically stop VMs during non-business hours&nbsp;to avoid paying for VMs that are not actively being used.

You can think of the cloud scheduler as a programmable on/off/destroy switch for your VMs – in our use case it dynamically turns them off on a set schedule.&nbsp;

Let’s get started…

## **Step 1** - Start by adding a cloud(s) and auto-discovering your VMs.&nbsp;

Skip this step if you already have clouds in your Mist.io account.

1. Log in to your Mist.io account
2. Click [Add Cloud](http://docs.mist.io/category/75-adding-clouds-bare-metal-and-containers)

Mist.io will automatically fetch and list all your VMs across all your cloud environments to give you an inventory of your resources.

## **Step 2** - View cost and usage reports.

Next, view the cost and usage reports to spot waste. To view the reports:

1. Click on Insights (last option on the side navigation bar)  
2. Use the cost, inventory, and usage reports to understand your usage patterns and identify VMs that can be powered off.
3. Note: Insights is currently in a private beta. Contact Mist.io support to enable your account.
<figure data-orig-width="1122" data-orig-height="453" class="tmblr-full"><img src="assets/tumblr-images/tumblr_inline_olwnlfjX611rgqrs8_540.png" data-orig-width="1122" data-orig-height="453"></figure>
## **Step 3** - Create a schedule to stop VMs during non-business hours and stop burning cash.  

1. Click on Schedules&nbsp;
2. Click [Add Schedule](http://docs.mist.io/article/151-scheduler)  
3. Complete form and save

Schedules can be applied to either specific VMs or to tags.&nbsp;

<figure data-orig-width="676" data-orig-height="567" class="tmblr-full"><img src="assets/tumblr-images/tumblr_inline_olwnyvoR7r1rgqrs8_540.png" data-orig-width="676" data-orig-height="567"></figure>

Now the scheduled task will stop all VMs on the set schedule you created. On most public clouds when you stop a VM you don’t have to pay for it. The scheduler makes it really easy to stop machines across all your public and private clouds and, hopefully, drastically reduce your monthly costs associated with non-production VMs.

The scheduler works with AWS, Azure, GCE, IBM/SoftLayer, Rackspace, DigitalOcean, KVM, VMware, OpenStack, and more.

