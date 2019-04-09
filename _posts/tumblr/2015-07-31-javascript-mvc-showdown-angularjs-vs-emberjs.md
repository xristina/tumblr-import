---
layout: post
title: 'JavaScript MVC showdown: AngularJS vs EmberJS'
date: '2015-07-31T13:39:01+03:00'
tags:
- angularjs
- emberjs
- javascript
tumblr_url: https://blog.mist.io/post/125506672521/javascript-mvc-showdown-angularjs-vs-emberjs
---
As web applications continue to grow in complexity and functionality, JavaScript usage is increasing rapidly. A few years ago, using jQuery or even vanilla JS was enough to build a web app. As applications evolved, code complexity increased and it was getting harder and harder to manage it.

Several JS frameworks started to pop up in order to solve these issues and to provide added functionality that was much needed in modern, rich web applications. At [Mist.io](https://mist.io/), we’ve built our frontend using [EmberJS](http://emberjs.com/) back when it was still in its infancy. We are constantly following its progress ever since and comparing it with what other frameworks are doing to see if it is still the best fit. We’re happy to share that knowledge in the hope that it’ll help anyone trying to find the right tool for the job. Today, we’re publishing a comparison between Ember and its closest rival: [AngularJS](https://angular.io/).

<figure class="tmblr-full" data-orig-height="500" data-orig-width="640"><img data-orig-height="500" data-orig-width="640" src="assets/tumblr-images/tumblr_inline_nsct2dSxZA1rgqrs8_540.png" alt="image"></figure>

Both of them are expected to release their second version in the upcoming months, so this is a great time to look at their current state and what is in store for us. Ember has already shipped a Beta and is expected to release version 2.0 within the month. Angular is being completely rewritten and has a goal of releasing version 2.0 by the end of the year.

Lets take a step by step comparison:

Learning Curve

Documentation

Support

Performance

Scalability

AMD (Asynchronous Module Definition)

Flexibility

Conclusion and the future

## Learning Curve

For anyone with small or no experience with JavaScript frameworks and a deadline to catch, learning curve is important. The first time experience varies a lot, so let’s look at them separately:

Ember uses strict patterns in order to keep the code clean, readable and scale gracefully. It is heavily opinionated in the way it wants things done. Τhere are specific conventions that have to be followed by developers and specific Ember objects that one can extend. Unfortunately, this can be a bit alienating for a newcomer. Fortunately, nowadays there is [Ember CLI](http://www.ember-cli.com/) to help you jumpstart your project and keep your dependencies up to date. It is still a steeper experience than Angular but it is much smoother than it was a year ago.

Version 2.0 will not only make Ember faster and EcmaScript 6 compatible, but it will follow a more familiar upgrade and versioning pattern which will help newcomers feel more comfortable.

Angular on the other hand is easier to grasp the first time you look at it. It is not very oriented to Single Page Applications, so you don’t need to learn stuff like routing from the very start. It uses vanilla JavaScript objects so there is no need to memorize and extend framework-specific objects.

On the down side, its Directive Definition Object mechanism is quite complicated for novice developers and requires a great effort to understand and memorize it.

Angular is being rewritten from scratch for version 2, in order to become better, faster and follow the EcmaScript 6 goodies. Unfortunately some features will simply die in version 2.0 (while others will be introduced) and some of the existing practices will die with them. That might add a little confusion at the beginning.

**Verdict**

There is no clear winner here although Angular probably has the edge. Angular is easier to grasp, but EmberCLI can jumpstart your Ember project and allow you to play with more advanced stuff, faster. Installing EmberCLI and launching your first project is a matter of a few commands:

    npm install -g ember-cli
    ember new my-new-app
    cd my-new-app
    npm install && bower install
    ember server

And if you visit [http://localhost:4200](http://localhost:4200) you will see your Ember app.

Also, if you’re building a Single Page Application, Ember’s routing system is easier to master and cleaner to write:

    App.Router.map(function() {
     this.route('user', {path : 'my-profile'});
    });
    Ember.Route.extend({
     model: function() {
       return {
         name: "John Doe"
       };
     }
    });

While to declare a new route in Angular you need to:

    angular.module('project', ['ngRoute'])
    .config(function($routeProvider) {
     $routeProvider
     .when('/my-profile', {
       controller:'UserController',
       templateUrl:'profile.html'
     });
    })
    .controller('UserController', function($scope) {
     $scope.user = {
       name: 'John Doe'
     };
    });

## Documentation

Documentation plays a very important role when choosing a new JS framework, as all of them are comparatively new and in rapid development. Many developers complained regularly about Angular’s webpage so the core team decided to update this alongside with the upcoming 2.0 version. The new documentation is already up at [https://angular.io/](https://angular.io/) and it looks very nice, although for production applications, version 1.x is still highly recommended.

On the other side, Ember’s documentation pages are very carefully designed and built. The main issue with Ember is that the core team has applied many important changes per minor version which are not documented that well. This is also going to change in version 2.0 after which a stricter versioning pattern will be followed. The documentation history starts from version 1.10, so &nbsp;projects that didn’t follow Ember’s progress and are stuck in a previous unmaintained release, might be out of luck.

**Verdict**

Ember’s documentation is much easier to read and well written so it steals the win. Despite the fact that there are many more resources for Angular as it has been around longer and is backed by Google. Things are changing to the better with Angular version 2.0, although as Ember targets mainly the increasingly popular SPAs the Ember community is also getting bigger. &nbsp;

## Support

Angular has been around since 2009 and it has gained a lot of adoption so there are tons of resources out there. From blog posts and tutorials to books and meetups, Angular has a vibrant ecosystem providing a lot of direct and indirect support. The fact that it is backed by Google makes it even more attractive. Ember on the other hand is more community oriented without a huge company behind it. It was launched two years later, in 2011, but the Ember community is well established and support requests rarely remain unanswered.

**Verdict**

Angular is the clear winner here with tons of resources and a vibrant ecosystem plus Google’s backing. That is not to say that Ember’s support is lacking though. The Ember community is very helpful and well established, while growing in numbers and resources.

## Performance

Both frameworks support two-way data binding. That means that any changes in the API goes through to the UI and vice-versa. This is very cool, but has performance limits which eventually started plaguing both frameworks as they became more popular and were started being used for larger applications.

To solve that, both frameworks are going to take advantage of HTML5’s Web Components. Web Components are a set of standards currently being produced by Google engineers as a W3C specification. They allow for the creation of reusable widgets or components in web documents and web applications. AngularJS will fully conform on version 2.0, while EmberJS has already started moving in that direction using the Glimmer engine for rendering, which greatly improved its performance. Unfortunately not all browsers yet support Web Components, although that is mostly the [usual suspect’s fault](http://jonrimmer.github.io/are-we-componentized-yet/).

**Verdict**

Although each framework has its own strong and weak points, Ember feels much faster at the moment and since the introduction of Glimmer. A lot is going to change for Angular though regarding performance with version 2.0.

## Scalability

When building larger and more complex applications, the advantages of using a strict syntax, specific conventions and routing declarations pay off. Ember’s strict conventions and layout, while providing a steeper learning curve, eventually pay off in terms of scalability. Unless the project manager sets very strict guidelines, Angular’s freedom may end up in a messy and unmanageable code as complexity increases.

**Verdict**

The strictness that was a downside for Ember when we were discussing its’ learning curve, is an asset when building bigger and more complex apps. Although it is perfectly possible to build bigger apps with Angular, Ember makes your life easier down the road.

## AMD (Asynchronous Module Definition)

Up until recently, Javascript did not have a module loading mechanism supporting dependencies. The most commonly used solution to that was AMD pattern with the main player being requireJS for scalable applications. This is still the only choice for AngularJS, which means that you also need to learn how requireJS works and shape your code accordingly. Fortunately, this is also going to change in version 2.0 &nbsp;where they’ll start using EcmaScript 6’s Module pattern.

Ember developers [never actually liked](http://tomdale.net/2012/01/amd-is-not-the-answer/) AMD pattern a lot, so they’ve developed EmberCLI to solve that problem. EmberCLI uses Esperanto which translates ES6 module syntax into AMD modules. That way, you can skip from learning requireJS and start writing directly using the future standard of ES6. The code ends up looking as simple as this:

    import Ember from 'ember';
    export default Ember.Route.extend({
    ...
    });

## Flexibility

Each framework follows its own philosophy. EmberJS offers a more stable and flexible ecosystem with Bower and Broccoli all packaged under EmberCLI hood. This makes the development workflow much smoother as many steps are already prepared.   
  
AngularJS on the other hand, offers more flexibility although that comes with some shortcomings. If we need to minify and concatenate our files we simply have to use a separate tool like Grunt or Gulp etc. It also lets us select our own way to structure our app while EmberJS offers a quite stable and compound ecosystem.

## Conclusion and the future

The fact that Angular is being re-written from scratch with no backward compatibility [has created quite a stir in the community](http://developer.telerik.com/featured/can-angularjs-maintain-dominance/). Additionally Google is also supporting [Polymer](https://www.polymer-project.org) which recently released its first production-ready version, so everyone is concerned about the future of Angular.

Regarding the use of web components, all three frameworks are heading that way at the moment. Polymer is the new kid on the block and it was always based on Web Components, Ember has already gone there since version 1.13 and EmberCLI, while Angular will be there in a few months.

It is hard to pick a clear winner. Each framework has its strong points. Angular is easier to learn but is undergoing a complete rewrite, while Ember provides a better workflow and its ecosystem scales better. If you’re already using Ember, there doesn’t seem to be a compelling enough reason to switch. If you’ve been using Angular up until now there didn’t seem to be a good enough reason to switch either, although with version 2.0 being a complete rewrite you might as well re-evaluate your options.

If you’re starting a new project, the best fit would depend on the size and complexity of your project. Αre you aiming to build a scalable and well-organised Single Page App? Ember is probably the best option at the moment as it will jumpstart the process and -at least until Angular’s version 2.0 is out- it can smoothly transition you to the latest technologies. Additionally, since Angular is being rewritten from scratch, we expect it to be a bit unstable at the start. Ember on the other side has an easier upgrade process and the core team has been moving in careful steps towards new technologies.

**Do you have machines across clouds and want to easily manage them? [Sign up for Mist.io and try it for free!](https://mist.io/)**

