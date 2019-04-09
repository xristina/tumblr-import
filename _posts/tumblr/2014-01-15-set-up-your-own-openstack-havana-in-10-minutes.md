---
layout: post
title: Set up your own OpenStack Havana in 10 minutes
date: '2014-01-15T13:52:00+02:00'
tags:
- openstack
- opensource
- softlayer
tumblr_url: https://blog.mist.io/post/73402239574/set-up-your-own-openstack-havana-in-10-minutes
---
Supporting&nbsp;[OpenStack&nbsp;](http://www.openstack.org/)was a choice we made when&nbsp;[Mist.io](https://mist.io/)&nbsp;was still a prototype. Back then, OpenStack was notoriously difficult to install. Fortunately things got better and better over time. Now it is much easier and streamlined.

The following is not meant to be production-grade. It is just a way to quickly set up Havana, the latest version of OpenStack, for testing purposes. The steps should be similar for any environment though. We will use&nbsp;[DevStack](http://devstack.org/)&nbsp;to speed up and simplify our installation. DevStack is a shell script to easily build complete development environments.

After we’re done, we’ll end up with a working Havana deployment, the latest version of Openstack. We will also illustrate how to configure the network and ip auto-assignment so that you can start rolling out VMs.

We chose to install it on a bare metal server from&nbsp;[Softlayer](http://www.softlayer.com/)&nbsp;with the following specs:

- 1 Processor: Intel Xeon-SandyBridge E5-2620-HexCore [2GHz]&nbsp;
- 16 GB Ram
- Public ips in the form of 159.253.138.184/29. This way we ended up with a public ip and secondary public ips on the subnet
- OS: Ubuntu 12.04-64 minimal install

We decided to go with a bare metal server from Softlayer because we wanted to control the entire environment (CPU, I/O) and avoid noisy neighbors to further test OpenStack. The price/performance ratio is hard to beat so you get more out of your $$$. Softlayer is already integrated with Mist.io and we can provision servers really fast ;)

![image](/assets/tumblr-images/tumblr_inline_mze7idBUqw1rgqrs8.jpg)

With our Softlayer server launched, we’ll use git to download DevStack and install OpenStack:

    sudo apt-get install git

    git clone&nbsp;[https://github.com/openstack-dev/devstack.git](https://github.com/openstack-dev/devstack.git)

    cp samples/localrc localrc

In our machine we have two main interfaces:

- bond0 (internal network)
- bond1 (public network)

We’ll edit localrc and config our network by adding the following:

    HOST\_IP: our public ip
    FLAT\_INTERFACE: The best way is to assign br100 interface to it
    FLOATING\_RANGE: 159.253.138.184/29
    PUBLIC\_INTERFACE=bond1 (this is the interface for our internal network used by OpenStack services to communicate with each other)

And then finish the OpenStack installation.

    cd devstack; ./stack.sh

Doing so, we end up with a working OpenStack environment, with floating ips and all, however we have to assign a floating ip manually to each newly created instance. In order to auto assign ips from the floating ips pool we have to configure a floating ip pool:

    sudo nova-manage floating create --pool pool\_auto\_assign --ip\_range 159.253.138.184/29

Set auto\_assign\_floating\_ip = True in nova.conf file and restart nova-networking.

Finally, we modify the nova.conf file by adding:

    default\_floating\_pool = pool\_auto\_assign
    floating\_range = 159.253.138.184/29
    auto\_assign\_floating\_ip = True

Restart nova-network and that’s it!

We now have a working OpenStack Havana installation that auto-assigns IP addresses.&nbsp;To test everything works ok we’ll spin up some VMs. OpenStack’s dashboard Horizon is ok but we want to get these together with our machines on EC2 so let’s do it from Mist.io. First we add our OpenStack backend.

![image](/assets/tumblr-images/tumblr_inline_mz4usfYR0j1rgqrs8.png)

And we’re ready to roll. We can now create, destroy, reboot and monitor our virtual machines running on our own OpenStack cloud through Mist.io.

![image](/assets/tumblr-images/tumblr_inline_mze7iw8gpc1rgqrs8.png)

**Do you have machines across clouds and want to easily manage them?&nbsp;[Sign up for Mist.io and try it for free!](https://mist.io)**

