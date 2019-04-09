---
layout: post
title: Announcing support for Microsoft Azure
date: '2014-11-25T12:50:00+02:00'
tags:
- monitoring
- cloud computing
- open source
- azure
tumblr_url: https://blog.mist.io/post/103543949091/announcing-support-for-microsoft-azure
---
With Satya Nadella, Microsoft’s new CEO, focusing on the Cloud and the massive weight the company holds, it is no wonder Microsoft Azure is the fastest growing cloud provider for the last 6 months. According to Synergy Research Group, Azure was growing at 164% on a rolling annualized basis [in Q2 2014](https://www.srgresearch.com/articles/ibm-microsoft-among-leaders-both-cloud-hardwaresoftware-and-infrastructure-services), while [in Q3](https://www.srgresearch.com/articles/microsoft-cloud-revenues-leap-amazon-still-way-out-front) it was still the fastest growing provider at 136%.

![image](/assets/tumblr-images/tumblr_inline_nflckvfA7R1rgqrs8.jpg)

This week, we are proud to announce support for Microsoft Azure in Mist.io. During the past days we finished our last round of testing, and we rolled out support through [the Mist.io service](https://mist.io) and contributed our code to the mist.io [open-source project](https://github.com/mistio/mist.io).

To add your Azure backend to Mist.io, you’ll need to supply the Subscription ID and a certificate file. The certificate file is required by Azure to authenticate your account and can be created with OpenSSL on MacOS and Linux. On Windows, you can use IIS, or install [OpenSSL for Windows](http://slproweb.com/products/Win32OpenSSL.html).

Run the following openssl command to create the file:

    user@user:~$ openssl req -x509 -nodes -days 730 -newkey rsa:2048 -subj "/C=GR/ST=Attiki/L=Athens/O=Dis/CN=www.mist.io" -keyout azure.pem -out azure.pem
    Generating a 2048 bit RSA private key
    ....++++++
    ....++++++
    writing new private key to 'azure.pem'

This creates azure.pem, a 2048 bit RSA self signed pem certificate that will be valid for 2 years.

Then, generate a .cer file out of it with the following command:

    user@user:~$ openssl x509 -inform pem -in azure.pem -outform der -out azure.cer

You will need to upload the azure.cer file on Azure and the azure.pem file on Mist.io.

Login to your Azure account on [https://manage.windowsazure.com](https://manage.windowsazure.com) and visit the SETTINGS section on the bottom left. Copy the SUBSCRIPTION ID (eg 807fff8a-86ec-4314-84fa-cfea350485e5), then select MANAGEMENT CERTIFICATES, and on the bottom of the page UPLOAD. A window pops up, where you can select the .cer file and upload it to Azure. Once the file is uploaded, you should see it on the listing of your certificates.

&nbsp; ![image](/assets/tumblr-images/tumblr_inline_nflcl5pS1Q1rgqrs8.jpg)

Now to add your backend to Mist.io, select Add backend and choose Azure. Paste the Subscription ID and press Add certificate, to upload the .pem file.

![image](/assets/tumblr-images/tumblr_inline_nflcljGh1C1rgqrs8.jpg)

By clicking Add, Mist.io will try to authenticate with Azure, and if the credentials are correct the backend will be added. You can now proceed to manage your VMs or create new machines.

![image](/assets/tumblr-images/tumblr_inline_nflclzfN2G1rgqrs8.jpg)

Mist.io currently supports monitoring on VMs running Linux, but we are in the process of adding support for Windows, so stay tuned!

**Are you using Microsoft Azure? Mist.io can help you manage and monitor your virtual machines from anywhere. [Try it for free](https://mist.io).**

