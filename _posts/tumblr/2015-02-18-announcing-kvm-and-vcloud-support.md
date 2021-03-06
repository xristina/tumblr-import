---
layout: post
title: Announcing KVM and vCloud support
date: '2015-02-18T13:11:00+02:00'
tags:
- kvm-virtualization
- vcloud
- cloud computing
- monitoring
tumblr_url: https://blog.mist.io/post/110890044256/announcing-kvm-and-vcloud-support
---
One of our goals at Mist.io, is to ease the pain of infrastructure management by providing a simple, touch-friendly interface for all of your infrastructure. Whether you’re using private or public cloud Virtual Machines, Docker containers or bare metal servers, Mist.io aims to streamline your management workflow. We are always aiming to support more diverse ecosystems and this week, we are proud to announce support for KVM and VMware’s vCloud.

Mist.io supports adding KVM backends via [libvirt](http://libvirt.org/). It will show a list of all of the virtual machines (running and terminated), along with the hypervisor itself. You can execute actions such as reboot, shutdown and start. Instead of having to log in to the hypervisor to reboot a VM that has hanged, you can now do it with a few taps on your smartphone, laptop or tablet, along with the rest of your infrastructure.

Adding a KVM backend is a simple, one-step process. Just click on the Add Backend button and fill in the form.

<figure><img src="assets/tumblr-images/tumblr_inline_njpjwpME8H1rgqrs8.png" alt="image"></figure>

If you don’t have keys associated with Mist.io, refer to our [SSH keys documentation](https://mistio.zendesk.com/hc/en-us/articles/200794389-Creating-keys-and-providing-SSH-access-to-Mist-io) on how to connect Mist.io with your servers. If you don’t specify an SSH key, Mist.io will assume that you are connecting via tcp (qemu+tcp), in this case the KVM hostname should have tcp port 5000 open and allowing connections from Mist.io. If you are running a standalone mist.io installation and KVM can be connected via unix socket, you could specify localhost as the hostname, and mist.io will try to connect via a unix socket (qemu:///system).

To add a vCloud backend, you’ll need the vCloud username and password, your Organization name and your Hostname.

<figure class=""><img src="assets/tumblr-images/tumblr_inline_njpjzeviVr1rgqrs8.png" alt="image"></figure>

Once you click Add, Mist.io will connect to your KVM or vCloud backend and your VMs will appear on the Machines list next the rest of your infrastructure, neatly tagged with the backend name to make them easier to find.

<figure><img src="assets/tumblr-images/tumblr_inline_njpjyqblQG1rgqrs8.jpg" data-img-key="190" alt="image"></figure>

To create a new VM through Mist.io, click on the Create button and set the VM’s configuration in the popup form. You can optionally choose to run a command or have Mist.io set up monitoring once the server is up and running.

<figure class=""><img src="assets/tumblr-images/tumblr_inline_njpk1aovPI1rgqrs8.jpg" alt="image"></figure>

**Are you using KVM or vCloud? Mist.io can help you manage and monitor your virtual machines from anywhere. [Try it for free](https://mist.io).**

