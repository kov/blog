---
title: Who knew we still had low-hanging fruits?
author: kov
type: post
date: 2017-10-16T18:23:45+00:00
url: /2017/10/who-knew-we-still-had-low-hanging-fruits/
openid_comments:
  - 'a:1:{i:0;i:33605;}'
categories:
  - Collabora
  - English
  - Epiphany
  - Mozilla
  - webkit

---
Earlier this month I had the pleasure of attending the Web Engines Hackfest, hosted by Igalia at their offices in A Coruña, and also sponsored by my employer, [Collabora][1], Google and Mozilla. It has grown a lot and we had many new people this year.

Fun fact: I am one of the 3 or 4 people who have attended all of the editions of the hackfest since its inception in 2009, when it was called WebKitGTK+ hackfest \o/

![](/wp-content/uploads/2017/10/20171002_204405-1024x576.jpg)

It was a great get together where I met many friends and made some new ones. Had plenty of discussions, mainly with [Antonio Gomes][3] and Google&#8217;s Robert Kroeger, about the way forward for Chromium on Wayland.

We had the opportunity of explaining how we at Collabora cooperated with igalians to implemented and optimise a Wayland nested compositor for WebKit2 to share buffers between processes in an efficient way even on broken drivers. Most of the discussions and some of the work that led to this was done in [previous][4] [hackfests][5], by the way!

![](https://blog.kov.eti.br/wp-content/uploads/2017/10/20171002_193518-1024x576.jpg)

The idea seems to have been mostly welcomed, the only concern being that Wayland&#8217;s interfaces would need to be tested for security (fuzzed). So we may end up going that same route with Chromium for allowing process separation between the UI and GPU (being renamed Viz, currently) processes.

On another note, and going back to the title of the post, at Collabora we have recently adopted [Mattermost][7] to replace our internal IRC server. Many Collaborans have decided to use Mattermost through an Epiphany Web Application or through a simple Python application that just shows a GTK+ window wrapping a WebKitGTK+ WebView.

![](https://blog.kov.eti.br/wp-content/uploads/2017/10/20171002_101952-1024x576.jpg)

Some people noticed that when the connection was lost Mattermost would take a very long time to notice and reconnect &#8211; its web sockets were taking a long, long time to timeout, according to our colleague Andrew Shadura.

I did some quick searching on the codebase and noticed WebCore has a NetworkStateNotifier interface that it uses to get notified when connection changes. That was not implemented for WebKitGTK+, so it was likely what caused stuff to linger when a connection hiccup happened. Given we have [GNetworkMonitor][9], implementation of the missing interfaces required only [3 lines of actual code][10] (plus the necessary boilerplate)!

![](https://blog.kov.eti.br/wp-content/uploads/2017/10/Screenshot-from-2017-10-16-11-13-39-1024x236.png)

I was surprised to still find such as low hanging fruit in WebKitGTK+, so I decided to look for more. Turns out WebCore also has a notifier for low power situations, which was implemented only by the iOS port, and causes the engine to throttle some timers and avoid some expensive checks it would do in normal situations. This required a few more lines to implement using upower-glib, but [not that many either][12]!

That was the fun I had during the hackfest in terms of coding. Mostly I had fun just lurking in break out sessions discussing the past, present and future of tech such as WebRTC, Servo, Rust, WebKit, Chromium, WebVR, and more. I also beat a few challengers in Street Fighter 2, as usual.

I&#8217;d like to say thanks to Collabora, Igalia, Google, and Mozilla for sponsoring and attending the hackfest. Thanks to Igalia for hosting and to Collabora for sponsoring my attendance along with two other Collaborans. It was a great hackfest and I&#8217;m looking forward to the next one! See you in 2018 =)

 [1]: http://collabora.com/ "Collabora Ltd."
 [2]: https://blog.kov.eti.br/wp-content/uploads/2017/10/20171002_204405.jpg
 [3]: https://blogs.igalia.com/tonikitoo/
 [4]: https://blog.kov.eti.br/2016/09/webkitgtk-2-14-and-the-web-engines-hackfest/ "WebKitGTK+ 2.14 and the Web Engines Hackfest"
 [5]: https://blog.kov.eti.br/2016/10/web-engines-hackfest-2016/ "Web Engines Hackfest 2016!"
 [6]: https://blog.kov.eti.br/wp-content/uploads/2017/10/20171002_193518.jpg
 [7]: https://about.mattermost.com/
 [8]: https://blog.kov.eti.br/wp-content/uploads/2017/10/20171002_101952.jpg
 [9]: https://developer.gnome.org/gio/stable/GNetworkMonitor.html
 [10]: https://bugs.webkit.org/attachment.cgi?id=322383&action=prettypatch
 [11]: https://blog.kov.eti.br/wp-content/uploads/2017/10/Screenshot-from-2017-10-16-11-13-39.png
 [12]: https://bugs.webkit.org/attachment.cgi?id=322532&action=prettypatch