---
layout: post
title: Open-source multi cloud orchestration with Mist.io and Ansible
date: '2014-09-25T18:33:00+03:00'
tags:
- ansible
- opensource
- cloud computing
- Docker
tumblr_url: https://blog.mist.io/post/98392661806/open-source-multi-cloud-orchestration-with-mistio
---
“Orchestration” is a broad term in IT automation, that can be used to describe many completely different things. From blasting commands to hundreds of servers, to mass provisioning of new Virtual Machines or managing multiple diverse configurations. With the arrival of the Cloud, systems became more scalable and while the quest for zero downtime continues, new tools are emerging to help sysadmins deal with managing their infrastructure.

![image](/assets/tumblr-images/tumblr_inline_ncgr38L1wL1rgqrs8.png)

Whether you’re looking to improve scalability, automate your testing procedure or manage a growing number of VMs, the latest and greatest kid on the block is called [Ansible](http://www.ansible.com/). In Mist.io, we’ve been using Ansible (along with Jenkins and Docker) extensively in our [Continuous Integration suite](http://blog.mist.io/2014-04-11-move-fast-and-dont-break-things-testing-with) and for managing our configuration.

Using Ansible to launch and configure Docker images saved us a lot of time. And while we love Ansible’s simplicity and elegance, we’ve found its cloud integration a bit lacking. While it ships with [modules](http://docs.ansible.com/list_of_cloud_modules.html) supporting the most popular cloud providers, there are quite a few missing from the list. Since cloud abstraction is one of the things we do well in Mist.io, we decided to step in and give something back to the awesome Ansible community.

Enter [Mist.ansible](https://github.com/mistio/mist.ansible). A set of Ansible modules that use the open-source Mist.io API to connect and manage VMs in multiple cloud providers. Of course, integrating the Mist.io monitoring service with Ansible means that it is easier than ever for the Mist.io users to automate the procedure of creating a new VM and monitor it.

From provisioning new VMs and deploying new configurations to enabling monitoring, alerting and automation, all it takes is Mist.ansible and a simple playbook. Lets look at an example on how to leverage Ansible’s power and simplicity and combine it with Mist.io’s extensive provider list to provision a new server on EC2 or Nephoscale. We’ll start by installing Mist.ansible:

    pip install ansible
    pip install mist.ansible

Deploying a new VM in EC2 takes just one addition to our Ansible playbook:

    - name: Provision Ubuntu machine to EC2
     mist:
       mist_email: your@email.com
       mist_password: yourpassword
       backend: EC2
       state: present
       name: MyMachine
       key: myKey
       image_id: ami-bddaa2bc
       size_id: m1.small
       location_id: 0

A more complete playbook that checks connectivity to the Nephoscale backend, creates a pair of ssh keys, selects and provisions a new VM by choosing from a list of sizes and images, and deploys our ssh key would look like this:

    ---
    - name: Provisioning playbook for nephoscale
     hosts: localhost
     tasks:
     - name: Ensure NephoScale backend is present
       mist_backends:
         mist_email: yourmist@account.com
         mist_password: yourmistpassword
         provider: nephoscale
         state: present
         backend_key: nepho_username
         backend_secret: nepho_password
         name: Nepho
     - name: Generate Key and save locally
       mist_keys:
         mist_email: yourmist@account.com
         mist_password: yourmistpassword
         name: NephoKey
         auto_generate: true
         save_locally: true
         local_save_path: /home/user/.ssh/NephoKey
     - name: Search for Ubuntu images
       mist_images:
         mist_email: yourmist@account.com
         mist_password: yourmistpassword
         backend: Nepho
         search_term: Ubuntu
       register: images
     - name: List available sizes
       mist_sizes:
         mist_email: yourmist@account.com
         mist_password: yourmistpassword
         backend: Nepho
       register: sizes
     - name: List available locations
       mist_locations:
         mist_email: yourmist@account.com
         mist_password: yourmistpassword
         backend: Nepho
       register: locations
     - name: Create Machine
       mist:
         mist_email: yourmist@account.com
         mist_password: yourmistpassword
         backend: Nepho
         key: NephoKey
         location_id: "{{ locations['locations'][0]['id'] }}"
         size_id: "{{ sizes['sizes'][0]['id'] }}"
         image_id: "{{ images['images'][0]['id'] }}"
         name: nephomachine

Mist.ansible also integrates with the Mist.io monitoring service. As long as you have an account on [https://mist.io](https://mist.io) and you haven’t exceeded your monitoring plan, you can enable monitoring in one step, during server creation:

    - name: Provision SUSE machine on EC2 and enable monitoring
      mist:
        mist_email: your@email.com
        mist_password: yourpassword
        backend: EC2
        state: present
        name: MyMachine
        key: myKey
        image_id: ami-9178e890
        size_id: m1.small
        location_id: 0
        monitoring: true
        wait_for_stats: true

And if you want to monitor a specific app, you can provision the server and set up monitoring for a custom metric by doing:

    - name: Enable monitoring and add custom plugin.py
      mist:
        mist_email: your@email.com
        mist_password: yourpassword
        backend: EC2
        name: dbServer
        state: present
        key: newKey
        wait: true
        monitoring: true
        wait_for_stats: true
        metric: MyPlugin
        python_file: /home/user/plugin.py

Where plugin.py is a python script that returns the value of the metric you want to monitor. We’ve covered custom metrics in one of our [previous blog posts](http://blog.mist.io/2014-07-08-monitor-everything-from-anywhere-introducing-custom) and you can read more about monitoring at [Mist.io’s documentation](https://mistio.zendesk.com/hc/en-us/articles/201095508).

Of course, there’s a lot more that you can do with Mist.ansible. You can find the module’s extensive documentation at [readthedocs](http://mistansible.readthedocs.org/en/latest/).

**Do you have machines across clouds and want to easily manage them?&nbsp;[Sign up for Mist.io and try it for free!](https://mist.io/)**

