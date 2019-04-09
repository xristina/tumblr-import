---
layout: post
title: 5 Monitoring and Logging Best Practices in a Multi-Cloud Environment
date: '2015-04-14T09:37:11+03:00'
tags:
- multicloud
- logging
- monitoring
- devops
tumblr_url: https://blog.mist.io/post/116366091166/5-monitoring-and-logging-best-practices-in-a
---
_by [Dimitris Moraitis](https://twitter.com/d1_mo), CTO @ [Mist.io](https://mist.io)_

Having a complex application within a multi-cloud environment means having to manage and maintain the diversity that comes along with hybrid computing configurations. Modern, diverse environments have increased infrastructure flexibility, but have raised practical challenges like effectively monitoring and controlling external systems and services, or managing the enormous amount of logs that come from different sources. The question is, how can you build monitoring and governance systems to alert you, then configure them to trigger actions when needed? In this post, we’ll share a few practices that can help you juggle data from multiple providers.

Sometimes, even the best cloud providers fail and their services degrade. In addition, there is always the probability of an unexpected spike in an application’s popularity. As a result, planning for failover and autoscaling is necessary. Modern infrastructure environments have become more diverse, and managing an array of virtual machines and containers across public and private clouds is the price to pay for that diversity. The first step towards regaining control of your systems involves detecting issues, ideally in real time. Once you have a way to monitor your stack effectively, you can start building an automated response mechanism.

## **Multi-Cloud Logging and Monitoring in 5 Steps**

## 1. Identifying Metrics and Events

The first basic step in building your cloud governance strategy involves carefully identifying the metrics you need to collect. While it may seem trivial, some IT organizations will just start monitoring everything, when not everything is actually of use. To start, it is important to collect system metrics, such as CPU, disk usage, network usage, and so on. Then, collect the application generated metrics that matter to your bottom line, along with the ones that may hint towards incidents and respective action points. These may include the number of API requests served, the number of new or active users, the number of failed logins, or even the revenue generation rate of your application. You may also want to measure other external metrics, such as the reliability of network links, temperatures in specific locations or even currency exchange rates, but it’s important to not get too measuring happy. Some of these metrics may need to be aggregated from server groups that span across clouds. In these cases, the monitoring solution of any single cloud provider will not suffice.

## 2. Set Up Rule-Based Governance

Create a list of rules to enforce. For example, when the average load composite metric in your application server group goes over a threshold, your cloud management system should automatically provision a new virtual machine, bootstrap it as a new application server and add it to your load balancer. Similarly, when a load goes beneath a certain threshold, one of those servers should automatically be terminated so that you don’t spend more than you need. In a multi-cloud environment, migration automation is crucial. For example, if you need to abide by a customized SLA, it may include specific expectations regarding user response times. As a result, it would be up to you to continuously monitor your different providers’ performance. That way, if service degradation occurs, you can automatically trigger a switch to another provider.

## 3. Centralize Your Monitoring Data 

All of your logs, events, and metrics should be kept in one place, independent of your main IaaS provider. This is necessary if you want to maintain good data visualization with a birdseye view, and make sure that everything is running properly. In addition, monitoring your data in a centralized location enables you to better manage access, reliably detect intrusions, equip your developers with the flexibility to debug coding errors, and maintain a closed production environment.

## 4. &nbsp;Test Your System’s Self-Healing Capabilities 

Trigger events yourself, simulate loads and events, and make sure you receive alerts in order to optimize priorities and actions. In order to have confidence that your system will be able to react in the event of an outage, it is important to run periodic drill down tests. One of the most popular examples of this kind of service is [Netflix Chaos Monkey, offering](http://techblog.netflix.com/2012/07/chaos-monkey-released-into-wild.html)a self-healing test where machines are randomly shut down so that the tested system can recover on its own. A self-healing, automated approach is key if you have a multi-cloud, distributed system.

## 5. Postmortem

As in every type of modern IT environment, you should break your system down into small parts, refining them as you go. Make sure to rinse and repeat, and treat every incident as a chance to learn and improve your cloud governance. Continuously identify and refine the current state of your logs and monitoring capabilities. Remember that every incident is a chance to get rid of false positives or automate your responses towards recurring issues. Additionally, since you are responsible for overall user experience, it is critical to understand which part of your infrastructure failed and why, even if that part is hosted externally.

## **Final Notes**

Multi-cloud environments are gaining more traction every day, because their users don’t want to be locked into a specific vendor or limit their capabilities. In a modern and more demanding environment, it’s important to monitor and log every aspect of an application, including data from different IaaS providers. To do this, you need the ability to receive, digest, and act on a lot of information quickly. Treating operations as a significant part of your business, along with automation and having the right tool set, will increase your customers’ level of trust and enable success.

