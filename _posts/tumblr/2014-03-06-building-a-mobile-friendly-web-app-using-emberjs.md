---
layout: post
title: Building a mobile-friendly web app using Ember.js and jQuery Mobile
date: '2014-03-06T18:53:00+02:00'
tags:
- emberjs
- opensource
- jquery mobile
tumblr_url: https://blog.mist.io/post/78757774060/building-a-mobile-friendly-web-app-using-emberjs
---
[Ember.js](http://emberjs.com/) is an MVC framework for creating powerful web applications. It has some nifty features like auto-updating templates and flexible routing. We use Ember.js to build the frontend of mist.io and because we want our interface to be mobile-friendly, we combine it with [jQuery Mobile](http://jquerymobile.com/).

![image](/assets/tumblr-images/tumblr_inline_n20yvbGg8m1rgqrs8.png)

This howto assumes you know how to build a basic Ember.js application. If this is the first time you’ve heard of it, you should first check out ember’s excellent&nbsp;[startup guide](http://emberjs.com/guides/getting-started/).

The first and most important problem when trying to combine Ember.js and JQM is navigation. Each library has its own way of navigating between sections of a web page and they tend to not play nice with each other. We have to use only one of them, and we’ll go with Ember’s navigation system as it is much more powerful. &nbsp;So, to disable JQM’s navigation system, add the following snippet before loading jquery-mobile.js :

    $(document).bind('mobileinit', function() {
        $.mobile.ajaxEnabled = false;
        $.mobile.pushStateEnabled = false;
        $.mobile.linkBindingEnabled = false;
        $.mobile.hashListeningEnabled = false;
    });

That will stop JQM from trying to add hashes to our url and create links automatically.

Now, let us move to our second biggest problem. Ember has a great support for data bindings, which makes dynamic content applications a child’s play. However, JQM renders the page only once and as a result, any widget inserted into the DOM afterwards will look ugly. To make JQM re-render an element that has changed, use the following code :

    $(‘#my-element’).trigger(‘create’); // Renders #my-element AND all child elements

If you know exactly what widget you are dealing with, you can use it’s constructor:

    $(‘#my-element’).button(); // Makes sure #my-element looks like a button

Beware of the difference though! Calling the constructor will force #my-element to become a button. If it’s rendered already, you will end up with a button inside another one. So, triggering the create event is mostly prefered.

Now you know how to render an unrendered element, but that means you need to call this line of code each time your app inserts widgets into your DOM. Let’s see an example of how you can accomplish that:

**JS**

    // Foo Model
    App.fooModel = Ember.Object.extend({
        text: ''
    });
    
    App.fooController = Ember.ArrayController.extend({
    
        content: [],
    
        addFoo: function() {
            var foo = App.fooModel.create({
                'text': 'Foo ' + this.content.length
            });
            this.content.pushObject(foo);
        },
    
        removeFoo: function() {
            // Remove last object
            this.content.removeObject(this.content[this.content.length - 1]);
        }
    }).create();

**HTML**

    <div id=”my-list” data-role=”listview”>
        {{#each App.fooController.content}}
            <li>
                <a data-role=”button”>{{this.text}}</a>
            </li>
        {{/each}}
    </div>

Here we have a super simple application. For each foo in fooController, we get a button with the text ‘Foo \<number\>’ . Now we run this code:

    App.fooController.addFoo();
    App.fooController.addFoo();

That will add new, unrendered buttons into the DOM. Use an observer to make sure everything shows up nicely:

    // It would be more appropriate for this observer to be placed inside
    // a View, but for the sake of simplicity we wrote it inside fooController
    contentObserver: function() {
        Ember.run.next(function() {
            $(‘#my-list’).trigger(‘create’);
        });
    }.observes(‘content’)

_NOTE: jQuery mobile v1.4.0+ does not need data-role=”button” for buttons. However, the solution we present here applies to all widgets that make use of the data-role attribute_

Navigation and UI updates are the most common issues when combining Ember.js and JQM. &nbsp;But when you’re trying to build a multi-page application, you run into both of these problems at the same time.   
  
Here is a [fiddle](http://jsfiddle.net/5Scqp/10/) that combines the two solutions we showed above, to build a working, multi-page application. Here is the same [fiddle](http://jsfiddle.net/5Scqp/11/) including some functionality with the fooController we used in the examples.

On one of our next posts, we’ll share some tips on how to optimize Ember.js and jQuery mobile to improve overall performance while reducing your page size.

**Do you have machines across clouds and want to easily manage them?&nbsp;[Sign up for Mist.io and try it for free!](https://mist.io/)**

