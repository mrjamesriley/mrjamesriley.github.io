---
layout: post
title: "Simple Google Maps Example with Coffeescript and jQuery"
date: 2012-05-08 07:51:23
categories: blog
---

I'm working with the latest version of the Google Maps API (V3) and the demo in the [developers guide](https://developers.google.com/maps/documentation/javascript/tutorial) guide screams out for a dose of Coffeescript and jQuery. The three Javascript requirements here are:

1. Include the Google Maps API
2. Define the `initialize()` function
3. Call the `initialize()` function `onload` (one the page has finished loading)

Here's a before and after for the Coffeescript/jQuery makeover:
<!-- more -->

{% highlight javascript %}
  // Javascript
  function initialize() {
    var myOptions = {
  center: new google.maps.LatLng(-34.397, 150.644),
          zoom: 8,
          mapTypeId: google.maps.MapTypeId.ROADMAP
    };
    var map = new google.maps.Map(document.getElementById("map_canvas"),
        myOptions);
  }

  // Coffeescript
  initialize = ->
    myOptions =
      center: new google.maps.LatLng -34.397, 150.644
      zoom: 8,
      mapTypeId: google.maps.MapTypeId.ROADMAP
    map = new google.maps.Map $('#map_canvas')[0], myOptions
{% endhighlight %}

Variables scoped locally by default, curly braces made redundant, semi-colons unecessary, readability increased and number of lines reduced. Of course, this only scratches the surface of what [Coffeescript offers](http://coffeescript.org). Notice also the use of jQuery to select the 'map_canvas' div, remembering that the selector returns an array and we want the actual DOM element.

## Body Onload

Adding an `onload` to the `body` tag can be a pain, especially if you want it conditional, it's likely you're layout is shared amongst other pages. Thankfully we can take advantage of how jQuery can run once the page has loaded, moving this out of the body tag and into your external Javascript file like so:

{% highlight javascript %}
  // this
  <body onload="initialize()">

  // becomes
  $ ->
    initialize()
{% endhighlight %}

## Including the Maps API

As I mentioned it above, it'd be rude not to elaborate - here's a quick Rails example of how it can be done. We don't want to simply place a `javascript_include_tag` in our layout if the only want the map on a single page. A clean method here is to pass the javascript include in a call to `content_for` which is then rendered in your layout with use of the yield method. Hopefully this makes it clear:

{% highlight ruby %}
  # application layout
  yield :js

  # the view
  - content_for :js do
    = javascript_include_tag 'http://maps.googleapis.com/maps/api/js?key=API_KEY=false'
{% endhighlight %}

The `:js` symbol can be anything, it's simply an identifier but it helps to keep it somewhat generic so the yield can be used by other views.
