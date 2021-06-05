---
title: Web Engines Hackfest 2014
author: kov
type: post
date: 2014-12-15T23:20:52+00:00
url: /2014/12/web-engines-hackfest-2014/
categories:
  - Collabora
  - development
  - English
  - webkit

---
For the 6th year in a row, Igalia has organized a hackfest focused on web engines. The 5 years before this one were actually focused on the GTK+ port of WebKit, but the number of web engines that matter to us as Free Software developers and consultancies has grown, and so has the scope of the hackfest.

It was a very productive and exciting event. It has already been covered by [Manuel Rego][1], [Philippe Normand][2], [Sebastian Dröge][3] and [Andy Wingo][4]! I am sure more blog posts will pop up. We had [Martin Robinson][5] telling us about the new Servo engine that Mozilla has been developing as a proof of concept for both Rust as a language for building big, complex products and for doing layout in parallel. Andy gave us a very good summary of where JS engines are in terms of performance and features. We had talks about CSS grid layouts, TyGL &#8211; a GL-powered implementation of the 2D painting backend in WebKit, the new Wayland port, [announced by Zan Dobersek][6], and a lot more.

With help from my colleague ChangSeok OH, I presented a description of how a team at Collabora led by Marco Barisione made the combination of [WebKitGTK+ and GNOME&#8217;s web browser a pretty good experience for the Raspberry Pi][7]. It took a not so small amount of both pragmatic limitations and hacks to get to a multi-tab browser that can play youtube videos and be quite responsive, but we were very happy with how well WebKitGTK+ worked as a base for that.

One of my main goals for the hackfest was to help drive features that were lingering in the bug tracker for WebKitGTK+. I picked up a patch that had gone through a number of iterations and rewrites: the HTML5 notifications support, and with help from Carlos Garcia, managed to finish it and [land it at the last day of the hackfest][8]! It provides new signals that can be used to authorize notifications, show and close them.

To make notifications work in the best case scenario, the only thing that the API user needs to do is handle the permission request, since we provide a default implementation for the show and close signals that uses libnotify if it is available when building WebKitGTK+. Originally our intention was to use GNotification for the default implementation of those signals in WebKitGTK+, but it turned out to be a pain to use for our purposes.

GNotification is tied to GApplication. This allows for some interesting features, like notifications being persistent and able to reactivate the application, but those make no sense in our current use case, although that may change once [service workers][9] become a thing. It can also be a bit problematic given we are a library and thus have no GApplication of our own. That was easily overcome by using the default GApplication of the process for notifications, though.

The show stopper for us using GNotification was the way GNOME Shell currently deals with notifications sent using this mechanism. It will [look for a .desktop file named after the application ID][10] used to initialize the GApplication instance and reject the notification if it cannot find that. Besides making this a pain to test &#8211; our test browser would need a .desktop file to be installed, that would not work for our main API user! The application ID used for all Web instances is _org.gnome.Epiphany_ at the moment, and that is not the same as any of the desktop files used either by the main browser or by the web apps created with it.

For the future we will probably move Epiphany towards this new era, and all users of the WebKitGTK+ API as well, but the strictness of GNOME Shell would hurt the usefulness of our default implementation right now, so we decided to stick to libnotify for the time being.

Other than that, I managed to review a bunch of patches during the hackfest, and took part in many interesting discussions regarding the next steps for GNOME Web and the GTK+ and Wayland ports of WebKit, such as the [potential introduction of a threaded compositor][11], which is pretty exciting. We also tried to have Bastien Nocera as a guest participant for one of our sessions, but it turns out that requires more than a notebook on top of a bench hooked up to   a TV to work well. We could think of something next time ;D.

I&#8217;d like to thank [Igalia][12] for organizing and sponsoring the event, [Collabora][13] for sponsoring and sending ChangSeok and myself over to Spain from far away Brazil and South Korea, and [Adobe][14] for also sponsoring the event! Hope to see you all next year!

![Web Engines Hackfest 2014 sponsors: Adobe, Collabora and Igalia](http://blogs.igalia.com/mrego/files/2014/12/web-engines-hackfest-2014-sponsors.png "Web Engines Hackfest 2014 sponsors: Adobe, Collabora and Igalia")

 [1]: http://blogs.igalia.com/mrego/2014/12/10/web-engines-hackfest-2014/ "Web Engines Hackfest 2014"
 [2]: http://base-art.net/Articles/web-engines-hackfest-2014/ "Web Engines Hackfest 2014"
 [3]: https://coaxion.net/blog/2014/12/web-engines-hackfest-2014/ "Web Engines Hackfest 2014"
 [4]: http://wingolog.org/archives/2014/12/09/state-of-js-implementations-2014-edition "state of js implementations, 2014 edition"
 [5]: http://abandonedwig.info/ "Martin Robinson"
 [6]: http://blogs.igalia.com/zdobersek/2014/12/09/announcing-webkit-for-wayland/ "Announcing WebKit for Wayland"
 [7]: http://blog.barisione.org/2014-09/rpi-browser/ "A web browser for the Raspberry Pi"
 [8]: http://trac.webkit.org/changeset/177073 "Changeset 177073"
 [9]: http://www.w3.org/TR/2014/WD-service-workers-20141118/ "Service Workers"
 [10]: https://git.gnome.org/browse/gnome-shell/tree/js/ui/notificationDaemon.js?h=gnome-3-14#n631
 [11]: http://blog.ryumiel.net/articles/introducing-threaded-compositor/ "Introducing Threaded Compositor"
 [12]: http://www.igalia.com "Igalia"
 [13]: http://www.collabora.com/ "Collabora"
 [14]: http://webplatform.adobe.com/ "Adobe Web Platform"