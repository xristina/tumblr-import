---
layout: post
title: 'New Mist.io interface: Faster and optimized for large installations'
date: '2015-10-13T17:00:48+03:00'
tags:
- cloud computing
- monitoring
- cloud management
- open source
tumblr_url: https://blog.mist.io/post/131088493091/new-mistio-interface-faster-and-optimized-for
---
After months of hard work by our design and development team we are proud to announce today a new version of the Mist.io UI. Built on the latest Ember version, the new UI is much faster and looks more streamlined. It reduces the browser load, while new UI enhancements like sorting and pagination can facilitate large installations with tens, hundreds or even thousands of machines.

<figure data-orig-width="1283" data-orig-height="620" class="tmblr-full"><img src="assets/tumblr-images/tumblr_inline_nw5vhkWYdu1rgqrs8_540.png" alt="image" data-orig-width="1283" data-orig-height="620"></figure>

The new Incidents panel makes it easier to see at a glance what is going on with your servers and the graphs have been improved both in speed and readability.

<figure data-orig-width="1263" data-orig-height="880" class="tmblr-full"><img src="assets/tumblr-images/tumblr_inline_nw5vtys2iK1rgqrs8_540.png" alt="image" data-orig-width="1263" data-orig-height="880"></figure>

The Machines listing has been redesigned to bring you more info, faster. Machines that have monitoring enabled, will be highlighted with an exclamation mark if there was an alert triggered recently, while a green checkbox indicates that everything has been working smoothly.

<figure data-orig-width="1250" data-orig-height="677" class="tmblr-full"><img src="assets/tumblr-images/tumblr_inline_nw5vvuCRys1rgqrs8_540.png" alt="image" data-orig-width="1250" data-orig-height="677"></figure>

The activity log has been revamped too and has become much easier to read, with different kinds of notifications getting different color codes.

<figure data-orig-width="1737" data-orig-height="988" class="tmblr-full"><img src="assets/tumblr-images/tumblr_inline_nw5vwcXc0g1rgqrs8_540.png" alt="image" data-orig-width="1737" data-orig-height="988"></figure>

Another section that has been redone to enhance readability is the Images section. Clicking on an image will show more details like the OS, image creation time, disk size, etc. Clicking on the Create Machine button will launch the new machine wizard with the image and provider preselected.

<figure data-orig-width="1121" data-orig-height="603" class="tmblr-full"><img src="assets/tumblr-images/tumblr_inline_nw5vy5EXS81rgqrs8_540.png" alt="image" data-orig-width="1121" data-orig-height="603"></figure>

The forms mechanism has also been redesigned from scratch offering better usability, making required fields stand out and preventing double submissions. Last but not least, new users will be greeted with a much more friendly home screen, guiding them to adding their infrastructure and getting the most out of Mist.io.

<figure data-orig-width="1144" data-orig-height="611" class="tmblr-full"><img src="assets/tumblr-images/tumblr_inline_nw5vyubliM1rgqrs8_540.png" alt="image" data-orig-width="1144" data-orig-height="611"></figure>

Not all changes are visible to the end user though. The redesign gave us the opportunity to finetune our UI development workflow, introducing grunt and bower to prepare all our assets. Τhe end result is minified and concatenated css, js and htmlbars files. This means faster rendering for our pages and a more streamlined development workflow that reduces the time it takes for new features to land on production.

The new interface offers speed improvements across the board, but it is especially redesigned to facilitate all kinds of infrastructure sizes. It will help you manage and monitor your infrastructure more effectively, regardless of whether it consists of a few servers or several hundreds virtual machines and containers.

Although we’ve tested the new UI extensively, there is always something that we might have missed. If you’ve stumbled on a bug or want to send us your take on the redesign, leave us a comment below, open a ticket on [github](https://github.com/mistio/mist.io/issues), or throw us an email at [support@mist.io](mailto:support@mist.io).

