---
layout: post
title: Move fast and don’t break things!  Testing with Jenkins, Ansible and Docker
date: '2014-04-11T16:50:00+03:00'
tags:
- opensource
- testing
- ansible
- docker
- jenkins
tumblr_url: https://blog.mist.io/post/82383668190/move-fast-and-dont-break-things-testing-with
---
One of our highest priorities at [Mist.io](https://mist.io) is to never break production. Our users depend on it to manage and monitor their servers and we depend on them. At the same time, we need to move fast with development and deliver updates as soon as possible. We want to be able to easily deploy several times per day.

A big part of Mist.io is the web interface so we use tools like [Selenium](http://docs.seleniumhq.org/), [Splinter](http://splinter.cobrateam.info/) and [Behave](https://pypi.python.org/pypi/behave) for headless web interface testing. However testing the UI is time-consuming. Having to wait 40 minutes to get a green light before merging each pull request is not very agile.

In this post we’ll describe the setup we’ve used to reduce testing time. It is based on Jenkins, Ansible and Docker and its main mission is to automate build steps and run tests in parallel and as quickly as possible.

Since execution time is essential for us, we opted to run our CI suite on one of Rackspace’s [High-Performance Cloud Servers](http://www.rackspace.com/cloud/servers/)&nbsp;due to their fast performance and short provisioning times. In general, make sure you give your test server enough RAM and a fast disk.

<figure class="tmblr-full" data-orig-height="161" data-orig-width="500" data-orig-src="assets/tumblr-images/tumblr_inline_n3vea8wswP1rgqrs8.png"><img alt="image" src="assets/tumblr-images/tumblr_inline_pkbcog71yN1rgqrs8_540.png" data-orig-height="161" data-orig-width="500" data-orig-src="assets/tumblr-images/tumblr_inline_n3vea8wswP1rgqrs8.png"></figure>

Our first stop for our testing suite, is&nbsp;[Jenkins](http://jenkins-ci.org/). We’ve used Jenkins in other projects before and we feel comfortable with it since it is mature, widely used and provides great flexibility through its plugin system. Jenkins has very good [Github integration](https://wiki.jenkins-ci.org/display/JENKINS/GitHub+Plugin) which is another plus for us. It is [quite simple](http://fourkitchens.com/blog/2011/09/20/trigger-jenkins-builds-pushing-github) to set it up so that every commit in a branch will trigger the tests.

When our needs started growing, our first thought was to add more Jenkins nodes and have multiple tests running in parallel. But there are many different environments that we want to test every commit against:

- Test the deployment of the app in a clean environment, a fresh build.
- Test the deployment of the app in a staging environment where you want to ensure backwards compatibility
- Test the web interface against all supported browsers.

All these meant that testing requirements would grow over time and a testing infrastructure based solely on Jenkins would not do the job fast enough.<figure class="tmblr-full" data-orig-height="170" data-orig-width="500" data-orig-src="assets/tumblr-images/tumblr_inline_n3veczlmLb1rgqrs8.png"><img alt="image" src="assets/tumblr-images/tumblr_inline_pkbcohVKlA1rgqrs8_540.png" data-orig-height="170" data-orig-width="500" data-orig-src="assets/tumblr-images/tumblr_inline_n3veczlmLb1rgqrs8.png"></figure>

## Enter Docker

[Docker](https://www.docker.io/)&nbsp;helps you easily create lightweight, portable, self-sufficient containers from any application. It is fast, reliable and a perfect fit for our needs. The idea, is to set up Jenkins so that every pull request to a specific branch (e.g. staging) triggers a job. Jenkins then commands our testing infrastructure to spawn different docker applications simultaneously and runs the tests in parallel.

This way, we can test any environment. We just have to describe each application the same way we would’ve done manually. On the plus side, we can use pre-made images from the docker repository to jumpstart our tests.

For example we could do this:

    docker pull ubuntu

And we will end up with an ubuntu image/application. We can then run the docker container by typing:

    docker run -i -t ubuntu /bin/bash

And we will be in a fresh, newly created ubuntu environment.

But we don’t want to manually run docker commands after every pull request. What we need, are Dockerfiles. Dockerfiles are sets of steps that describe an image/container. We’ll use them to build our custom docker images.

Dockerfile commands are fairly [simple](https://docs.docker.io/en/latest/reference/builder/). For example:

    FROM ubuntu:latest
    MAINTAINER mist.io
    RUN echo "deb http://archive.ubuntu.com/ubuntu precise main universe" > /etc/apt/sources.list
    RUN apt-get update
    RUN apt-get upgrade -y
    RUN apt-get install -y build-essential git python-dev python-virtualenv
    RUN apt-get install -y xterm
    RUN apt-get install -y -q x11vnc xvfb
    RUN apt-get install -y xfonts-100dpi xfonts-75dpi xfonts-scalable xfonts-cyrillic
    RUN add-apt-repository -y ppa:mozillateam/firefox-next
    RUN apt-get update
    RUN apt-get install -y firefox
    RUN mkdir MIST
    RUN cd MIST && git clone https://github.com/mistio/mist.io
    WORKDIR MIST/mist.io
    RUN git pull
    RUN cp settings.py.dist settings.py
    RUN echo JS_BUILD = True >> settings.py
    RUN echo CSS_BUILD = True >> settings.py
    RUN echo SSL_VERIFY = True >> settings.py
    RUN virtualenv . && ./bin/pip install --upgrade setuptools
    RUN ./bin/python bootstrap.py && ./bin/buildout -N
    ADD ./test_config.py src/mist/io/tests/features/
    ADD ./init.sh /
    ENTRYPOINT ./init.sh

**FROM** &nbsp;Chooses the base image (ubuntu/latest)

**RUN** Runs the following commands. First, we update the system ant then we install Xvfb, the latest firefox etc in order to run headless browser steps. Finally, we build the Mist.io app

**ADD** &nbsp;Adds files from our host machine to the docker image. In this example, we added the configuration for tests and an init.sh script. The init.sh script could be as simple as this:

    #!/bin/bash
    cd MIST/mist.io
    git checkout $BRANCH
    ./bin/run_test

**ENTRYPOINT** &nbsp;This tells docker to start with ./init.sh script every time we run the image.

To build the docker image for future reuse:

    docker build -t mist/iotest /path/to/Dockerfile

Now we have a test environment. If there are more tests in other branches that need to be spawned we can use it for every one of them:

    docker run -e BRANCH=your_branch mist/iotest

If we had multiple test servers, we would have to build every custom image in every test server. Fortunately, docker lets you have your own [private repository](http://docs.docker.io/en/latest/use/workingwithrepository/) of docker images. You can build your custom image once, say mist/iotest, push it to the repository and run:

    docker pull mist:iotest

This is a simple test scenario, but the possibilities are endless. For example, in another of our test scenarios we want to spawn a docker application with our monitor service and one with the mist web app.

The problem, is that we need every test server configured with docker and every docker image available. And we need to automate the procedure to be able to scale our test server infrastructure whenever needed.

<figure class="tmblr-full" data-orig-height="61" data-orig-width="425" data-orig-src="assets/tumblr-images/tumblr_inline_n3veecl1RP1rgqrs8.png"><img alt="image" src="assets/tumblr-images/tumblr_inline_pkbcohD4Yh1rgqrs8_540.png" data-orig-height="61" data-orig-width="425" data-orig-src="assets/tumblr-images/tumblr_inline_n3veecl1RP1rgqrs8.png"></figure>

## Ansible to the rescue

[Ansible](http://www.ansible.com/home)&nbsp;automates deployment. It is written in Python and installation is [relatively simple](http://docs.ansible.com/intro_installation.html).&nbsp;It is available through most linux distro repositories, you can clone it from github or install it via pip.

Configuring ansible is also [easy](http://docs.ansible.com/intro_configuration.html). All you have to do is group your servers in an ansible\_hosts file and use ansible’s playbooks and roles to [configure](http://docs.ansible.com/playbooks.html) them.

For example, this is a simple ansible\_hosts file:

    [testservers]
    testserver1 ansible_ssh_host=178.127.33.109
    testserver2 ansible_ssh_host=178.253.121.93
    testserver3 ansible_ssh_host=114.252.27.128
    
    [testservers:vars]
    ansible_ssh_user=mister
    ansible_ssh_private_key_file=~/.ssh/testkey

We just told Ansible that we have three test servers, grouped as _testservers_. For each one the user is _mister_ and the ssh key is _testkey_, as defined in the [testservers:vars] section.

Each test server should have docker installed and a specified docker image built and ready for use. To do that, we have to define some playbooks and roles:

    - name: Install new kernel
      sudo: True
      apt:
        pkg: "{{ item }}"
        state: latest
        update-cache: yes
      with_items:
        - linux-image-generic-lts-raring
        - linux-headers-generic-lts-raring
      register: kernel_result
    
    - name: Reboot instance if kernel has changed
      sudo: True
      command: reboot
      register: reboot_result
      when: "kernel_result|changed"
    
    - name: Wait for instance to come online
      sudo: False
      local_action: wait_for host={{ ansible_ssh_host }} port=22 state=started
      when: "reboot_result|success"
    
    - name: Add Docker repository key
      sudo: True
      apt_key: url="https://get.docker.io/gpg"
    
    - name: Add Docker repository
      sudo: True
      apt_repository:
        repo: 'deb http://get.docker.io/ubuntu docker main'
        update_cache: yes
    
    - name: Install Docker
      sudo: True
      apt: pkg=lxc-docker state=present
      notify: "Start Docker"
    
    - name: Make dir for io docker files
      command: mkdir -p docker/iotest
    
    - name: Copy io Dockerfiles
      template:
        src=templates/iotest/Dockerfile.j2 dest=docker/iotest/Dockerfile
    
    - name: Copy io init scripts
      copy:
        src=templates/iotest/init.sh dest=docker/iotest/init.sh
    
    - name: Build docker images for io
      sudo: True
      command: docker build -t mist/iotest docker/iotest

After that, we just &nbsp;have to set ansible to trigger the tests and spawn these docker applications. All Jenkins has to do is catch the webhook of a new pull request and issue one command:

    ansible-playbook runtests.yml

Thats it.

We have set up Jenkins to respond to Github commits and call Ansible to automatically spawn and configure our test servers, and we have optimized our testing speed by using pre-made Docker images.

_Thanks to [Phil Kates](https://twitter.com/philkates)&nbsp;from Rackspace and [Nick Stinemates](https://twitter.com/nickstinemates)&nbsp;from Docker for proof reading early versions of this._

**Do you have machines across clouds and want to easily manage them?&nbsp;[Sign up for Mist.io and try it for free!](https://mist.io/)**

