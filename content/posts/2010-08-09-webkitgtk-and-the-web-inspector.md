---
title: WebKitGTK+ and the Web Inspector
author: kov
type: post
date: 2010-08-09T15:35:53+00:00
url: /2010/08/webkitgtk-and-the-web-inspector/
enclosure:
  - |
    |
        http://kov.eti.br/media/webkit/inspector.ogv
        14553912
        video/ogg

categories:
  - Collabora
  - development
  - English
  - Epiphany
  - gnome
  - webkit

---
When I started working on WebKitGTK+ I was a web developer, writing IT applications using Python and Django, and building features for content portals running Plone (argh). Even though I was an Epiphany user ever since it was forked off Galeon, I still had to use Firefox for my work, because I couldn&#8217;t really live without Firebug.

It should come as no surprise, then, that one of my first patches to WebKitGTK+ was actually [making the awesome Web Inspector work in our port][1]. After the initial support, though, not a lot has been done to further improve it, partly because it was already good enough for many uses, partly because I somehow started doing non-web development again ;).

These last weeks, through my R&D efforts in Collabora, I have been able to push Web Inspector features and integration a bit further. A simple change that boosts the Inspector&#8217;s usability quite a bit is having the [nodes that are being hovered highlighted][2]. Along with that, the ability to [attach the inspector to Epiphany&#8217;s window][3] should make it easier to use for poking the DOM.

The Web Inspector has a number of settings that control its behaviour. Since, for instance, enabling javascript debugging may slow down javascript performance, the inspector usually has it disabled by default, and provides a button to enable it. It also provides an option for always enabling that feature, but that does not work right now, because we are not saving/restoring the relevant settings. A solution to that is [in the works][4] using the GSettings infrastructure that was recently merged into glib.

Here&#8217;s a simple screencast, showing these improvements in action (click the video to check it out in full size):

{{< youtube _ug0MVB5TGE >}}

 [1]: http://trac.webkit.org/changeset/37982 "Making the inspector work for WebKitGTK+"
 [2]: http://trac.webkit.org/changeset/64567 "Node highlighting in the inspector"
 [3]: http://git.gnome.org/browse/epiphany/commit/?id=83f85841473bea3642e1f694f0b3c5f0d8ca07d4 "Attaching the inspector to Epiphany's window"
 [4]: https://bugs.webkit.org/show_bug.cgi?id=43512 "GSettings integration"