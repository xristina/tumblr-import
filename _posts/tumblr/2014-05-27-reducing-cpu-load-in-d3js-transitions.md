---
layout: post
title: Reducing CPU load in D3.js transitions
date: '2014-05-27T18:48:00+03:00'
tags:
- d3
- javascript
- opensource
tumblr_url: https://blog.mist.io/post/87007749461/reducing-cpu-load-in-d3js-transitions
---
In a [previous post](http://blog.mist.io/post/80079597818/dissecting-the-new-mist-io-graphs), we promised that we’ll share our solution for optimizing our [D3.js](http://d3js.org/) graphs in the hope that it will be useful to others or that it’ll spark a conversation around ways to optimize D3 transitions.

![image](/assets/tumblr-images/tumblr_inline_n68pmd6MRK1rgqrs8.jpg)

Our default data fetch interval is 10 seconds and we want our graphs to update in real-time. We serve a continuous stream of data from the backend to the browser and we want our graphs to get smoothly updated. In order to do that, we define a wider scale than the graph width and for every new value we get, we slide the line at the far right so we can hide the new value and then we slide the line left with a D3 transition and wait until our next data fetch.

You can find a common approach used to create line animations [here](http://bl.ocks.org/benjchristensen/1148374). Unfortunately this approach uses too much cpu. The values of the svg elements are changing continuously and as a result the browser re-renders the DOM elements for every change.

So in order to reduce the browser load, we needed to reduce rendering by cutting down on the changes in the DOM elements on every graph change. We’ll start by creating a custom D3 transition:

    var animation = new Animation();
    animation.select(d3line)
            .fps(12)
            .duration(10)
            .points(startX,startY,stopX,stopY)
            .push()

- **d3line** is the element that we want to change
- **fps** are the frames per second (or better Changes in The Dom Per Second)
- **duration** : how long we want this transition to take (in seconds)
- **points** : the start and stop coordinates

When the animation starts, it will translate the element to the startx,starty position. It will gradually move the element to the final position(stopX,stopY) in the duration we have specified. In those 10 seconds we will have a total of 120 changes in the dom, 12 changes every second. This process is CPU intensive and can increase the browser load especially on lower end devices like smartphones and tablets.

Lets try and reduce that load:

d3 has already created an interpolation between transforms, so we will start by creating 2 transforms:

    var start = d3.transform( "translate(" + this.startPoint.x + "," + this.startPoint.y + ")");
    
    var stop = d3.transform("translate(" + this.stopPoint.x + "," + this.stopPoint.y + ")");");

then we’ll create our interpolate function:&nbsp;

    var interpolate = d3.interpolateTransform(start,stop);

interpolateTransform returns an interpolator between the two 2D affine transforms represented by start and stop. The interpolate function’s argument is a percentage of the translated value that we want.

For instance if we wanted the transform values of half of our transition we could run interpolate(0.5);

Then we will set an interval that runs every time we need to render. That would be 1 second / frames per second.

    var animation_interval = window.setInterval(function(){
    
    ...
    },1000 / fps);

Inside our interval function we increase the frame that we are rendering,

We get the transform value from the interpolate function by supplying the progress of our transition (x frames out of FPS)

and we apply it into the d3 selector.

    frame++;
    
    // Get transform Value and apply it to the DOM
    var transformValue = interpolate(frame/(self.fps * self.duration));
    self.d3Selector.attr("transform", transformValue);
    

Finally we check if we reached our targeted Frames or if we manually stopped the animation(stopFlag)

       if(frame >= self.fps * self.duration || self.stopFlag){
           window.clearInterval(animation_interval);
           return;
       }

**Putting it all together:**

    var start = d3.transform( "translate(" + this.startPoint.x + "," + this.startPoint.y + ")");
    
    var stop = d3.transform("translate(" + this.stopPoint.x + "," + this.stopPoint.y + ")");
    var interpolate = d3.interpolateTransform(start,stop);
    
    // Initial Start
    this.d3Selector.attr("transform", interpolate(0));
    
    // Create Interval For The Animation
    var animation_interval = window.setInterval(function(){
    
       frame++;
    
       // Get transform Value and aply it to the DOM
       var transformValue = interpolate(frame/(self.fps * self.duration));
       self.d3Selector.attr("transform", transformValue);
    
       // Check if animation should stop
       if(frame >= self.fps * self.duration || self.stopFlag){
           window.clearInterval(animation_interval);
           return;
       }
    
    
    },1000 / fps);

Our new animation constructor not only requires less cpu load but saves each animation in a buffer for uninterrupted flow. Each animation has its own callbacks that will be run when the animation starts. In our case, we experienced an average of 20-30% decrease in CPU load depending on the browser and OS.

**Do you have machines across clouds and want to easily manage them?&nbsp;[Sign up for Mist.io and try it for free!](https://mist.io/)**

