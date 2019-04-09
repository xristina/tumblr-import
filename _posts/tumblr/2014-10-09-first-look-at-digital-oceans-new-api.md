---
layout: post
title: First look at Digital Ocean’s new API
date: '2014-10-09T17:18:00+03:00'
tags:
- DigitalOcean
- api
- cloud
tumblr_url: https://blog.mist.io/post/99488119976/first-look-at-digital-oceans-new-api
---
Digital Ocean was built with developers in mind. With low prices, high performance virtual machines and a simple and straightforward interface, it has managed to attract many freelancers, startups and SMBs.

Especially the web interface really shines in comparison to competion. However this is not the only way a developer will interact with a cloud provider. Digital Ocean’s API, though available from early on, came with some caveats. Fortunately, a few months ago, DO [launched a beta version](https://www.digitalocean.com/company/blog/api-v2-enters-public-beta) of their new API, which aims to solve most of these issues, while adding new functionality like oAuth authentication and support for IPv6.

**A RESTful API makes much more sense**

Certain tasks in the first version felt a little awkward. Lets look for example at [key management](https://developers.digitalocean.com/#keys):

<table style="width:100%;"\><tr style="font-weight: bold;"\><td style="border: solid 1px black;padding: 10px;"\>Function</td\><td style="border: solid 1px black;padding: 10px;"\>Type of request</td\><td style="border: solid 1px black;padding: 10px;"\>Command sent</td\></tr\><tr\><td style="border: solid 1px black;padding: 10px;"\>Create a new key</td\><td style="border: solid 1px black;padding: 10px;"\>GET</td\><td style="border: solid 1px black;padding: 10px;"\>/ssh\_keys/new/</td\></tr\><tr\><td style="border: solid 1px black;padding: 10px;"\>List all keys</td\><td style="border: solid 1px black;padding: 10px;"\>GET</td\><td style="border: solid 1px black;padding: 10px;"\>/ssh\_keys</td\></tr\><tr\><td style="border: solid 1px black;padding: 10px;"\>Destroy a key</td\><td style="border: solid 1px black;padding: 10px;"\>GET</td\><td style="border: solid 1px black;padding: 10px;"\>/ssh\_keys/key\_id/destroy/</td\></tr\></table\>

&nbsp;

It just seems unnatural and quite difficult to remember, forcing you to check the documentation all the time. However, the new beta version is truly RESTful, which makes things much simpler:

<table style="width:100%;"\><tr style="font-weight:bold;"\><td style="border: solid 1px black;padding: 10px;"\>Function</td\><td style="border: solid 1px black;padding: 10px;"\>Type of request</td\><td style="border: solid 1px black;padding: 10px;"\>Command sent</td\></tr\><tr\><td style="border: solid 1px black;padding: 10px;"\>Create a new key</td\><td style="border: solid 1px black;padding: 10px;"\>POST</td\><td style="border: solid 1px black;padding: 10px;"\>/account/keys</td\></tr\><tr\><td style="border: solid 1px black;padding: 10px;"\>List all keys</td\><td style="border: solid 1px black;padding: 10px;"\>GET</td\><td style="border: solid 1px black;padding: 10px;"\>/account/keys</td\></tr\><tr\><td style="border: solid 1px black;padding: 10px;"\>Destroy a key</td\><td style="border: solid 1px black;padding: 10px;"\>DELETE</td\><td style="border: solid 1px black;padding: 10px;"\>/account/keys/key\_id</td\></tr\></table\>

&nbsp;

Easier to remember and pretty much what you would expect once you start working with the API.

**Improved Authentication**

[OAuth support](https://www.digitalocean.com/company/blog/integrate-your-apps-with-our-api-using-oauth/) was also a much welcomed addition. It allows developers to authenticate with DO using third-party services like Github. Also, services and apps built around the DO API can now share credentials with the DO account.

More importantly, users can create and authorize API tokens to provide read-only or read/write access to their account without exposing their credentials. Having your read-only API token compromised can result in much less frustration and damage than having your credentials stolen.

**Better key management**

The are also several issues fixed in the new version. For example, asking for a list of your ssh keys [through the old API](https://developers.digitalocean.com/v1/ssh-keys/), returned a list with ids and names, but without the public keys. That made it impossible to tell if a key you had locally, already existed in the DO account. Furthermore, if you tried to upload a key that already existed, the API returned an unspecified error with no info on why the key failed to get imported (was it a bad/unsupported key or did the key already existed in the database?):

    {u'error_message': u'SSH Key failed to be created',
    u'message': u'SSH Key failed to be created',
    u'status': u'ERROR'}

[In the latest version](https://developers.digitalocean.com/#list-all-keys), you get a proper response with much more info:

    {u'message': u'Ssh key SSH Key is already in use on your account', u'id': u'unprocessable_entity'}

**Conclusion**

Although still in public beta, the new API is already a big improvement compared to the first version. There is an [open tracker](https://github.com/digitalocean/api-v2/issues) if you are having issues or if you want to ask for a new feature, and as always, the [Digital Ocean community](https://www.digitalocean.com/community/) is very active in writing documentation and providing help. Most of the tools using the API are migrating or have already migrated to the new version, although we expect that the old version will still be around for some time.

The new API is [very well documented](https://developers.digitalocean.com/v2/) and with many examples provided. Overall, we think that Digital Ocean did a great job on the new version and solved some long standing issues while creating a modern, easy to use API. Well done!

We have already switched to using the new API and we’ve been quite happy with it so far.

**Are you using Digital Ocean? Mist.io can help you manage and monitor your virtual machines from anywhere. [Try it for free](https://mist.io/).**

