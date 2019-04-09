---
layout: post
title: 'Improving Ember.js performance. Part 1: Separating the templates'
date: '2014-12-22T17:30:00+02:00'
tags:
- emberjs
- opensource
- handlebars
tumblr_url: https://blog.mist.io/post/105872826186/improving-emberjs-performance-part-1-separating
---
In Mist.io we love [Ember.js](http://emberjs.com). It is a powerful framework and we’ve been using it since the very early days. Today, we are launching the first of a series of articles on how to optimize the performance of your Ember.js applications.

One way to increase Ember’s performance, is to use precompiled templates. But in order to do that, we first have to separate the templates from the rest of our code. It is a required step for precompilation and it will also help us to keep our code manageable while our app is growing.

This guide assumes you know how to build a basic Ember.js application. If this is the first time you’ve heard of it, you should first check out ember’s excellent [startup guide](http://emberjs.com/guides/getting-started/).

Let’s take a look on how templates are used: On a basic Ember.js app, we have index.html in which we load Ember, it’s dependencies and our templates. We also have an app.js file for our application code.

    # code:html:index.html
    <body>
       <script type="text/x-handlebars" data-template-name="index">
           <h1>
               Hello World from the index template
           </h1>
       </script>
    
       <script type="text/x-handlebars" data-template-name="about">
           <h1>
               About page
           </h1>
       </script>    
    
       <script src="jquery.js"></script>
       <script src="handlebars.js"></script>
       <script src="ember.js"></script>
       <script src="app.js"></script>
    </body>
    ##

[Here](http://jsbin.com/mepupu/1/edit?html,js,output) is a jsbin with the code.

When this line of code is run:

    # code:js:app.js
    App = Ember.Application.create();
    ##

Ember finds all the templates in the DOM (index and about in our case), compiles them and stores them into the Ember.TEMPLATES (an object used for template look ups). We want to end up with our templates code on separate files, but if we remove them from index.html, Ember.js has no way of discovering them. So, lets help Ember by initiating the compilation ourselves and assign our templates to the TEMPLATES object:

    # code:html:index.html
    <body>
      <!-- Template code is no more part of index.html -->
       <script src="jquery.js"></script>
       <script src="handlebars.js"></script>
       <script src="ember.js"></script>
       <script src="app.js"></script>
    </body>
    ##

    # code:js:app.js
    // Manually compiling "index" template and storing it into the TEMPLATES object
    Ember.TEMPLATES['index'] = Ember.Handlebars.compile('<h1>Hello World from the index template</h1>');
    
    // We do the same for "about"
    Ember.TEMPLATES['about'] = Ember.Handlebars.compile('<h1>About page</h1>');
    App = Ember.Application.create();
    ##

Or, [in jsbin](http://jsbin.com/mepupu/2/edit?html,js,output).

Our index template is now compiled and added to the TEMPLATES object. Of course, writing the entire html into a javascript statement is far from ideal, and although we’ve moved the templating code from index.html, it is still mixed up with our js code. It would be much better to load the template from a file in our project tree. This can be done through an AMD library like [require.js](http://requirejs.org/) or just a plain ajax call. Whatever the choice, the code should look somewhat like this:

    # code:hbs:app/templates/index.hbs
    <h1>Hello World from the index template</h1>
    ##

    # code:hbs:app/templates/about.hbs
    <h1>About page</h1>
    ##

_Note: Whether you give your files “hbs” or “html” suffix doesn’t matter at this point_

    # code:js:app.js
    // getFile represents an ajax call or a require.js statement
    getFile('app/templates/index.hbs', function (indexTemplate) {
       Ember.TEMPLATES['index'] = Ember.Handlebars.compile(indexTemplate);
    });
    
    getFile('app/templates/about.hbs', function (aboutTemplate) {
        Ember.TEMPLATES['about'] = Ember.Handlebars.compile(aboutTemplate);
    });
    App = Ember.Application.create();
    ##

Much better. Our code is cleaner and our templates are separated from our logic. This however, has introduced a new problem. The template needs to exist by the time ember wants to render it, but file loading is asynchronous and we have no way of knowing when it will be loaded. The solution is to create our application right after our templates are loaded. Something along these lines should do the trick:

    # code:js:app.js
    getFiles(['app/templates/index.hbs', 'app/templates/about.hbs'],
      function (indexTemplate, aboutTemplate) {
        Ember.TEMPLATES['index'] = Ember.Handlebars.compile(indexTemplate);
        Ember.TEMPLATES['about'] = Ember.Handlebars.compile(aboutTemplate);
        App = Ember.Application.create();
      }
    );
    ##

You can see the result on jsbin, [here](http://jsbin.com/mepupu/3/edit?html,js,output).

We have extracted our templates from index.html and we have them neatly organized in separate files. We’ve compiled them and stored them into the Ember.TEMPLATES object. Finally, we’ve made sure that the application is created after the templates are loaded.

Our code is now clean and tidy, but we’re still compiling the templates in real time which is performance expensive. What we want is to serve the templates precompiled and let the browser evaluate the end result. And with our templates separated from the rest of our code, we are ready to do just that.

We’ll look at templates precompilation and on ways to integrate it to our development workflow on part two of our series next week, so stay tuned!

**Do you have machines across clouds and want to easily manage them?&nbsp;[Sign up for Mist.io and try it for free!](https://mist.io/)**

