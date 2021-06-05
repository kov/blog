---
title: WebKitGTK+, and the Page Cache
author: kov
type: post
date: 2010-02-15T23:09:53+00:00
url: /2010/02/webkitgtk-and-the-page-cache/
enclosure:
  - |
    http://kov.eti.br/media/webkit/page-cache.ogv
    6234886
    video/ogg
categories:
  - Collabora
  - development
  - English
  - Epiphany
  - webkit

---
So, one of the things I get to do during work hours for [Collabora][1] is to contribute code, and do maintenance tasks for WebKitGTK+, and have been doing so since early last year, working on all kinds of things, from improving the network backend to handle the real-world web, to fixing scrolling problems, while reviewing patches from the many awesome developers who have been joining us (more on that later =D).

One of the big features I have worked on this past month or so, along with Xan Lopez is the Page Cache. The page cache is a feature of web browsers that makes going back, and forward between pages in the same view very fast. It&#8217;s better explained in this [post][2], but to summarize, the idea is that instead of destroying all the work you have done since downloading the resources, and having to reparse/rebuild the structures the view uses to display the page from the cached resources, you hit pause on the page, and store the whole thing as is, and when coming back to it, you just hit play. You can see in the video two instances of Epiphany, one with the page cache enabled, one with it disabled. Easy to see which was has it enabled. Thanks to KiBi for the suggestion regarding a page that shows this easily =D.

{{< youtube o5Dj8IA9ywo >}}

We initially thought we had this feature enabled, since our initialization functions (that exists since before the current maintainers were involved) did setup the number of desired pages in the cache, but during the hackfest we held in December we found out we were fooled all this time. Enabling the page cache does make going back faster, but also made lots of things become unstable and crash.

Since then, we have been working on figuring out all the problems, and fixing them, using help from adventurous users of in-development software ;D. I believe we&#8217;re now at a point in which I can happily declare the GTK+ port has a working page cache in trunk! If you&#8217;re interested in the nasty details, bear with me!

Let me go back in time a bit, and show you what problems we had. First, some background: the GTK+ port deviates a lot from the other ports when it comes to scrolling. This is because, when designing this part of the port, Holger Freyther had a very nice idea in mind: that the WebView should be a first-class citizen GTK+ scrollable widget. Meaning it would use GTK+&#8217;s adjustments for scrolling, and be able to interact with any parent scrolling widget, be it a GtkScrolledWindow, or a MokoFingerScroll.

We cannot just throw away all the rest of the scrolling code in WebCore, though, that deals with all the details related to interacting with the DOM, and JavaScript code. This means our WebView contains adjustments that need to be set, and unset on our port&#8217;s version of WebKit&#8217;s own representation of the view, called the FrameView, to interact with it, and to get updates on the bounds of content, and such. For every load, in the non-page-cache case, a new FrameView is created, the previous one is destroyed &#8211; this means we need to set the adjustments on every load.

The problem starts when you have the page cache enabled, because the code path used to do what is called &#8220;commit&#8221; the load of a cached page (that is, start replacing the content that is currently being displayed by the one that should now be displayed) is completely different, and we were not setting the adjustments on this new view, so we started with [that][3].

But all was not well. We were still having weird behaviour with scrollbars disappearing, and becoming the wrong size, and worse, crashes when &#8220;back&#8221; was hit. We then started investigating in more detail how it is that the page cache does its magic, to try and figure out the source of all evil.

It turns out that when you leave a page that can be cached, the existing FrameView is no longer destroyed &#8211; it is stored as is in a CachedFrame to be restored if you go back, and a new one is created for the new page. This was having the undesired effect of having the adjustment be set in more than one FrameView at once, causing all kinds of (predictable, after we knew for real what was going on) unwanted effects. Thus, we [reworked the code][4] to make sure the adjustments are only ever set in one FrameView at once, making sure they are unset when the FrameView is being frozen, and reset when it&#8217;s being restored from the page cache.

Last, but not least, it was discovered that going back to a page that contained resources with data: URIs (such as Google results pages which contain a small number of image hits) also caused a crash. This was because our network backend was not storing the data: URI in the ResourceResponse objects it fed into WebCore. The page cache relies on those responses to recreate the requests it uses to artificially replay the load when restoring the page from the page cache, so we [fixed that as well][5].

What can be taken from all this? Building browsers is a lot of hard work! I can&#8217;t think how we could deal with this level of complexity without the awesome testing suite of WebKit. The good news is all of those issues I talked about in this post are now covered by the automate tests that run as part of the normal buildbot cycle in [our bots][6], so we&#8217;re covered for the future, at least for these specific problems =D.

 [1]: http://www.collabora.co.uk/
 [2]: http://webkit.org/blog/427/webkit-page-cache-i-the-basics/
 [3]: http://trac.webkit.org/changeset/54559
 [4]: http://trac.webkit.org/changeset/54620
 [5]: http://trac.webkit.org/changeset/54786
 [6]: http://build.webkit.org/waterfall