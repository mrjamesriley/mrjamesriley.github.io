---
layout: post
title: "jQuery ajax with Rails respond_to format.js"
date: 2010-10-24 08:50
categories: blog
---

This year has been quite a shift for me in every aspect of my life &ndash;  especially with regard to web development and this blog will now reflect  that. For most part, my full time job and side projects use Ruby on  Rails and related technologies such as jQuery, mysql, memcache, redis and other web development aspects such as xhtml, css, usability and design. Future posts will announce projects of my own, document my discovers and occasionally broadcast my opinions on all that is Web Development. Follow me. Let&rsquo;s jump straight in:

There's many resources out there that detail how to use jQuery with Ruby on Rails, specially how to handle the returning of data and the setting of javascript acceptance headers. Ryan Bates for example has a [Railscast](http://railscasts.com/episodes/136-jquery) on the topic - yet in my case, I was not wanting to serialize and submit a form. If you simply want to post data to the server and insert the returned generated html into your page &ndash; simply use the jQuery.ajax with `dataType` set to `script`. From here, you can simply render the partial within the controllers `format.js` block and all should work without the need for any prior jQuery setup.

To demonstrate, here's two code snippets copied straight from an application of mine:

{% highlight javascript %}
  # the jquery
  $.ajax({
    url: '/polls/' + poll_id + '/vote?option_id=' + option_id,
    dataType: 'script',
    type: 'POST',
    success: function(data) { $('#poll').html(data); }
  });

  # the controller action
  respond_to do |format|
    format.html { redirect_to '/' }
    format.js { 
      render :partial => 'widget', :locals => { :poll => @poll } 
    }
  end
{% endhighlight %}

Till next time!
