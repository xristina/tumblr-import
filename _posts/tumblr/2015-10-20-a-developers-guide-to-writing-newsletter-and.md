---
layout: post
title: A developer’s guide to writing newsletter and email templates
date: '2015-10-20T15:35:21+03:00'
tags:
- web design
- templates
- mailchimp
tumblr_url: https://blog.mist.io/post/131085224931/a-developers-guide-to-writing-newsletter-and
---
Newsletters and emails play a vital role for businesses. They help them stay in touch with their audience, inform their customers about important news, brand new features or re-engage people with new offers. To increase their efficiency, email templates need to bear the company’s branding, colors and attitude. A plain text email, is unattractive and especially when there is a lot of information to communicate it can be quite inefficient. It definitely won’t stand out from the rest.

Designers try to use the most appealing and attractive design techniques such as flat and material design principles combined with their companies logos, colors and trademarks in order to provide a consistent style. Additionally, more than half the people read their emails using their smartphone or tablet. This forces professionals to target mobile devices -along with desktop and laptop users- &nbsp;and their great variety of different screen sizes and email clients,.

So, using HTML templates is crucial. Unfortunately, email client limitations forbid the use of the most common modern approaches used when building a web page. These limitations apply regardless of whether you’re using a newsletter service like Mailchimp or your own in-house platform.

**Problems, problems and more problems**

Of the many email clients out there, most of them are far behind in CSS support, forcing you to work the oldschool way and defer from using CSS3 rules and media queries. Nonetheless, the increasing variety of screen sizes requires a modern, responsive approach to the template design.

To add to the lack of proper CSS support, many popular email clients like Gmail and Outlook use their own preprocessors before serving your code. This is done mainly to combat malicious code, but it forces the template to look much different than what you envisioned and have tested with your browser. For example, all the head link tags get stripped out, so you can say goodbye to any external css files added in there. Some of the clients -I’m looking at you Gmail- go ahead and even strip out style tags and body tags, while most of them completely remove any javascript code.

One of the most popular clients, Gmail, uses one of the strictest preprocessors. So, make sure your template appears as intended on Gmail and the rest should require minor changes.

Of course, there are no assets or folders that you can include, so all your images should be served from somewhere. And the use of retina technology and high pixel density screens can often make these images appear blurry and ugly.

**Working around the CSS restrictions**

Getting around the no-head rule and the lack of external assets can be easily done by putting your CSS code inline, inside your html tags. The problem is, it defies the purpose of using CSS styles in the first place, which is to separate code and appearance and reuse the same rule. Using \<style\> tags instead of inline code in the body tag would at least let your styles be reusable. Unfortunately Gmail strips style tags from the body too.

