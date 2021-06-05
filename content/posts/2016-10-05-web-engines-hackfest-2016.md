---
title: Web Engines Hackfest 2016!
author: kov
type: post
date: 2016-10-05T12:23:44+00:00
url: /2016/10/web-engines-hackfest-2016/
categories:
  - Collabora
  - English
  - webkit

---
I had a great time last week and the [web engines hackfest][1]! It was the 7th web hackfest hosted by Igalia and the 7th hackfest I attended. I&#8217;m almost a local Galician already. Brazilian Portuguese being so close to Galician certainly helps! Collabora co-sponsored the event and it was great that two colleagues of mine managed to join me in attendance.

It had great talks that will eventually end up in videos uploaded to the web site. We were amazed at the progress being made to Servo, including some performance results that blew our minds. We also discussed the next steps for WebKitGTK+, WebKit for Wayland (or WPE), our own Clutter wrapper to WebKitGTK+ which is used for the [Apertis][2] project, and much more.

![Zan giving his talk on WPE (former WebKitForWayland)](https://blog.kov.eti.br/wp-content/uploads/2016/10/20160928_103753-1024x576.jpg "Zan giving his talk on WPE (former WebKitForWayland)")

One thing that drew my attention was how many Dell laptops there were. Many collaborans (myself included) and igalians are now using Dells, it seems. Sure, there were thinkpads and macbooks, but there was plenty of inspirons and xpses as well. It&#8217;s interesting how the brand make up shifted over the years since 2009, when the hackfest could easily be mistaken with a thinkpad shop.

Back to the actual hackfest: with the recent release of Gnome 3.22 (and Fedora 25 nearing release), my main focus was on dealing with some regressions suffered by users experienced after a change that made putting the final rendering composited by the nested Wayland compositor we have inside WebKitGTK+ to the GTK+ widget so it is shown on the screen.

One of the main problems people reported was applications that use WebKitGTK+ [not showing anything][4] where the content was supposed to appear. It turns out the problem was caused by GTK+ not being able to create a GL context. If the system was simply not able to use GL there would be no problem: WebKit would then just disable accelerated compositing and things would work, albeit slower.

The problem was WebKit being able to use an older GL version than the minimum required by GTK+. We fixed it by testing that GTK+ is able to create GL contexts before using the fast path, falling back to the slow glReadPixels codepath if not. This way we keep accelerated compositing working inside WebKit, which gives us nice 3D transforms and less repainting, but take the performance hit in the final &#8220;blit&#8221;.

![Introducing &quot;WebKitClutterGTK+&quot;](https://blog.kov.eti.br/wp-content/uploads/2016/10/20160928_151644-1024x576.jpg "Introducing &quot;WebKitClutterGTK+&quot;")

Another issue we hit was GTK+ not properly updating its knowledge of the window&#8217;s opaque region when painting a frame with GL, which led to some really interesting issues like [a shadow appearing when you tried to shrink the window][6]. There was also an issue where the window would [not use all of the screen when fullscreen][7] which was likely related. Both were fixed.

André Magalhães also worked on a couple of patches we wrote for customer projects and are now pushing upstream. One enables the use of [more than one frontend to connect to a remote web inspector server at once][8]. This can be used to, for instance, show the regular web inspector on a browser window and also use IDE integration for setting breakpoints and so on.

The other patch was cooked by Philip Withnall and helped us deal with some performance bottlenecks we were hitting. It improves [the performance of painting scroll bars][9]. WebKitGTK+ does its own painting of scrollbars (we do not use the GTK+ widgets for various reasons). It turns out painting scrollbars can be quite a hit when the page is being scrolled fast, if not done efficiently.

Emanuele Aina had a great time learning more about meson to figure out a build issue we had when a more recent GStreamer was added to our jhbuild environment. He came out of the experience rather sane, which makes me think meson might indeed be much better than autotools.

![Igalia 15 years cake](https://blog.kov.eti.br/wp-content/uploads/2016/10/20160928_152130-1024x576.jpg "Igalia 15 years cake")

It was a great hackfest, great seeing everyone face to face. We were happy to celebrate Igalia&#8217;s 15 years with them. Hope to see everyone again next year =)

 [1]: http://www.webengineshackfest.org/ "Web Engines Hackfest"
 [2]: https://wiki.apertis.org/Main_Page "Apertis project wiki"
 [3]: https://blog.kov.eti.br/wp-content/uploads/2016/10/20160928_103753.jpg
 [4]: https://bugzilla.redhat.com/show_bug.cgi?id=1378987 "Bug 1378987 - Epiphany 3.22 fails to render pages"
 [5]: https://blog.kov.eti.br/wp-content/uploads/2016/10/20160928_151644.jpg
 [6]: https://bugzilla.gnome.org/show_bug.cgi?id=771553 "Bug 771553 - Shrinking window generates a black patch when gl is used"
 [7]: https://bugzilla.gnome.org/show_bug.cgi?id=771944 "Bug 771944 - Fullscreen does not cover entire screen"
 [8]: https://bugs.webkit.org/show_bug.cgi?id=148902 "Multiple web inspector frontends"
 [9]: https://bugs.webkit.org/show_bug.cgi?id=162673 "Improve performance of painting scrollbars"
 [10]: https://blog.kov.eti.br/wp-content/uploads/2016/10/20160928_152130.jpg