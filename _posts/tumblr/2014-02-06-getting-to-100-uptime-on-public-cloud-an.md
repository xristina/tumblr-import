---
layout: post
title: Getting to 100% uptime on public cloud. An interview with Telemachus Luu, CTO
  @ NephoScale
date: '2014-02-06T22:19:31+02:00'
tags:
- iaas
- nephoscale
tumblr_url: https://blog.mist.io/post/75804062301/getting-to-100-uptime-on-public-cloud-an
---
Two weeks ago, we promised a series of interviews with cloud providers &nbsp;in order to&nbsp;get a glimpse of what’s in store in the rapidly changing IaaS market. We kicked off [the series](http://blog.mist.io/2014-01-21-paas-is-a-blessing-not-a-curse-an-interview-with) with John Edgar,&nbsp;DigitalOcean’s Chief Technology Evangelist and today we are posting an interview with Telemachus Luu, the CTO of [NephoScale](http://nephoscale.com/). Telemachus is an all around great guy with deep technical and business knowledge. We know him since our Mozilla WebFWD days. All this time he has helped us out a lot with his advice and insights so we’re very happy to hear from him today.

![image](/assets/tumblr-images/tumblr_inline_n0ii9cHKyv1rgqrs8.jpg)

**Businesses have been feverishly moving their infrastructure to public clouds. What should they pay attention to?**

Three of the biggest complaints that we get from customers about cloud infrastructure providers is 1) uptime and reliability 2) predictability in performance and 3) transparency in billing.

There’s a misconception that moving to public clouds automatically translates to 100% service uptime. This simply isn’t the case. Customers should work with their providers to understand how to architect their infrastructure to achieve 100% uptime, whether that’s through database replication or global load balancing or through some other means.

Moving your infrastructure to public cloud providers also means a high probability of being on multi-tenant infrastructure. Between network, IO, CPU, and RAM contention, there’s a high likelihood that customers will experience some variability in performance. Again, you can build an infrastructure to account for these variances but you should work closely with your provider to ensure that you mitigate any impact caused by these variances.

Lastly, as the trend toward usage based billing accelerates, some providers are beginning to break out infrastructure and service components into additional billable artifacts. Hence, we get many complaints of “hidden” charges and the inability of the customer to forecast ongoing expenses. So customers need to pay close attention to this.

**There are several IaaS providers out there. What is unique in what NephoScale offers?**

Our existing customers love us because of our hi-touch engagement. We are all about the customer and ensuring they are happy. From the initial pre-sales engagement, onboarding, to ongoing support, our customers are very pleased with the end-to-end interaction with NephoScale’s staff. We work directly with our customers and give them the benefit of our experience with moving applications to the cloud.

We are also one of the few providers that offer on-demand bare-metal servers. I’m talking about no hypervisor, single-tenant, and highly customizable servers. Many of our customers run bare-metal servers with SSD drives, PCIe flash cards, and GPU cards. They love that they can provision and manage their servers through our web-based portal or through our API.

We also allow customers to provision and manage their service through 3 different mechanisms. Most providers only provide a customer portal and/or API. In addition to those two mechanisms, NephoScale also provides a template based solution called CloudScript. You can think of CloudScript as an orchestration-based provisioning engine that allows you to define entire datacenter configurations using a single text-file. Our customers and partners use CloudScript to deploy multi-server environments within minutes with a single click of a button.

**What setups are currently popular and what do you think will gain popularity in the future? (eg hybrid, multicloud, bare metal + cloud machines)**

Our top customers all have hybrid compute environments. Unlike hybrid cloud, hybrid compute is the use of both virtual machines and bare-metal servers working in concert. A typical setup is leveraging virtual machines for web/app layer and leveraging bare-metal servers for databases. An advantage of using NephoScale’s platform is that all virtual servers and bare-metal servers exist on the same broadcast domain using NephoScale’s own SDN technology. In addition, users can create custom server images and they can be used on both virtual and bare-metal servers interchangeably.

In the future, I think that supporting workload migrations across disparate cloud providers, on-premise environments, and hybrid compute environments will be necessary. At NephoScale, we are developing technology that supports the concept of on-prem/off-prem, public/private, and virtual/dedicated environments. Customers want to seamlessly move workloads between all of these types of environments and use a single interface to manage that experience.

**What do you see as the major challenges when it comes to increasing interoperability across IaaS solutions? Is this a priority for NephoScale? Why so?**

It’s clear that our industry is very fragmented when it comes to interoperability. As a group, we can attempt to work on open-standards to mitigate some of this fragmentation. But ultimately, I think that companies that build technology that provides an abstraction above the infrastructure providers will win.

The reality is, the manufacturers of different cloud enablement solutions and different hypervisors all have their own agenda. So companies that can build drivers, adapters, and toolkits that can facilitate mobility between infrastructure providers will do very well.

**What do you think about the growing popularity of PaaS solutions? Do they constitute a threat or a boost to IaaS?**

I think that most IaaS companies see PaaS solutions as a threat. At NephoScale, we embrace PaaS and have customers that run CloudFoundry, OpenShift, etc on top of our servers. We also partner with a few PaaS providers that leverage our API to scale-out their public PaaS offerings.

**What are NephoScale’s plans for the future? What features and enhancements should we expect to see?**

At NephoScale, we’re focused on three main themes.

We’re always striving to deliver solutions that allow our customers to deploy complex multi-server multi-stack environments with ease. Hence, you can expect more development and enhancements tailored to the deployment experience within our portal, the CloudScript engine, and our Marketplace library of one-click CloudScript template installs.

As I mentioned above, we’re also very focused on delivering solutions to facilitate the migration from on-prem private to off-prem private/off-prem public. One of the biggest pain points is the lack of centralized management of all your disparate resources so you can expect NephoScale to add more functionality in that area.

Finally, we see a real vacuum in terms of truly enterprise-grade high performance capability in the utility cloud space. We’re currently prototyping new zone configurations in our datacenters on both coasts to deliver our customers the very latest in high bandwidth, low latency, and high IOP nodes for the most demanding big data workloads. This usage tier has been traditionally relegated to bare-metal environments and would benefit greatly from the scalable model that utility clouds offers.

**What’s your favorite OS and text editor?**

Ubuntu and vim. On Windows (yes I know I know, I’m an Excel junkie), I use gedit.

[![image](/assets/tumblr-images/tumblr_inline_n0iii3wkyB1rgqrs8.jpg)](http://nephoscale.com/)

**Are you using NephoScale? Mist.io can help you manage and monitor your virtual machines from anywhere.&nbsp;[Try it for free](https://mist.io/).**

