---
title: WebKitGTK+ testing
author: kov
type: post
date: 2010-02-19T21:51:23+00:00
url: /2010/02/webkitgtk-testing/
categories:
  - Collabora
  - English
  - webkit

---
Most times when I blog about WebKitGTK+ it&#8217;s to talk about new features, and their usage in Epiphany. This time I&#8217;d like to tell those who care more about our test infrastructure. Like I said in my [previous post][1], testing is something we take very seriously in WebKit land. It would be hard to get such a complex project, with such diversity of platforms moving forward without automated testing.

Apple hosts a [buildbot master][2] that controls a whole lot of build slaves, for many platforms. Today we added the fourth WebKitGTK+ build slave to the family: 64bits release. This makes up for 4 build slaves in total, 2 release bots sponsored by [Collabora][3], and 2 debug bots sponsored by [Igalia][4]. These bots build WebKitGTK+, and then run what we call the &#8220;layout tests&#8221;. I use quotes there because the name is a bit misleading. Despite being HTML/JavaScript tests, they cover a whole lot of functionality, and tests for regressions in many areas, including security, crashes, animations, media playing, DOM behaviour, and javascript API behaviour. WebKitGTK+ bots currently run 6397 tests, which represent about half the available tests.

Our bots are also, as of today, the first ones to run platform-specific [API tests][5]. Almost a year ago we started writing small tests based on glib&#8217;s GTestSuite, and they have been very valuable in helping us make sure our API expectations aren&#8217;t breaking (at least unknowingly), and to be able to test things that would be very hard to have Layout Tests for. So, yay! Thanks to everyone involved.

Back to layout tests, now: the other half is currently [skipped][6] because of one of three main reasons:

  * We suck, and the test fails for real, either because we are missing implementation of something it uses (say, JavaScript isolated worlds, which has been recently added), or because our implementation is wrong
  * The test is a render dump, and we did not generate results for it yet
  * We lack functionality to run the test in our DumpRenderTree implementation

The first one is the worst of all. It means we have broken functionality, or lack web compatibility. The second one is less bad, we can usually trust layout, and rendering to be good because most of the rendering code is shared (thought there are exceptions, of course). Render dumps are a special way of representing the render tree as text, and we need to generate our own results because of differences in things such as font sizes. The third one is also pretty bad &#8211; it means we cannot test some features; DumpRenderTree is an application that uses the port&#8217;s API to run the tests, and provide additional JavaScript API through JavaScriptCore.

If you feel like helping WebKitGTK+, choosing a bunch of these (specially non-render-dump) skipped tests to make them pass could likely be a good first step =).

 [1]: /2010/02/webkitgtk-and-the-page-cache/
 [2]: http://build.webkit.org/waterfall
 [3]: http://www.collabora.co.uk/
 [4]: http://www.igalia.com/
 [5]: http://build.webkit.org/builders/GTK%20Linux%2032-bit%20Release/builds/9057/steps/API%20tests/logs/stdio
 [6]: http://trac.webkit.org/browser/trunk/LayoutTests/platform/gtk/Skipped