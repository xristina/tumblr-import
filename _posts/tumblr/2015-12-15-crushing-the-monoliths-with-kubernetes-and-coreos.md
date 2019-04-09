---
layout: post
title: Crushing the monoliths with Kubernetes and CoreOS
date: '2015-12-15T12:47:18+02:00'
tags:
- coreos
- kubernetes
- DevOps
- continuous integration
tumblr_url: https://blog.mist.io/post/135245723876/crushing-the-monoliths-with-kubernetes-and-coreos
---
When we started building [Mist.io](https://mist.io/), we needed to bring a working product to the market, and we needed it fast. Our goal was to build a cloud-agnostic, easy-to-setup tool that would take care of all your infrastructure needs: Management, monitoring, automation and orchestration. But for the prototype, all we wanted was to support the most famous clouds at the time (EC2, Rackspace, Linode and OpenStack) and some basic monitoring and alerting stuff.

Our codebase was small and manageable so our service was a bundled, monolithic python app. But as time went on, we kept adding new features and services and it was apparent that we needed to break our app into smaller services to keep it scalable and be able to test and deploy new features fast and painlessly. Additionally, as our product was aimed at infrastructure management, we wanted to keep an eye on new technologies that emerge in the DevOps world.

Take Docker for example. More than a year ago, while it was rising in popularity, we [added support for Docker containers](http://blog.mist.io/2014-09-03-one-ui-to-rule-them-all-manage-your-docker), allowing you to manage and monitor them alongside the rest of your infrastructure. While doing that we heavily integrated and used docker in our QA infrastructure and workflow, switching to multiple parallel environments [to speed up our test suite](http://blog.mist.io/2014-04-11-move-fast-and-dont-break-things-testing-with).

While we continued to make use of Docker with all its advantages (and disadvantages which are not in the scope of this post), we started breaking up our codebase in microservices. And while breaking up code into several services brings advantages in scaling and code management, it also brings its own obstacles. How should we orchestrate all our containers and achieve high availability (vital when building a monitoring service) without ending up with a solution more complicated than our own product?

<figure data-orig-width="800" data-orig-height="256" class="tmblr-full"><img src="assets/tumblr-images/tumblr_inline_nzeavuhKNY1rgqrs8_540.png" alt="image" data-orig-width="800" data-orig-height="256"></figure>

Fortunately, CoreOS soon entered the game. As we started adding support for it in Mist.io, we wanted to take advantage of the rolling updates and the awesome fleet service with etcd and built in Docker support. But even with fleet, we still had to provide our own solution for Docker orchestration and networking. And we didn’t want to end up with a gazillion fleet unit files and sidekicks.

<figure data-orig-width="560" data-orig-height="136" class="tmblr-full"><img src="assets/tumblr-images/tumblr_inline_nzeaynqVzh1rgqrs8_540.png" alt="image" data-orig-width="560" data-orig-height="136"></figure>

Then came along Kubernetes, promising to create order where container chaos existed. It was love at first sight and we set one of our DevOps teams to start working on a prototype. Soon, we had our first proof-of-concept cluster to start testing upon.

<figure data-orig-width="625" data-orig-height="337" class="tmblr-full"><img src="assets/tumblr-images/tumblr_inline_nzear3BWpD1rgqrs8_540.jpg" alt="image" data-orig-width="625" data-orig-height="337"></figure>

But such a fundamental change in our infrastructure came with its own set of issues. How will we keep developing our app now? Do we keep using the old way (buildout and virutalenv) or do we all have to keep kubernetes clusters in our laptops? And will we end up coding in a fundamentally different environment compared to production? What about pre-production? Is there an easy way to set up kubernetes dev environments without having to keep track of multiple different configuration files?

We also needed to change our CI workflow, build new Jenkins jobs, stress test our pre-production environment and get used to the new way of developing. Stop using tail -f everyone, it is now called kubectl!

We figured that these issues are more or less stuff that everyone interested in high availability and microservices will eventually stumble upon, so we documented our solutions. On the following days we will be publishing a series of blog posts that will cover everything from one-click installations of dev environments to CI workflows and production deployments. Stay tuned!

