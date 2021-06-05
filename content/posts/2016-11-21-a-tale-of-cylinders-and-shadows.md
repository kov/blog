---
title: A tale of cylinders and shadows
author: kov
type: post
date: 2016-11-21T17:04:25+00:00
url: /2016/11/a-tale-of-cylinders-and-shadows/
categories:
  - Collabora
  - English
  - Epiphany
  - webkit

---
Like I wrote before, [we at Collabora have been working on improving WebKitGTK+ performance][1] for customer projects, such as [Apertis][2]. We took the opportunity brought by recent improvements to WebKitGTK+ and GTK+ itself to make the final leg of drawing contents to screen as efficient as possible. And then we went on investigating why so much CPU was still being used in some of our test cases.

The first weird thing we noticed is performance was actually degraded on Wayland compared to running under X11. After some investigation we found a lot of time was being spent inside GTK+, painting the window&#8217;s background.

Here&#8217;s the thing: the problem only showed under Wayland because in that case GTK+ is responsible for painting the window decorations, whereas in the X11 case the window manager does it. That means all of that expensive blurring and rendering of shadows fell on GTK+&#8217;s lap.

During the web engines hackfest, a couple of months ago, [I delved deeper into the problem][3] and noticed, [with Carlos Garcia&#8217;s help][4], that it was even worse when HiDPI displays were thrown into the mix. The scaling made things unbearably slower.

You might also be wondering why would painting of window decorations be such a problem, anyway? They should only be repainted when a window changes size or state anyway, which should be pretty rare, right? Right, that is one of the reasons why we had to make it fast, though: [the resizing experience was pretty terrible][5]. But we&#8217;ll get back to that later.

So I dug into that, made a few tries at understanding the issue and came up with a patch showing how applying the blur was being way too expensive. After a bit of discussion with our own Pekka Paalanen and Benjamin Otte we found the root cause: a fast path was not being hit by pixman due to the difference in scale factors on the shadow mask and the target surface. We [made the shadow mask scale the same as the surface&#8217;s][6] and voilà, sane performance.

I keep talking about this being a performance problem, but how bad was it? In the following video you can see how huge the impact in performance of this problem was on my very recent laptop with a HiDPI display. The video starts with an Epiphany window running with a patched GTK+ showing a [nice demo][7] the WebKit folks cooked for CSS animations and 3D transforms.

After a few seconds I quickly alt-tab to the version running with unpatched GTK+ &#8211; I made the window the exact size and position of the other one, so that it is under the same conditions and the difference can be seen more easily. It is massive.

{{< youtube XQVqSms5guU >}}

Yes, all of that slow down was caused by repainting window shadows! OK, so that solved the problem for HiDPI displays, made resizing saner, great! But why is GTK+ repainting the window even if only the contents are changing, anyway? Well, [that turned out to be an off-by-one bug][8] in the code that checks whether the invalidated area includes part of the window decorations.

If the area being changed spanned the whole window width, say, it would always cause the shadows to be repainted. By fixing that, we now avoid all of the shadow drawing code when we are running full-window animations such as the CSS poster circle or gtk3-demo&#8217;s pixbufs demo.

As you can see in the video below, the gtk3-demo running with the patched GTK+ (the one on the right) is using a lot less CPU and has smoother animation than the one running with the unpatched GTK+ (left).

{{< youtube aZEGdpMcgm4 >}}

Pretty much all of the overhead caused by window decorations is gone in the patched version. It is still using quite a bit of CPU to animate those pixbufs, though, so some work still remains. Also, the overhead added to integrate cairo and GL rendering in GTK+ is pretty significant in the WebKitGTK+ CSS animation case. Hopefully that&#8217;ll get much better from GTK+ 4 onwards.

 [1]: https://blog.kov.eti.br/2016/09/webkitgtk-2-14-and-the-web-engines-hackfest/ "WebKitGTK+ 2.14 and the Web Engines Hackfest"
 [2]: http://wiki.apertis.org/ "Apertis project"
 [3]: https://blog.kov.eti.br/2016/10/web-engines-hackfest-2016/ "Web Engines Hackfest 2016!"
 [4]: https://bugzilla.gnome.org/show_bug.cgi?id=772075#c2 "Bugzilla comment saying the problem seemed to be on wayland + hidpi"
 [5]: https://bugzilla.gnome.org/show_bug.cgi?id=774378 "Poor performance when resizing GTK/Wayland applications on high resolution screen"
 [6]: https://bugzilla.gnome.org/show_bug.cgi?id=772075#c8 "Patch making shadow scale the same as target surface's"
 [7]: https://webkit.org/blog-files/3d-transforms/poster-circle.html "Poster circle demo"
 [8]: https://bugzilla.gnome.org/show_bug.cgi?id=774114 "Window shadows are repainted even if only the contents of the window change"