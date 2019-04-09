---
layout: post
title: A review of Microsoft Azure’s API
date: '2014-12-04T17:37:00+02:00'
tags:
- API
- azure
- cloud computing
tumblr_url: https://blog.mist.io/post/104331766626/a-review-of-microsoft-azures-api
---
In the last months, Microsoft has made the headlines for putting its (enormous) weight behind its Cloud business, Microsoft Azure. Between cut prices and feature enhancements, Microsoft has managed to gain significant share in the cloud provider market. [Microsoft BizSpark](http://www.microsoft.com/bizspark/) is also an excellent startup program that gives you access to many software development tools. More importantly, you get 150$/month on Azure credit to spend the way you like, providing some crucial breathing space for early startups.

![image](/assets/tumblr-images/tumblr_inline_ng2e9eSmo81rgqrs8.jpg)

As we were working to [add support for Azure](http://blog.mist.io/2014-11-25-announcing-support-for-microsoft-azure)&nbsp;in Mist.io, we had to deal with the API. A few weeks ago, we [published a review](http://blog.mist.io/2014-10-09-first-look-at-digital-oceans-new-api) of Digital Ocean’s new API. While it is still in beta, we were pleasantly surprised as it solved many issues and shortcomings apparent in the first version.

And while we were also pleasantly surprised by Azure’s speed, features and pricing, we found that the API was not in par with the rest of the Azure framework.

**Authenticating with the API**

Unlike most modern APIs that allow you to authenticate through oAuth, Azure’s API requires your subscription ID and a pem file. That pem file, needs to be produced via openssl or IIS and then uploaded to the Azure portal. Thats a lot of hoops to jump just to authenticate with an API.

**Listing your Virtual Machines**

When you log in to your Azure portal, you get a list of all your Virtual Machines. Τo get the same list through the API, you may need to make several calls. You first need to call “list cloud services”, and for each of the cloud services you have enabled, you need to call “list deployments” to get a list of all your VMs. Be prepared to make those calls in parallel, otherwise you’ll be waiting for a long time to get a list of all your VMs.

**Creating a new VM**

Creating a new VM through the API can also become a bit complicated. If the VM you want to create is your first VM in that cloud service, you need to run “create deployment” which will also create your VM. For your next VMs though, you must call on “add role”. Even if MS needs to make that distinction internally due to their architecture, there is no reason to expose that distinction to the API.

**Performing actions on your machine**

In most providers, you can perform more or less four actions on a VM: Start, stop, reboot and destroy. The same goes for Azure, although for some reason, you have to call “operations in roles” to reboot and destroy a machine, but to start or stop it, you have to call “operations on virtual machines”.

To reboot a machine you need a POST call to:

    https://management.core.windows.net/<subscription-id>/services/hostedservices/<cloudservice-name>/deploymentslots/<deployment-slot>/roleinstances/<role-instance-name>?comp=reboot

with Request Body: None

But to start a machine, you need a POST call to:

    https://management.core.windows.net/<subscription-id>/services/hostedservices/<service-name>/deployments/<deployment-name>/roleinstances/<role-name>/Operations

with Request Body:

    <StartRoleOperation xmlns="http://schemas.microsoft.com/windowsazure" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
     <OperationType>StartRoleOperation</OperationType>
    </StartRoleOperation>

It would make a little more sense if reboot was grouped with start and stop, with destroy requiring some extra security role. But as it stands now, it just complicates things.

For reference, those actions in Digital Ocean’s new RESTful API require a POST call at [https://api.digitalocean.com/v2/droplets/droplet\_id/actions](https://api.digitalocean.com/v2/droplets/droplet_id/actions) with a request body of:

    {
     "type": "power_on"
    }

to start a VM, or with:

    {
     "type": "reboot"
    }

to reboot a VM.

**Deploying SSH keys on new VMs**

One of the most common scenarios when creating a non-windows virtual machine, is for the provider to deploy it with your public SSH key already in place. That usually means calling “deploy VM” with at least one extra input, your public key. This is also the case when you create a new VM from Azure’s portal. But for some reason, the API call [requires you to upload your private key too](http://msdn.microsoft.com/library/azure/jj157194.aspx#SSH). This is obviously unacceptable for many production environments.

**Conclusion**

While we were pleasantly surprised by Azure itself and the way Microsoft is upping its game as a Cloud provider, the Azure API still needs a lot of love to be on par with the rest of the service. While it works for most MS technologies, trying to do anything else with it (like deploying a Linux VM) is too much of a challenge. Using JSON alongside XML for requests and responses, would reduce complexity for those that are used to working with other APIs. Furthermore, the API is not RESTful and the requests are far too complicated to remember, forcing you to constantly check the documentation.

**Are you using Microsoft Azure? Mist.io can help you manage and monitor your virtual machines from anywhere. [Try it for free](https://mist.io).**

