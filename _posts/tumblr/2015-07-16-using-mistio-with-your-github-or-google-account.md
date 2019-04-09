---
layout: post
title: Using Mist.io with your Github or Google account
date: '2015-07-16T11:45:34+03:00'
tags:
- oauth
- cloud management
- github
- google
tumblr_url: https://blog.mist.io/post/124229027271/using-mistio-with-your-github-or-google-account
---
All of us have to manage multiple accounts daily. One of the most frequently requested features by Mist.io users was the ability to register and log on to their account using Google and Github’s oAuth service. We’ve heard you and today we are proud to announce social auth support for Mist.io.

<figure data-orig-width="397" data-orig-height="438" class="tmblr-full"><img src="assets/tumblr-images/tumblr_inline_nrko8miZgD1rgqrs8_540.png" alt="image" data-orig-width="397" data-orig-height="438" class="center-image"></figure>

Adding oAuth support makes logging in to Mist.io much easier, but there are also several security reasons behind our decision to support social authentication.

One of the most common issues when signing up to try new services, is that there is a tendency to reduce password complexity in order to remember them, or even worse, to reuse the same password on multiple accounts. If one of these services fails to enforce strong security measures, other accounts can get compromised too. That is why we chose two of the most secure oAuth providers that most of our users already have an account with: Google and Github.

Additionally, many of us evaluate different services that we may not end up using and eventually forget we ever signed up for. Both Google and Github provide the ability to revoke access to any external service from a central place, keeping the list of connected services manageable. It is far easier to make a habit of checking your connected services once in a while, than trying to remember every single service you ever signed up for.

Last but not least, Google and Github provide two-factor authentication, adding an extra layer of protection by using a second device to secure access to your account. Github provides some [helpful instructions](https://help.github.com/articles/about-two-factor-authentication/)on how to set it up and Google has built a nice [landing page](https://www.google.com/landing/2step/index.html) that describes what its all about.

When choosing to log in with one of these services for the first time, you’ll have to authorize Mist.io to be able to access the login information.

<figure data-orig-width="517" data-orig-height="512" class="tmblr-full"><img src="assets/tumblr-images/tumblr_inline_nrko9pu3jo1rgqrs8_540.png" alt="image" data-orig-width="517" data-orig-height="512" class="center-image"></figure>

Using social auth with Mist.io enables you to skip typing your password everytime you want to use the service. This way, logging in is not only faster, but also more secure as your password is safe from prying eyes.

If you already have an account with Mist.io, logging in through Github or Google will take you to your existing account as long as you use the same email for both services. Give it a spin and as always let us know if you run into any issues.

