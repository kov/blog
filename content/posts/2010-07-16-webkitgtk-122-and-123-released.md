---
title: WebKitGTK+ 1.2.2 and 1.2.3 released!
author: kov
type: post
date: 2010-07-16T13:11:21+00:00
url: /2010/07/webkitgtk-122-and-123-released/
categories:
  - English
  - gnome
  - webkit

---
Some of you may have noticed WebKitGTK+ 1.2.2 and 1.2.3 have been uploaded recently. Here&#8217;s their announcement =). A quick summary: if you&#8217;re running the 1.2.x series upgrade to 1.2.3.

Here&#8217;s some information regarding 1.2.2:

1.2.2 is an update to the 1.2.x stable series; along with a lot of crash, and misc fixes the biggest changes are: 1) the inclusion of a new API from the development branch (webkit\_back\_forward\_list\_clear()),  
because it&#8217;s simple and will help with fixing a problem in Epiphany stable, and 2) lots of drag and drop, and clipboard related work by Martin Robinson.

Despite not being strictly fixes, we believe the stable series has a lot to gain from this work; a couple examples should illustrate this better: the changes included fix both a crash when dragging links from WebKit into other browsers, and the annoying bug that made the cursor get stuck in a grab when dragging, sometimes.

> <http://webkitgtk.org/webkit-1.2.2.tar.gz>  
> MD5: 40338001324a38b977c163291e8816d3

Here&#8217;s some information regarding 1.2.3:

To some such a quick succession in releases may look like a brown paper bag was in order. Not strictly, but indeed 1.2.3 aims to fix some oversights with easy fixes. First of all, WebKit was not buildable with ICU 4.4.1, but thankfully a fix had already been checked in to trunk, so 1.2.3 includes that fix. Secondly, Debian&#8217;s Michael Gilbert has done a great job going through all CVEs released about WebKit, and including patches in the Debian package. 1.2.3 includes all of the commits from trunk to fix those, too.

> <http://webkitgtk.org/webkit-1.2.3.tar.gz>  
> MD5: 0ab5c478a6f5b74a1ae96bf13a456662

You can read some more details, including the list of CVEs that were addressed, in the NEWS file:

> <http://gitorious.org/webkitgtk/stable/blobs/master/WebKit/gtk/NEWS>

Enjoy!