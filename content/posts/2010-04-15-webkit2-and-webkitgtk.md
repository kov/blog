---
title: WebKit2 and WebKitGTK+
author: kov
type: post
date: 2010-04-15T13:13:22+00:00
url: /2010/04/webkit2-and-webkitgtk/
categories:
  - Collabora
  - development
  - English
  - Epiphany
  - webkit

---
So you&#8217;ve seen people talking about [WebKit2][1], perhaps have seen someone claiming it &#8220;[drops support for Linux][2]&#8220;, and you&#8217;ve been wondering what the hell that means for WebKitGTK+. Well, welcome to the preemptive Q&A section with WebKitGTK+ maintainers =D. Let&#8217;s first explore some history so we can better understand what exactly is going on.

## What exactly **_is_** WebKit2?

Currently, when we say &#8220;WebKit&#8221; we really mean one of the ports that are built on top of WebCore using the WebKit layer. WebCore is the part that does all of the hard Web-related work, WebKit an API layer that exposes WebCore functionality in a coherent way, so that the platform-specific ports can expose a public API layer for their applications to use, which is usually also called &#8220;WebKit&#8221;. This WebKit layer was designed by Apple to build the Mac, and Windows ports it maintains, and was later released as Free Software so that other ports, such as the GTK+, Qt, EFL ports could be built on top of it, instead of having to do all the heavy lifting from WebCore directly.

![Current WebKit model](/media/webkit/current-webkit.svg)

WebKit2 is nothing more than the second version of that interface, with a whole lot of changes on what you can expect from it, and on how it interacts with WebCore, and the platform-specific API and UI. First of all, the first WebKit was not API stable, and that interface was usually not made public by the various ports &#8211; they only exposed their platform-specific APIs. WebKit2 is being designed to provide a stable, cross-platform, C-based, non-blocking public API. This is huge. It will allow cross-platform code to be written without having to consider language, and port differences for basic functionality.

The second big change is the API will be made fully non-blocking. Currently most things you do are asynchronous already, but some of them may be completed in a synchronous ways (like, loading a string into WebKit instead of an URI). This is important for responsiveness, and is also a very important need for what comes next: process splitting.

WebKit2 will bring into WebKit proper the concept of splitting the UI process from the Web process, similar to what Chromium has. It also much more awesome than what Chrome has for a large number of reasons, including, but not limited to:

  1. It&#8217;s being contributed directly to the WebKit project, in a cross-platform way that lets ports such as WebKitGTK+ take advantage of it, instead of being shipped directly into Safari, like Google does with Chrome;
  2. The process separation goes bellow the API layer, meaning that all complexity involved in managing the process separation is handled by the library, and hopefully none of it leaks to the application using it; that means that applications like Devhelp and Yelp will be able to take advantage of this without having to make their lives more complicated;

There&#8217;s a much better diagram in WebKit2&#8217;s wiki page, but here goes a simplified version that demonstrates what I&#8217;m talking about:

![WebKit2 model](/media/webkit/webkit2.svg)

## What WebKit2 _is not_?

WebKit2 is NOT a rewrite of the whole WebKit stack. Webcore will continue mostly unchanged, and all ports currently building on top of it will keep working. It is also not a fork &#8211; the code lives in the same tree as the current version of WebKit, which will allow us to progressively move towards using this new, improved layer. WebKit2 is not Apple-only, and it is not dropping Linux support. Initial builds of the code that is being landed will likely show up building on Linux in the near future (specially because us porters are already eager to play with it).

## What happens to WebKitGTK+?

In the near future, nothing special. We will continue working towards making it feature-complete, more stable, faster, and rocking on it as always. We will, though, start [working out][3] how we can best take advantage of WebKit2 in order to provide an even more awesome library for the G world. What this means is you can expect us to have a library that will provide a nice GTK+ widget, just like we have today, with a GObject-based API, like we have today, but that is built on top of this new WebKit2 infrastructure, taking advantage of the process-splitting, and the bigger focus on not blocking the UI thread. This should give us a platform that is more stable, and faster and more responsive than what we already have today.

The API is bound to change, of course, but the WebKit2 version of WebKitGTK+ will be a separate, parallel-installable library, and we will keep supporting the WebKit1 version while we work on making the new one at least as good as the current one. This is long term we&#8217;re talking here. We&#8217;ll likely see WebKitGTK+ 1.4, and 1.6 come to life before we are satisfied enough with WebKitGTK+2.

We hope this clears some of the doubts up, and lightens your hearts!

The WebKitGTK+ maintainers.

 [1]: http://trac.webkit.org/wiki/WebKit2
 [2]: http://news.ycombinator.com/item?id=1251452
 [3]: https://bugs.webkit.org/show_bug.cgi?id=37369 "Enabling WebKit2 for GTK+"