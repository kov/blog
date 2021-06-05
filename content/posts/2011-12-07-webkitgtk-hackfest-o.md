---
title: WebKitGTK+ hackfest \o/
author: kov
type: post
date: 2011-12-07T23:34:58+00:00
url: /2011/12/webkitgtk-hackfest-o/
categories:
  - Collabora
  - English
  - gnome
  - webkit

---
It&#8217;s been a couple days since I returned from this year&#8217;s WebKitGTK+ hackfest in A Coruña, Spain. The weather was very nice, not too cold and not too rainy, we had great food, great drinks and I got to meet new people, and hang out with old friends, which is always great!

![Hackfest black board](http://farm8.staticflickr.com/7173/6461611339_d03659c168_m.jpg "Hackfest black board")

I think this was a very productive hackfest, and as usual a very well organized one! Thanks to the GNOME Foundation for the travel sponsorship, to our friends at Igalia for doing an awesome job at making it happen, and to Collabora for sponsoring it and granting me the time to go there! We got a lot done, and although, as usual, our goals list had many items not crossed, we did cross a few very important ones. I took part in discussions about the new WebKit2 APIs, got to know the new design for GNOME&#8217;s Web application, which looks great, discussed about [Accelerated Compositing][2] along with Joone, Alex, Nayan and Martin Robinson, hacked libsoup a bit to port the multipart/x-mixed-replace patch I wrote to the awesome gio-based infrastructure Dan Winship is building, and some random misc.

The biggest chunk of time, though, ended up being devoted to a very uninteresting (to outsiders, at least), but very important task: making it possible to more easily reproduce our test results. TL;DR? We made our bots&#8217; and development builds use jhbuild to automatically install dependencies; if you&#8217;re using tarballs, don&#8217;t worry, your usual autogen/configure/make/make install have not been touched. Now to the more verbose version!

**The need**

![Our three build slaves reporting a few failures](/wp-content/uploads/2011/12/bots.png "Our three build slaves reporting a few failures")

For a couple years now we have supported an increasingly complex and very demanding automated testing infrastructure. We have three buildbot slaves, one provided by Collabora (which I maintain), and two provided by Igalia (maintained by their WebKitGTK+ folks). Those bots build as many check ins as possible with 3 different configurations: [32 bits release][4], [64 bits release][5], and [64 bits debug][6].

In addition to those, we have another bot called the EWS, or Early Warning System. There are two of those at this moment: one VM provided by Collabora and my desktop, provided by myself. These bots build every patch uploaded to the bugzilla, and report build [failures][7] or [passes][8] (you can see the green bubbles). They are very important to our development process because if the patch causes a build failure for our port people can often know that before landing, and try fixes by uploading them to bugzilla instead of doing additional commits. And people are usually very receptive to waiting for EWS output and acting on it, except when they take way too long. You can have an idea of what the life of an EWS bot looks like by looking at the [recent status][9] for the WebKitGTK+ bots.

![EWS](/wp-content/uploads/2011/12/ews.png)

Maintaining all of those bots is at times a rather daunting task. The tests require a very specific set of packages, fonts, themes and icons to always report the same size for objects in a render. Upgrades, for instance, had to be synchronized, and usually involve generating new baselines for a large number of tests. You can see in [these instructions][11], for instance, how strict the environment requirements are &#8211; yes, we need specific versions of fonts, because they often cause layouts to change in size! At one point we had tests fail after a compiler upgrade, which made rounding act a bit different!

So stability was a very important aspect of maintaining these bots. All of them have the same version of Debian, and most of the packages are pinned to the same version. On the other hand, and in direct contradition to the stability requirement, we often require bleeding edge versions of some libraries we rely on, such as libsoup. Since we started pushing WebKitGTK+ to be libsoup-only, its own progress has been pretty much driven by WebKitGTK+&#8217;s requirements, and Dan Winship has made it possible to make our soup backend much, much simpler and way more featureful. That meant, though, requiring very recent versions of soup.

To top it off, for anyone not running Debian testing and tracking the exact same versions of packages as the bots it was virtually impossible to get the tests to pass, which made it very difficult for even ourselves to make sure all patches were still passing before committing something. Wow, what a mess.

**The explosion^Wsolution**

So a few weeks back Martin Robinson came up with a proposed solution, which, as he says, is the &#8220;nuclear bomb&#8221; solution. We would have a jhbuild environment which would build and install all of the dependencies necessary for reproducing the test expectations the bots have. So over the first three days of the hackfest Martin and myself hacked away in building scripts, buildmaster integration, a jhbuild configuration, a jhbuild modules file, setting up tarballs, and wiring it all in a way that makes it convenient for the contributors to get along with. You&#8217;ll notice that our buildslaves now have a step just before compiling called &#8220;updated gtk dependencies&#8221; (gtk is the name we use for our port in the context of WebKit), which runs jhbuild to install any new dependencies or version bumps we added. You can also see that those instructions I mentioned above [became a tad simpler][12].

It took us way more time than we thought for the dust to settle, but it eventually began to. The great thing of doing it during the hackfest was that we could find and fix issues with weird configurations on the spot! Oh, you build with AR_FLAGS=cruT and something doesn&#8217;t like it? OK, we fix it so that the jhbuild modules are not affected by that variable. Oh, turns out we missed a dependency, no problem, we add it to the modules file or install them on the bots, and then document the dependency. I set up a very clean chroot which we could use for trying out changes so as to not disrupt the tree too much for the other hackfest participants, and I think overall we did good.

**The aftermath**

By the time we were done our colleagues who ran other distributions such as Fedora were already being able to get a substantial improvements to the number of tests passing, and so did we! Also, the ability to seamlessly upgrade all the bots with a simple commit made it possible for us to very easily land a change that required a very recent (as in unreleased) version of soup which [made our networking backend way simpler][13]. All that red looks great, doesn&#8217;t it? And we aren&#8217;t done yet, we&#8217;ll certainly be making more tweaks to this infrastructure to make it more transparent and more helpful to the users (contributors and other people interested in running the tests).

If you&#8217;ve been hit by the instability we caused, sorry about that, poke mrobinson or myself in the #webkitgtk+ IRC channel on FreeNode, and we&#8217;ll help you out or fix any issues. If you haven&#8217;t, we hope you enjoy all the goodness that a reproducible testing suite has to offer! That&#8217;s it for now, folks, I&#8217;ll have more to report on follow-up work started at the hackfest soon enough, hopefully =).

[![Gnome logo](https://foundation.gnome.org/wp-content/uploads/sites/12/2021/03/gnome-logos-1.png)](https://foundation.gnome.org/)

[![Collabora Logo](http://www.collabora.com/logos/collabora-logo.svg)](https://www.collabora.com/)

[![Igalia logo](https://www.seekpng.com/png/full/430-4308845_i-can-only-thank-igalia-for-sponsoring-my.png)](http://www.igalia.com/)

 [1]: http://www.flickr.com/photos/mariosp/6461611339/sizes/l/in/set-72157628217381055/
 [2]: http://blog.kov.eti.br/?p=214
 [3]: http://blog.kov.eti.br/wp-content/uploads/2011/12/bots.png
 [4]: http://build.webkit.org/waterfall?show=GTK%20Linux%2032-bit%20Release
 [5]: http://build.webkit.org/waterfall?show=GTK%20Linux%2064-bit%20Release
 [6]: http://build.webkit.org/waterfall?show=GTK%20Linux%2064-bit%20Debug
 [7]: https://bugs.webkit.org/show_bug.cgi?id=73960#c9
 [8]: https://bugs.webkit.org/show_bug.cgi?id=73319
 [9]: http://webkit-commit-queue.appspot.com/queue-status/gtk-ews
 [10]: http://blog.kov.eti.br/wp-content/uploads/2011/12/ews.png
 [11]: https://trac.webkit.org/wiki/WebKitGtkLayoutTests?version=19
 [12]: https://trac.webkit.org/wiki/WebKitGtkLayoutTests
 [13]: http://trac.webkit.org/changeset/101917/trunk/Source/WebCore/platform/network/soup/ResourceHandleSoup.cpp