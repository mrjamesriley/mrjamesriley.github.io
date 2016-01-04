---
layout: post
title: "Haml helpers Succeed and Preceed"
date: 2012-04-27 16:59
categories: blog
---

With Haml, placing a full-stop or comma immediately after a link, for example, can be confusing. Sure, we have the whitespace removal functionaliy removal that gives us the ability to place `<` or `>` to remove the space before before and after a tag. The issue is, it's ugly, unclear and I'm come across instances where it simply doesn't do as advertised. 

The [haml helpers](http://haml-lang.com/docs/yardoc/Haml/Helpers.html) page helpfully offers the `succeed` and `preceed` helpers that can be used as follows:

{% highlight ruby %}
= succeed '.' do
  %a{ href: 'example.com' } Example
# => <a href="example.com">Example</a>.
{% endhighlight %}

If it isn't clear, the succeed helper takes a block which it'll evaluate and place the passed string directly after, without white space. In this case, a full-stop is placed directly after the generated link. `preceed` works the same, but places the given string before the output.

Writing out my own example was motivated purely by my strong preference for the Ruby 1.9 hash syntax.
As far as programming tidbits go, this is a good candidate. Small and tasty. Till Monday!
