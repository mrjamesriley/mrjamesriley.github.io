---
layout: post
title: "Pow Server for Rails Applications"
date: 2012-05-02 19:44
comments: true
categories: 
---
Many Rails developers have many apps and think nothing of switching between them as necessary with the trusty `rails server` command. Each morning cosists of the familiar routine: open terminal, start auto-running tests, and boot the dev server. The [Pow](http://pow.cx) server makes this final step unecessary.

Pow is a super simple to set-up solution for serving Rails apps on your dev machine. By simple, we're talking a single command, and by Rails, we're talking Rack apps which extends to any Rack solution you muster up. The [Pow Screencast](http://get.pow.cx/media/screencast.mov) will get you up to speed in no time. Watch the video, symlink your Rack apps and hit `{folder}.dev` in your browser, where `{folder}` is the name of the newly created symlink that should be within `~/.pow/` by default.

*Tip*: As mentioned above, by default Pow is set up to use `.dev`, which Google Chrome doesn't recognise and will 'helpfully' treat it as a search query. The solution here? End the call with a forward slash `/` e.g `hamster.dev/`. Finally, a solution for using `debugger` with a Rails app served through Pow can be found [here](https://gist.github.com/1098830).
