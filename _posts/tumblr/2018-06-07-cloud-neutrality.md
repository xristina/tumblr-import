---
layout: post
title: The case for cloud neutrality
date: '2018-06-07T00:00:00+03:00'
tags:
- cloud
- neutrality
tumblr_url: https://blog.mist.io/post/176018246846/cloud-neutrality
---
Computing is the fuel of AI. We need so much more of it and we have to become way more efficient in producing and distributing it.

**Clouds want to lock you in…**

<figure data-orig-width="800" data-orig-height="350" class="tmblr-full"><img src="assets/tumblr-images/tumblr_inline_pc278kSgTz1rgqrs8_540.jpg" alt="image" data-orig-width="800" data-orig-height="350"></figure>  

Public cloud providers have been frenetically developing new features and services. Most of those generate value by solving real world problems. Quite often, these add-ons are provided for free, which is wonderful, but there’s a catch: The overwhelming majority of these awesome products can only use computing power from that same cloud provider. Oh my!

Maybe this doesn’t sound so unreasonable. Isn’t it natural for a company’s products to fit well together? After all, this tight coupling can be beneficial to the end user despite some level of lock-in. For example, coffee capsule machines are not as rare as most gourmet coffee addicts would expect. They’re admittedly less messy than grinding your own beans. On the other hand, would you choose a refrigerator that only accepts food products of a single brand? Clearly, a line must be drawn somewhere.

When it comes to computing, it’s up to each organization to draw that line. If your workloads are small and predictable, perhaps optimizing your cloud neutrality isn’t worth the extra effort. At the same time, many organizations will go to great lengths to reduce their dependence on proprietary software and services that are not sufficiently commoditized.

This fact has been leveraged by Google in the ongoing cloud wars. They launched Kubernetes as an open source project, which quickly became the de-facto standard for orchestrating containers. Amazon, Microsoft & Docker tried to compete but were eventually forced to acknowledge the dominance of Kubernetes. Now they’re trying to catch up with Google as to who will build the best walled garden around it, as always constraining the compute resources one can add to it.

On another front of that same war, Microsoft acquired Github while Google is funding Gitlab. Both tech conglomerates are employing the very best strategies available to their deep pockets. All that jazz, just to win the hearts of the developers of this world and, more importantly, their computing workloads.

The machine learning / AI revolution is raising the stakes and the complexity involved. A lot of workloads are mostly stateless and idempotent, but often require GPU’s or new architectures like TPU’s and, soon enough, neuromorphic chips. Every new feature that makes a difference will be leveraged to tighten the lock-in. Is there a viable defense strategy to that fierce technological jiu jitsu?

**Turn them into mist!**

<figure data-orig-width="800" data-orig-height="368" class="tmblr-full"><img src="assets/tumblr-images/tumblr_inline_pc278tqKL01rgqrs8_540.jpg" alt="image" data-orig-width="800" data-orig-height="368"></figure>  

We’d like to help ignite the upcoming AI explosion without getting further locked in any cloud. This is why we built [Mist](https://mist.io/). Our mission is to perfect the tools that will streamline the process of provisioning, operating and distributing computing resources from any provider or platform.

We’re committed to provide these tools as Free and Open Source software. Thus, the vast majority of our code is included in the [Community Edition](https://github.com/mistio/mist-ce) of the Mist Cloud Management Platform.

In order to finance this effort, we’ve also launched a set of commercial offerings that enhance the base platform with features requested by our Enterprise users. These include tools for optimizing spending, regulating access, detecting anomalies and reselling excess resources. These are all available as part of the [Hosted Service](https://mist.io/) and the [Enterprise Edition](https://mist.io/get-started).

We’ve gone a long way in the last 5 years, but our work is far from complete. A growing number of users of all shapes and sizes depend on our software and services and the platform has matured significantly and broadened its scope in [Mist v3](https://github.com/mistio/mist-ce/releases). But let’s be honest. It’s still an infant. There are so many use cases that we’re not yet supporting but cannot afford to ignore if we’re serious about democratizing cloud computing.

Some of the areas that we’re starting to engage include i) cost based auto-scaling for Kubernetes clusters across clouds and ii) producing increasingly intelligent recommendations that aspire to evolve into an autopilot for DevOps.

If you think you have a multi-cloud use-case that Mist cannot yet address, please let us know. We love being challenged, especially when it’s about breaking virtualized chains.

**Clouds want to lock us in, let’s turn them into mist!**

_Photo by donabelandewen@flickr CC BY 2.0_

_This post was originally published by Dimitris Moraitis, Mist.io’s co-founder & CTO, on [Quora](https://cloudneutral.quora.com/The-case-for-cloud-neutrality-1)._

