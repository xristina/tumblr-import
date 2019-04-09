---
layout: post
title: Announcing Google Compute Engine Support
date: '2014-04-14T19:50:00+03:00'
tags:
- gce
tumblr_url: https://blog.mist.io/post/82699931164/announcing-google-compute-engine-support
---
It’s only been a few months since the official public launch of [Google Compute Engine](https://cloud.google.com/products/compute-engine/) (GCE) and it is already a formidable player in the cloud computing arena. A few weeks ago, Google captured a lot of media attention when it announced a massive 32 percent price drop for its Compute Engine service.

This week, we are proud to announce support for GCE in Mist.io. During the past days we finished our last round of testing, and yesterday we rolled out support through the [Mist.io service](https://mist.io)&nbsp;and contributed our code to the mist.io [open-source project](https://github.com/mistio/mist.io).

To add a GCE backend, you’ll need to supply the Project ID, the email address and a private key for a Service Account.

![image](/assets/tumblr-images/tumblr_inline_n41591Ps351rgqrs8.jpg)

To obtain this information, [login](https://cloud.google.com/console) with your Google account and select your project. Note the PROJECT ID, as this will be used as the Project ID on mist.io. If you have no projects created, you can create one.

![image](/assets/tumblr-images/tumblr_inline_n42i71J6ao1rgqrs8.jpg)

On the left column select APIs & auth and then APIs. Make sure Google Compute Engine is enabled. Then on the left column select Credentials and hit Create New Client ID.

![image](/assets/tumblr-images/tumblr_inline_n42i9ayGgS1rgqrs8.jpg)

In the popup window select Service account and create the Client ID.

![image](/assets/tumblr-images/tumblr_inline_n42ibclb3H1rgqrs8.jpg)

A private key file will be created for you to store. The key is encrypted with password ‘notasecret’. Note that you cannot retrieve the private key if you lose it and you’ll need to generate a new one.

Store the Email address, this will be the Email address used on mist.io. &nbsp;

![image](/assets/tumblr-images/tumblr_inline_n42ihh2M5j1rgqrs8.jpg)

Finally, you must convert the private key file with the following command:

    openssl pkcs12 -in GCE_private_key -passin pass:notasecret -nodes -nocerts | openssl rsa -out key.pem

where GCE\_private\_key is the encrypted private file you have stored on the previous step. This will produce key.pem which is the key file you’ll need for Mist.io.

For example:

    user@user:/tmp/GCE$ openssl pkcs12 -in 752fade-privatekey.p12 -passin pass:notasecret -nodes -nocerts | openssl rsa -out key.pem
    MAC verified OK
    writing RSA key
    user@user:/tmp/GCE$ cat key.pem
    -----BEGIN RSA PRIVATE KEY-----
    ...
    -----END RSA PRIVATE KEY-----

After you add the Email address and Project ID, click on Upload key and specify the path to the private key file you have produced on the last step.

![image](/assets/tumblr-images/tumblr_inline_n415fxtDWl1rgqrs8.jpg)

Mist.io will try to authenticate with these credentials and if they are valid the backend will be added. You will see a list with your existing servers in GCE and will be able to create/deploy new ones.

![image](/assets/tumblr-images/tumblr_inline_n415gnXnPZ1rgqrs8.jpg)

**Do you have machines across clouds and want to easily manage them?&nbsp;[Sign up for Mist.io and try it for free!](https://mist.io/)**

