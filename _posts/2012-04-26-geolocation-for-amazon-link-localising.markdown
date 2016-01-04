---
layout: post
title: "Geolocation for Amazon Link Localising"
date: 2012-04-26 17:57
categories: blog
---

This post starts a 30 days experiment. I'm not alone in starting a blog with every intention of keeping consistent, only to find weeks fly past without so much as a grunt. So for 30 days, this blog takes on a new format: each day I will end my 'working day' with something I learnt that day. However small, however insignificant, someone somewhere will stumble where I fell or ponder thoughts I've processed. 

A purpose of today's post is to make you aware of [FreeGeoIP](http://freegeoip.net/static/index.html), a solution for those wanting to look up a visitors country by IP. In my case, I've started a site where I hope to [teach portuguese](http://www.olaportuguese) and wanted a way to link to a mentioned book to the appropriate Amazon website.

The FreeGEOIP websites offers a simple web service that receives your chosen format (csv, xml or json) and an ipaddress, quickly hitting you back with a bunch of location data. The only downside here, is that the service is limited to 1000 hits a day, so if you have a large site, you could consider setting up a local instance yourself as the project is open source. Using Rails 3, and the great `HTTParty` gem, which makes consuming web services a walk in the park, using the service couldn't be simpler:

{% highlight ruby %}
ip_address = request.remote_ip
res = HTTParty.get("http://freegeoip.net/json/#{ip_address}")
JSON.parse(res.body)
# => { 'country_name' => 'United Kingdom', 'country_code' => 'GB' ... }
{% endhighlight %}

The solution I considered was placing a method in the `ApplicationController`, with a `before_filter` set to perform an IP lookup and save the country code to a cookie, unless such a cookie for the visitor already existed. From here, I was to have a helper that would show a default Amazon link unless a matching country cookie was set. The helper would take an Amazon product ID and be used as the second arguement of a call to a `link_to` call:

{% highlight ruby %}
  # If country cookie is set, show relevant Amazon link or default to UK
  def amazon_link(product)
    countries = {
      'GB'  => { tld: 'co.uk', id: '{UK_ASSOCIATES_ID}' },
      'US' =>  { tld: 'com',   id: '{US_ASSOCIATES_ID}' }
    }

    if cookies['country'] && countries.has_key?(cookies['country'])
      amazon = countries[cookies['country']]
    else
      amazon = countries['GB']
    end

    "http://www.amazon.#{amazon[:tld]}/#{product}/?tag=#{amazon[:id]}"
  end
{% endhighlight %}

In the end I scrapped the idea, mainly because I don't like the overhead of hitting an API for every new user, but also for privacy issues. It's a topic that's difficult to find information on, but what is and what isn't allowed regarding a users IP? And what alternatives are there to obtaining the country of a site visitor?

Well, there's HTML5 Geolocation, but this requires rightfully requires permission and could be considered overkill for this simple scenario. There are also services like [Cloudfare](https://www.cloudflare.com/), that stand between your domain registar and your web application, passing along a header with country code, among many other benefits. 

Ultimately, I feel it's a flaw with Amazon's Associates program - not only do you have to sign up for each of the numerous websites indepedently, an automatic redirection system could be offered that would greatly simplify the process for publishers, and encourage it's use, thus increasing their income. What gives? 

Well that was more wordy than I expected, perhaps that's why the post rate drops off everytime. Hah, check back for more tomorrow! Have you a good solution for simple geo-location? [Hit me up on Twitter](https://twitter.com/#!/mrjamesriley)
