---
title: WebKitGTK+ 2.14 and the Web Engines Hackfest
author: kov
type: post
date: 2016-09-22T17:03:36+00:00
url: /2016/09/webkitgtk-2-14-and-the-web-engines-hackfest/
categories:
  - Collabora
  - English
  - webkit

---
Next week our friends at [Igalia][1] will be hosting this year&#8217;s [Web Engines Hackfest][2]. [Collabora][3] will be there! We are [gold sponsors][4], and have three developers attending. It will also be an opportunity to celebrate Igalia&#8217;s 15th birthday \o/. Looking forward to meet you there! =)

[Carlos Garcia has recently released WebKitGTK+ 2.14][5], the latest stable release. This is a great release that brings a lot of improvements and works much better on Wayland, which is becoming mature enough to be used by default. In particular, it fixes the clipboard, which was one of the main missing features, thanks to Carlos Garnacho! We have also been able to contribute a bit to this release =)

One of the biggest changes this cycle is the threaded compositor, which was implemented by Igalia&#8217;s Gwang Yoon Hwang. This work improves performance by not stalling other web engine features while compositing. Earlier this year we contributed fixes to make the threaded compositor work with the web inspector and fixed elements, helping with the goal of enabling it by default for this release.

Wayland was also lacking an accelerated compositing implementation. There was a patch to add a nested Wayland compositor to the UIProcess, with the WebProcesses connecting to it as Wayland clients to share the final rendering so that it can be shown to screen. It was not ready though and there were questions as to whether that was the way to go and alternative proposals were floating around on how to best implement it.

At last year&#8217;s hackfest we had discussions about what the best path for that would be where collaborans Emanuele Aina and Daniel Stone (proxied by Emanuele) [contributed quite a bit][6] on figuring out how to implement it in a way that was both efficient and platform agnostic.

We later picked up the old patchset, rebased on the then-current master and [made it run efficiently][7] as proof of concept for the [Apertis][8] project on an i.MX6 board. This was done using the fancy GL support that landed in GTK+ in the meantime, with some API additions and shortcuts to sidestep performance issues. The work was sponsored by [Robert Bosch Car Multimedia][9].

{{< youtube j664ERyjWNQ >}}

Igalia managed to improve and land a very well designed patch that implements the nested compositor, though it was still not as efficient as it could be, as it was using glReadPixels to get the final rendering of the page to the GTK+ widget through cairo. I have improved that code by [ensuring we do not waste memory when using HiDPI][10].

As part of our proof of concept investigation, we got this [WebGL car visualizer][11] running quite well on our sabrelite imx6 boards. Some of it went into the upstream patches or proposals mentioned below, but we have a bunch of potential improvements still in store that we hope to turn into upstreamable patches and advance during next week&#8217;s hackfest.

One of the improvements that already landed was an alternate code path that leverages GTK+&#8217;s recent GL super powers to [render using gdk\_cairo\_draw\_from\_gl()][12], avoiding the expensive copying of pixels from the GPU to the CPU and making it go faster. That improvement exposed a [weird bug in GTK+][13] that causes a black patch to appear when shrinking the window, which I have a tentative fix for.

We originally [proposed to add a new gdk\_cairo\_draw\_from\_egl()][14] to use an EGLImage instead of a GL texture or renderbuffer. On our proof of concept we noticed it is even more efficient than the texturing currently used by GTK+, and could give us even better performance for WebKitGTK+. Emanuele Bassi thinks it might be better to add EGLImage as another code branch inside from_gl() though, so we will look into that.

Another very interesting igalian addition to this release is support for the MemoryPressureHandler even on systems with no cgroups set up. The memory pressure handler is a WebKit feature which flushes caches and frees resources that are not being used when the operating system notifies it memory is scarce.

We worked with the Raspberry Pi Foundation to add support for that feature to the [Raspberry Pi browser][15] and contributed it upstream back in 2014, when Collabora was trying to squeeze as much as possible from the hardware. We had to add a cgroups setup to wrap Epiphany in, back then, so that it would actually benefit from the feature.

With this improvement, it will benefit even without the custom cgroups setups as well, by having the UIProcess monitor memory usage and notify each WebProcess when memory is tight.

Some of these improvements were achieved by developers getting together at the Web Engines Hackfest last year and laying out the ground work or ideas that ended up in the code base. I look forward to another great few days of hackfest next week! See you there o/

 [1]: http://igalia.com/ "Igalia"
 [2]: http://www.webengineshackfest.org/ "Web Engines Hackfest"
 [3]: http://collabora.com/ "Collabora Ltd."
 [4]: http://www.webengineshackfest.org/#sponsors "Web Engines Hackfest Sponsors"
 [5]: https://blogs.igalia.com/carlosgc/2016/09/20/webkitgtk-2-14/ "WebKitGTK+ 2.14"
 [6]: https://lists.webkit.org/pipermail/webkit-gtk/2015-December/002465.html "webkit-gtk mailing list discussion"
 [7]: https://git.apertis.org/cgit/webkit-gtk-clutter.git/log/?h=wayland-ac "wayland-ac branch of the WebKitGTK+ engine with Clutter integration"
 [8]: http://www.apertis.org/ "Apertis"
 [9]: http://www.bosch.com/en/com/bosch_group/business_sectors_divisions/automotive_technology/car_multimedia/car-multimedia.html "Roberto Bosch Car Multimedia"
 [10]: https://trac.webkit.org/changeset/206045 "Changeset 206045"
 [11]: http://carvisualizer.plus360degrees.com/threejs/ "Car visualizer"
 [12]: https://trac.webkit.org/changeset/206080 "Changeset 206080"
 [13]: https://bugzilla.gnome.org/show_bug.cgi?id=771553 "Bug 771553 - Shrinking window generates a black patch when gl is used"
 [14]: https://bugzilla.gnome.org/show_bug.cgi?id=769739 "Bug 769739 - Add a more efficient way to draw EGLImages, gdk_cairo_draw_from_egl_image()"
 [15]: https://www.raspberrypi.org/blog/web-browser-beta/ "WEB BROWSER BETA"