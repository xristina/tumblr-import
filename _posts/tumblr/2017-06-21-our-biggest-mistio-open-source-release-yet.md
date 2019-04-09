---
layout: post
title: Our biggest Mist.io open source release yet
date: '2017-06-21T15:34:12+03:00'
tags:
- open source
- cloud management
tumblr_url: https://blog.mist.io/post/162083041316/our-biggest-mistio-open-source-release-yet
---
Our engineers have been hard at work over the past months to restructure the Mist.io code and bring the open-source version up to speed with the SaaS at [https://mist.io](https://mist.io)

Today, we are pleased to announce that the new version is out, and it is our biggest open-source release so far! It comes with an easier installation process using Docker Compose and many new features:

- Support for more providers  
- Faster and better designed UI  
- Run scripts and Ansible playbooks  
- Schedule actions  
- Manage DNS records, networks and logs  
- User and team management  
- Estimation of infrastructure costs  

Last but not least, the tagging mechanism has been vastly improved and is deeply integrated with the scheduler and the cost management system

And we’re not stopping there. Now that the restructuring is over, we will continue to improve the open-source version by adding orchestration and monitoring support in the coming months.

Here is a rundown of the most important changes in this release.

**User and team management**

Until now, the open-source version supported a single user and tenant, without any authentication system built in. With this release, multiple users and organizations can access the same Mist.io instance, using separate accounts. Each user can create multiple organizations and multiple teams inside those organizations in order to invite other users to manage the same infrastructure.

<figure data-orig-width="1153" data-orig-height="888" class="tmblr-full"><img src="assets/tumblr-images/tumblr_inline_orwcrz9EdA1rgqrs8_540.png" data-orig-width="1153" data-orig-height="888"></figure>

**Scripts & Schedules**

Another feature previously only found on the SaaS version is the ability to schedule and run scripts on your servers. Scripts can be executable or Ansible playbooks.

<figure data-orig-width="1162" data-orig-height="891" class="tmblr-full"><img src="assets/tumblr-images/tumblr_inline_orwcsjoaJ11rgqrs8_540.png" data-orig-width="1162" data-orig-height="891"></figure>

If you want to know more about script management, hop over to the Mist.io [documentation](http://docs.mist.io/article/58-create-and-run-executable-scripts).

You can also use Schedules to perform specific tasks (like reboot or destroy a VM) or run scripts on your servers periodically. Schedules can be run on multiple machines, grouped by the same tag or selected by their uptime and/or cost.

<figure data-orig-width="1152" data-orig-height="950" class="tmblr-full"><img src="assets/tumblr-images/tumblr_inline_orwctnG44i1rgqrs8_540.png" data-orig-width="1152" data-orig-height="950"></figure>

**DNS and network management**

You can now add, delete and manage your zones along with the rest of your infrastructure directly through Mist.io. We currently support Amazon’s Route 53, Google, Digital Ocean, Linode, Rackspace, Softlayer and Vultr with more providers coming up soon.

<figure data-orig-width="1614" data-orig-height="620" class="tmblr-full"><img src="assets/tumblr-images/tumblr_inline_orwdbsz4811rgqrs8_540.png" data-orig-width="1614" data-orig-height="620"></figure>

Additionally, network management has also landed on the open-source version and you can create and delete networks easier than ever before.

<figure data-orig-width="1615" data-orig-height="968" class="tmblr-full"><img src="assets/tumblr-images/tumblr_inline_orwdc9tktG1rgqrs8_540.png" data-orig-width="1615" data-orig-height="968"></figure>

**Detailed logs**

Logging is an important aspect of infrastructure management and security. Every action and event happening in Mist.io leaves an audit trail. The new dashboard presents a searchable list of the latest log entries for all of your infrastructure, while on each resource page there are detailed log listings with all entries related to that resource.

<figure data-orig-width="806" data-orig-height="519" class="tmblr-full"><img src="assets/tumblr-images/tumblr_inline_orwdeaHDFf1rgqrs8_540.png" data-orig-width="806" data-orig-height="519"></figure>

**Revamped architecture using microservices**

The Mist.io open-source code has also been broken down to smaller repositories. The code is now a lot cleaner and more manageable. The Mist.io UI, API, landing page, and even the test suite are now separate repositories that act as submodules to the main Mist.io git repository. Each part comes with its own docker image that gets fired up as a microservice by Docker Compose.

**New user interface based on web components**

Built using Polymer and the latest web component standards, the new UI is much faster and looks better. It reduces the browser load and can accommodate large installations with hundreds or even thousands of machines. Web components are a W3C standard supported by all major browsers. They enable more complex applications while encouraging better reusability and separation of concern between components.

<figure data-orig-width="1545" data-orig-height="960" class="tmblr-full"><img src="assets/tumblr-images/tumblr_inline_orwdkgOjYS1rgqrs8_540.png" data-orig-width="1545" data-orig-height="960"></figure>

**Installation**

Want to check out the new version? The installation steps have been greatly simplified. First, you need to install Docker Compose: [https://docs.docker.com/compose/install/](https://docs.docker.com/compose/install/)

With Docker Compose installed, download the release yaml file and fire up Mist.io:

    wget https://github.com/mistio/mist.io/releases/download/v2.0.0/docker-compose.yml
    docker-compose up -d

Sit back for a few minutes. The first time might take a while as it downloads all the required Docker images. When it’s ready you should be able to visit [http://localhost](http://localhost) and see the Mist.io landing page. Port 80 needs to be available as it will be mapped to the nginx container.

To create a user for the first time sign up using you email. By default Mist.io will forward all email to the internal mailmock container. You can get the confirmation link by tailing the logs of that container.

    docker-compose logs mailmock

You can update the mailer settings and configure Mist.io to use your own email server, by editing the settings file at config/settings.py. After updating the MAILER\_SETTINGS parameter you’ll need to restart Mist.io for the changes to take place.

    docker-compose restart api celery 

You can also add admin users through the command line.

    docker-compose exec api bin/adduser --admin user@example.com

Enter a password and use it to sign in at [http://localhost](http://localhost)

Welcome to Mist.io! Enjoy!

