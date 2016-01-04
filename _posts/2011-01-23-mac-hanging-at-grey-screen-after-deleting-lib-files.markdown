---
layout: post
title: "Mac hanging at grey screen after deleting lib files"
date: 2011-01-23 06:29
categories: blog
---
Is your Mac failing to boot past the grey screen, thanks to deleting some
important system files? The example below is specific, but the general method
to resolve should be the same in many cases: boot into single user mode and
re-install the missing program.

Having switched to a Mac after years of being solely a Windows user (and you
think you had a tough time growing up?!), every day is very much a learning
curve. Today was no different, while trying to get libxml2 working nicely with
the Nokogiri rubygem, I installed a newer version of the toolkit to a different
location. My Mac stubbornly insisted on using the current 2.6.16 version so I
bravely strolled into terminal and ran: `sudo rm /usr/lib/libxml*`.

It turns out libxml2 is pretty fundamental to Mac and without it I was stuck.
Helpfully, I had no access to the Leopard installer and the CD drive on this
MacBook happens to be broken. Still, equiped with a bit of time, a Windows PC
nearby and a USB stick, here&rsquo;s the steps I took:

1. Found out the exact software version I needed. As I am using version 10.5.8 of OSX10, [this webpage](http://www.explain.com.au/oss/libxml2xslt.html) told me it's libxml2 version 2.6.16 that's built in, so this was what I went on the hunt for.
2. Grabbed the source code  from the [directory of old libxml2 versions](http://xmlsoft.org/sources/old/) versions, before unpacking and copying the libxml2-2.6.16 to the USB stick.
3. Booted the Mac in Single user mode, by holding Command+S from the second you hit the power button. This sends you to a terminal prompt from where the fun begins.
4. Mounting the USB drive had to be done before making use of the contents.  This was done by simply following the steps detailed in [this blog](http://macsage.com/mounting-usb-drive-in-single-user-mode") article. This left me with my USB contents available within/Volumes/usb.

From here, it was a simple case of installing the software with use of the standard linux program installation steps:

{% highlight ruby %}
./configure
make
make install
{% endhighlight %}

Once complete, a reboot finally took me past the grey loading screen and back to where I was. Once the overwhelming emotion of being reunited with a loved one passed, I was back to developing. Lesson learnt: don't be an idiot.  Never sticks with me.
