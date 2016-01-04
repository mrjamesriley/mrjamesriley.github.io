---
layout: post
title: "Nginx Error: no such file to load -- rack/commonlogger"
date: 2011-03-20 06:07
categories: blog
---

Despite a lack of experience with server administration, I jumped in the deep end this week by grabbing a VPS from slicehost. With it, you start from scratch, with not even the firewall iptables setup to allow port 80. You're given root access to ssh and sent on your way, which is one level above being given a screw driver and then directed to a pile of hardware components. But with frustration and confusion comes learning and knowledge&hellip; got me feeling pretty powerful over here.

The aim was to set up a Sinatra app, running with Nginx and Passenger. The installation of all was easy enough, up till the final step of pointing Nginx to my Sinatra app and seeing the following error: `Exception LoadError in application (no such file to load -- rack/commonlogger)`

Searches online suggest the issue could be down to file permissions, a poorly configured config.ru, or incorrect file paths. My config.ru correctly required rubygems and Sinatra (which depends on rack and thus includes it) and I tried everything from switching ruby versions to creating new users/groups.  Frustratingly, the issue was none of these, and the error never helped lead me to the solution. Passenger is smart enough to know if your app is a rack app by the existence of config.ru in the folder above the &lsquo;public&rsquo; folder specified in the Nginx config settings. It's almost smart enough to call Bundler.setup() if you happen to have a Gemfile.

I had a Gemfile in my app, yet had removed Bundler and so wasn't including the gem anywhere. The solution? remove the Gemfile.

This post does more to show my lack of passenger skills than it does for my community giving efforts but hopefully this'll help someone even if that person is me, stumbling across this post in 6 months time.
