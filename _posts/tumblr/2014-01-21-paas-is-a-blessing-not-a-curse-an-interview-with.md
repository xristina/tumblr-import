---
layout: post
title: PaaS is a blessing, not a curse. An interview with DigitalOcean’s Chief Technology
  Evangelist, John Edgar
date: '2014-01-21T14:29:00+02:00'
tags:
- iaas
- digitalocean
- interview
tumblr_url: https://blog.mist.io/post/74055070351/paas-is-a-blessing-not-a-curse-an-interview-with
---
The last few years the IaaS market has seen a lot of competition with major players like Google, Microsoft and IBM joining a field dominated so far by Amazon, and new providers coming up to contest the traditional players.&nbsp;In order to get glimpse of what’s in store in this rapidly changing environment, we’re starting a series of interviews with key players.

Today, we are interviewing [John Edgar](https://twitter.com/jedgar), DigitalOcean's&nbsp;Chief Technology Evangelist.&nbsp;Launched in 2011, [DigitalOcean](https://www.digitalocean.com/) quickly became one of the fastest-growing cloud providers by focusing on simplicity, targeting developers and keeping the prices low. By the end of 2013 it surpassed even AWS in [month-to-month grow in web-facing computers](http://venturebeat.com/2013/12/24/digitaloceans-cloud-surpasses-amazon-web-services-in-one-category/).&nbsp;

![image](/assets/tumblr-images/tumblr_inline_mzr3ygVx6w1rgqrs8.jpg)

**1. Businesses have been feverishly moving their infrastructure to public&nbsp;clouds. What should they pay attention to?**

I think it’s really important to understand what you’re getting into. With “cloud” becoming the standard and infrastructure being fragmented out into individual offerings, it’s coming increasingly difficult to understand what exactly you’re getting and what exactly you need. I think they need to spend time really planning out what their infrastructure needs are and then match them to a provider. Understanding how your systems are going to operate together is still something really important when planning your cloud offerings. Elasticity is starting to become more and more popular though, providers with hourly bill rates really help with that. You should also obviously really pay attention to the physical location of your data centers.

**2. There are several IaaS providers out there. What is unique in what&nbsp;DigitalOcean offers?**

Most people are impressed by our price, and with good reason. Personally, I’m really excited about our ease of use. Your ability to create, move, and manage your VMs is greatly enhanced by our API. &nbsp;We find many people to be API users these days.

**3. What setups are currently popular and what do you think will gain&nbsp;popularity in the future? (eg hybrid, multicloud, bare metal + cloud&nbsp;machines)**

I think the future is probably pure “cloud” for most people, with dedicated gear for companies like Facebook and Twitter. To my mind, the cloud is just the next level of abstraction. git push some code to the “cloud” just works. The problem we face as an industry, is that we’ve not really figured out how to make these abstraction layers work together elegantly, from the application layer all the way down.

**4. What do you see as the major challenges when it comes to increasing&nbsp;interoperability across IaaS solutions? Is this a priority for DigitalOcean? Why so?**

Currently, there is, sadly, not much interoperability. At DO, we use AWS Glacier as our off site backup service. Right now our priories are really heavily set on organic growth. I’m not convinced that interoperability is really the thing we need to be focused on more as talking about what the standards are going to be in these new technologies and how to make them less fragmented.

**5. What do you think about the growing popularity of PaaS solutions?&nbsp;Do they constitute a threat or a boost to IaaS?**

I LOVE it! I think that PaaS is the bees knees and I don’t see it as a threat to IaaS at all. Infrastructure is very expensive and difficult, and not many people want to get involved with it. The way we consume infrastructure will change greatly over the next 14 months as we grow our cloud offerings and as js, go, rust etc. become more mature. At DO, we certainly want PaaS to know that we’re friends. We’re always really interested in knowing what platform companies are doing and how we can help them.

**6. What are DigitalOcean’s plans for the future? What features and&nbsp;enhancements should we expect to see?**

We’re really keen on keeping things super simple while we expand. This is why you haven’t seen any new product offerings from us for quite some time. Over the next eight months, we are going to be working on load-balancing/failover and object stores, as well as building out our infrastructure and upgrading our current locations.

**7. What’s your favorite OS and text editor?**

Been really enjoying Arch recently and I run it on my VMs. My personal computer is Os X and my text editor is Emacs, obviously! hehehe. I work in Sublime. :)

![image](/assets/tumblr-images/tumblr_inline_mzr3zyq4u11rgqrs8.png)

**Are you using DigitalOcean? Mist.io can help you manage and monitor your virtual machines from anywhere. [Try it for free](https://mist.io).**

