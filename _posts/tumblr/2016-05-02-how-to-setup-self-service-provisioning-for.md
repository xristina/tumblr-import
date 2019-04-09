---
layout: post
title: How to setup self-service provisioning for multi-clouds
date: '2016-05-02T18:53:54+03:00'
tags: []
tumblr_url: https://blog.mist.io/post/143742943766/how-to-setup-self-service-provisioning-for
---
In this tutorial, I will explain how to setup self-service provisioning on public and/or private clouds for your engineers.&nbsp;

Your engineers will be able to use the Mist.io interface to dynamically provisioning and configure machines. You, as the administrator, will have control and visibility to set policies / permissions and manage usage across environments.

_In most cases, setting up self-service provisioning should not take more than 20 minutes._

My assumptions about your scenario:

- You have a software / engineering team  
- You want to allow your engineers to dynamically create and configure machines across heterogenous infrastructure
- You want visibility and control to ensure effective use of resources

Let’s get started!

## **Step 1 - Add Clouds**

Log in to your Mist.io account. &nbsp;Add the “Clouds” you want your engineers to provision machines on.&nbsp;A “Cloud” can be a public or private cloud, a hypervisor, bare-metal server, or even a container.

<figure data-orig-width="282" data-orig-height="466"><img src="assets/tumblr-images/tumblr_inline_o6ewactRFR1rgqrs8_540.png" alt="image" data-orig-width="282" data-orig-height="466"></figure>

When you are done adding “Clouds”, they appear at the top of the Dashboard.

<figure data-orig-width="1250" data-orig-height="238" class="tmblr-full"><img src="assets/tumblr-images/tumblr_inline_o6ezd2Qpir1rgqrs8_540.png" alt="image" data-orig-width="1250" data-orig-height="238"></figure>

## **Step 2 - Create an Organization**

In the upper right hand corner, click on the User Settings icon” Click on &nbsp;“Add Organization.”

<figure data-orig-width="181" data-orig-height="285"><img src="assets/tumblr-images/tumblr_inline_o6ezidnCnx1rgqrs8_540.png" alt="image" data-orig-width="181" data-orig-height="285"></figure>

Give your Organization a name.

<figure data-orig-width="290" data-orig-height="208"><img src="assets/tumblr-images/tumblr_inline_o6ezjlUYxj1rgqrs8_540.png" alt="image" data-orig-width="290" data-orig-height="208"></figure>

Now that you have created an organization, you now need to create a&nbsp;“Team” and invite users.

## **Step 3 - Create a Team**

From the Dashboard, click “Teams.”

<figure data-orig-width="586" data-orig-height="276" class="tmblr-full"><img src="assets/tumblr-images/tumblr_inline_o6ezq2i0yS1rgqrs8_540.png" alt="image" data-orig-width="586" data-orig-height="276"></figure>

Click “Create Team.”

<figure data-orig-width="1080" data-orig-height="138" class="tmblr-full"><img src="assets/tumblr-images/tumblr_inline_o6f03aJGbW1rgqrs8_540.png" alt="image" data-orig-width="1080" data-orig-height="138"></figure>

Give the Team a name.

<figure data-orig-width="278" data-orig-height="320"><img src="assets/tumblr-images/tumblr_inline_o6f007xRGB1rgqrs8_540.png" alt="image" data-orig-width="278" data-orig-height="320"></figure>

This Team will belong to the Organization you created earlier. You can create many Organizations and Teams. This [blog post](http://blog.mist.io/2016-04-12-rbac-private-beta) provides a complete overview of the RBAC feature and capabilities.

## **Step 4 - Create a Policy**

Use the “Policy” engine to create permissions. The permission will be applied at the Team level. All Members of the Team will inherit the permissions.

##   

<figure data-orig-width="1202" data-orig-height="697" class="tmblr-full"><img src="assets/tumblr-images/tumblr_inline_o6k4vwm8uI1rgqrs8_540.png" alt="image" data-orig-width="1202" data-orig-height="697"></figure>
##   

## **Step 5 - Invite Members&nbsp;**

Invite&nbsp;“Members” to join the Team. Members will receive an email with instructions for joining the team.

<figure data-orig-width="1204" data-orig-height="442" class="tmblr-full"><img src="assets/tumblr-images/tumblr_inline_o6f0m6fBDt1rgqrs8_540.png" alt="image" data-orig-width="1204" data-orig-height="442"></figure>

## **Your Members**

Your “Members” will receive an email with instructions for creating an account and joining the Team.

<figure data-orig-width="706" data-orig-height="316" class="tmblr-full"><img src="assets/tumblr-images/tumblr_inline_o6f265HHKB1rgqrs8_540.png" alt="image" data-orig-width="706" data-orig-height="316"></figure>

Members will create a Mist.io account.

<figure data-orig-width="503" data-orig-height="402" class="tmblr-full"><img src="assets/tumblr-images/tumblr_inline_o6f2i0pjAW1rgqrs8_540.png" alt="image" data-orig-width="503" data-orig-height="402"></figure>

Members see their Dashboard. Members can only perform functions that are allowed by the policy you created for the Team.

<figure data-orig-width="1184" data-orig-height="498" class="tmblr-full"><img src="assets/tumblr-images/tumblr_inline_o6k4pj0MeP1rgqrs8_540.png" alt="image" data-orig-width="1184" data-orig-height="498"></figure>

Members can create machines.

<figure data-orig-width="307" data-orig-height="286" class="tmblr-full"><img src="assets/tumblr-images/tumblr_inline_o6g9035yBF1rgqrs8_540.png" alt="image" data-orig-width="307" data-orig-height="286"></figure>

This post should give you a high level overview of how to get started with RBAC. In the next posts I will explain how to use our&nbsp;“Scripts” and&nbsp;“Template” features to automate the setup of single servers and even complete topologies and stacks.