To tackle this, you can develop the templates with separate CSS rules and turn them to inline CSS when you finish with the design. A very useful tool that can do it on the fly is [ink inliner](http://zurb.com/ink/inliner.php). If you want a more solid solution try integrating a Grunt task into your workflow. If you’re unfamiliar with Grunt, there is a nice [Getting Started](http://gruntjs.com/getting-started) doc on how to use it.

Your Gruntfile should look like this:

```javascript
module.exports = function(grunt)
{
  grunt.initConfig({
   pkg: grunt.file.readJSON('package.json'),
    inlinecss: {
     main: {
      files: {
       'inlined.html': 'email.html'
      }
     }
    }
  });
  grunt.loadNpmTasks('grunt-inline-css');
  grunt.registerTask('default', ['inlinecss']);
};
```

Awesome, you can just put the rules inline and you’re done. Or not. Most clients don’t support CSS3 rules and some of them don’t even support common CSS 2.1 rules. For example, the outlook.com client doesn’t support margins and android Gmail application doesn’t support box-shadow. This forces your emails to look different across different devices and so you should be very careful on what style you are using and how you are applying it. A very useful table for css support across all types of email clients can be found [here](https://www.campaignmonitor.com/css/).

**Making the most your images**

Since your work is in one html file and you cannot send over a folder with files along with your template, you should use images with absolute paths only and host them on a publicly accessible server. If you don’t have access to a public server, you can use one of the innumerous available image sharing services like [Imgur](http://imgur.com/), [Imagevenue](http://www.imagevenue.com/), [Photobucket](http://photobucket.com/) or even [Amazon S3](https://aws.amazon.com/s3/).

To optimize render speed, remember to always use the less possible number of images, processed with a minification tool. There is also an image [minification plugin](https://github.com/gruntjs/grunt-contrib-imagemin) for Grunt that can be added to the previous example and automate your workflow.

Remember that email images are hidden by default and clients reveal them only when the user’s permission is granted. Εmpty space is a bad sign for your email campaign goals, so remember to add alt tags for all your images.

On screens with high density, like retina screens on high-end mobile phones, you should provide a retina-ready version of your logo. Use a higher resolution image and force it to resize down to its half size for optimal results by using a width or a height attribute. This is the best solution available since you cannot count on media queries, and background-image or background-size rules are not supported that well too.

Alternatively, SVG images look nice on retina displays but they are also not supported many email clients. If you really want to use the SVG format for your images, you should provide a fallback PNG version, use the SVG inline and use media queries to hide or reveal the appropriate image accordingly. That way the PNG logo will be always the default option in case the media rules are not supported by the client. Here is an example on how to do that:

HTML markup:
```html
<img style=”display:block;” class=”raster” src="images/logo.png" alt="logo" width="100px" height="150px">
<svg style=”display:none;” class=”vector” xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="100px" height="150px">
  <path fill="rgb(50, 235, 30)" d="M150 0 L75 200 L225 200 Z"/>
</svg>
```

CSS rule:
```css
@media (-webkit-min-device-pixel-ratio: 2), 
  (min--moz-device-pixel-ratio: 2),
  (min-resolution: 2dppx),
  (min-resolution: 192dpi) {
    [mobile] .raster {
      display: none !important;
    }
    [mobile] .vector {
      display: block !important;
    }
  }
````
When rendered on a client that can read the media query, the !important style will kick in, trumping the inline styles.

**Playing nice on different resolutions and clients**

On larger displays having a fixed width helps with avoiding long, difficult to read lines and keeping your email pretty. A good width for a template that looks nice on bigger resolutions is 600px. Using a fixed width can help center your content for big screens. For minor screen widths you should use media rules and change the width of your template to 100%.

```css
@media screen and (max-width: 480px) {
  div#main { 
    width: 100%;
  }
}
```

Unfortunately, as we covered earlier, style tags are not accepted by all clients and media queries can not be added inline. If you’re aiming at a very specific target group (i.e. only iOS users) you can get away with using media rules. You can check [here](https://litmus.com/blog/understanding-media-queries-in-html-email) which clients support media queries. Even rules like float, max-width and min-width are not 100% supported which reduces our options when building responsive templates. There are two solutions to provide responsive versions of your email: using tables attributes as shown [here](https://css-tricks.com/ideas-behind-responsive-emails/) or the so-called [hybrid approach](http://labs.actionrocket.co/the-hybrid-coding-approach).

Some clients, like Outlook.com, don’t support margin which is commonly used when positioning elements. Try to use padding where possible which is better supported across clients.

**Conclusion**

Developing a good email template that will look slick in different clients and resolutions can be a painful task even for an experienced developer. Sometimes there are important reasons for cutting of functionality that is available in browsers and that’s ok. For example, all clients automatically remove scripts, while Outlook disables forms to prevent email recipients from unwittingly submitting sensitive data.

But the most frustrating thing is the lack of decent CSS support across major email clients. It is a painful reminder of the browser state several years ago, when developing a webpage seemed like walking through a minefield of different CSS interpretations, broken rendering engines and unsupported rules.

