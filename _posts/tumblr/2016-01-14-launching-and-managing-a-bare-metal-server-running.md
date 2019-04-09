---
layout: post
title: Launching and managing a bare metal server running Docker in a few minutes
date: '2016-01-14T07:28:01+02:00'
tags:
- packet
- bare metal
- cloud management
- monitoring
tumblr_url: https://blog.mist.io/post/135389284061/launching-and-managing-a-bare-metal-server-running
---
In 2014, we saw Docker bursting into the infrastructure world, disrupting everything in its path. While almost every cloud provider is now offering Docker-related features, it seems the prefered weapon of choice in 2015 was the combination of Docker and bare metal servers. Meanwhile, according to this year’s [OpenStack Superuser survey](http://superuser.openstack.org/articles/openstack-users-share-how-their-deployments-stack-up), Docker is the fourth most used “hypervisor” in OpenStack. Rob Whiteley’s, VP of Marketing at Hedvig, anticipates that Docker [will become the #2 hypervisor](http://www.hedviginc.com/blog/four-predictions-for-openstack-in-2016) in OpenStack in 2016.

Given the right tools, launching and managing a bare metal server with Docker can be accomplished in minutes. In this example, we’ll launch a new bare metal server from [Packet](https://www.packet.net) with Docker, secure our Docker instance with TLS authentication and manage and monitor all of them via the Mist.io interface. You will need an account with Packet and either an account on the Mist.io [SaaS version](https://mist.io) or to install the [open-source version](https://github.com/mistio/mist.io).The same instructions can be applied for any provider though, not just Packet. If you’re on another provider, you can check the documentation on [how to add them](http://docs.mist.io/category/75-adding-your-infrastructure) to Mist.io.

Why did we choose Packet? We loved the speed and agility they provide when it comes to bare metal servers. They provide dedicated servers on demand, in 6 minutes or less. Their platform brings the price and performance benefits of dedicated servers to the cloud, so it seemed ideal for our use case. If you don’t have a Packet account, you can sign up through our [partnership page](https://www.packet.net/promo/mistio/) and get $100 in usage credit to check it out.

Now, let’s see how easy it is to launch a new bare metal with Docker through Mist.io. First, we need to add Packet as a cloud provider:

<figure data-orig-width="1219" data-orig-height="793" class="tmblr-full"><img src="assets/tumblr-images/tumblr_inline_nzi1g7spdU1rgqrs8_540.png" alt="image" data-orig-width="1219" data-orig-height="793"></figure>

Once you provide your API key and optionally a Project name, you should see your Packet machines along the rest of your infrastructure neatly tagged for easy access.

<figure data-orig-width="914" data-orig-height="647" class="tmblr-full"><img src="assets/tumblr-images/tumblr_inline_nzi1gwZQFJ1rgqrs8_540.png" alt="image" data-orig-width="914" data-orig-height="647"></figure>

Time to provision your new machine and install Docker in one go. Just choose your favorite OS (in this case we went with Debian 8) and size and in the Script section, paste the script from [get.docker.com](https://get.docker.com/). Click on Launch and Packet will have the bare metal server ready for you within minutes.

<figure data-orig-width="1232" data-orig-height="1011" class="tmblr-full"><img src="assets/tumblr-images/tumblr_inline_nzii29MrHd1rgqrs8_540.png" alt="image" data-orig-width="1232" data-orig-height="1011"></figure>

As with all the infrastructure managed through Mist.io, you can choose to enable monitoring, set rules and alerts in order to orchestrate your Packet machines. Once the server is ready, you can click on the shell button to open the web shell and test your Docker instance by writing:

    docker run hello-world

<figure data-orig-width="964" data-orig-height="695" class="tmblr-full"><img src="assets/tumblr-images/tumblr_inline_nziixqnpvU1rgqrs8_540.png" alt="image" data-orig-width="964" data-orig-height="695"></figure>

That’s it. Within a few minutes, a new bare metal server with Docker is up and running on Packet’s infrastructure.

But you can also use Mist.io to manage our Docker containers along your Packet bare metal server and the rest of your infrastructure. To do that, you must launch Docker in a way that can be remotely managed. To protect your Docker instance from being accessed by third parties, we will enable TLS authentication. You will need to write down the server’s IP address, that can be found inside the Basic Info panel:

<figure data-orig-width="1226" data-orig-height="200" class="tmblr-full"><img src="assets/tumblr-images/tumblr_inline_o0xtsxIL901rgqrs8_540.png" alt="image" data-orig-width="1226" data-orig-height="200"></figure>

In this instance the IP is 147.75.174.165.

The following commands will generate a certificate and the keys required, and allow Mist.io to connect to it. Substitute the SERVER\_IP with your server’s address:

    openssl genrsa -aes256 -out ca-key.pem 4096
    openssl req -new -x509 -days 365 -key ca-key.pem -sha256 -out ca.pem
    openssl genrsa -out server-key.pem 4096
    openssl req -subj "/CN=SERVER_IP" -sha256 -new -key server-key.pem -out server.csr
    echo subjectAltName = IP:127.0.0.1,DNS:mist.io > extfile.cnf
    openssl x509 -req -days 365 -sha256 -in server.csr -CA ca.pem -CAkey ca-key.pem -CAcreateserial -out server-cert.pem -extfile extfile.cnf
    openssl genrsa -out key.pem 4096
    openssl req -subj '/CN=client' -new -key key.pem -out client.csr
    echo extendedKeyUsage = clientAuth > extfile.cnf
    openssl x509 -req -days 365 -sha256 -in client.csr -CA ca.pem -CAkey ca-key.pem -CAcreateserial -out cert.pem -extfile extfile.cnf

That’s it. Now let’s clean up a bit by deleting unneeded files and restricting access to the rest:

    rm -v client.csr server.csr
    chmod -v 0400 ca-key.pem key.pem server-key.pem

Once you’re done, start Docker by running:

    docker daemon --tlsverify --tlscacert=ca.pem --tlscert=server-cert.pem --tlskey=server-key.pem -H=0.0.0.0:2376

Download the key.pem, cert.pem and ca.pem to your device and at the Mist.io home page, click on Add Cloud. Fill in the server’s IP, the port that the Docker daemon is listening to (in this case 2376), add the certificates and click on Add.

<figure data-orig-width="1247" data-orig-height="837" class="tmblr-full"><img src="assets/tumblr-images/tumblr_inline_o0xtus8zk41rgqrs8_540.png" alt="image" data-orig-width="1247" data-orig-height="837"></figure>

That’s it. Docker is up and running. You can now launch new containers in seconds through the Mist.io UI and enable monitoring and automation. Since we just installed the Docker engine, we’ll need to pull some images to use for our containers. To pull a Debian image from Mist.io’s Docker repository, you can run:

    docker pull mist/debian-wheezy

And then back to Mist.io, go to Machines and launch the Create Machine wizard:

<figure data-orig-width="1242" data-orig-height="1032" class="tmblr-full"><img src="assets/tumblr-images/tumblr_inline_o0xtveUsN71rgqrs8_540.png" alt="image" data-orig-width="1242" data-orig-height="1032"></figure>

Choose Debian Wheezy as your OS and click on Launch. That’s it. Your new container is ready to use!

<figure data-orig-width="1237" data-orig-height="368" class="tmblr-full"><img src="assets/tumblr-images/tumblr_inline_o0xtvvUatz1rgqrs8_540.png" alt="image" data-orig-width="1237" data-orig-height="368"></figure>
