---
title: Cool hack – html5tube
author: kov
type: post
date: 2010-01-17T15:42:29+00:00
url: /2010/01/cool-hack-html5tube/
categories:
  - English
  - Epiphany

---
Did I mention I hate flash? I do. It crashes a lot, and is overall a bad thing for the web, in my opinion. But I do enjoy watching videos on the web, and unfortunately, up to this day, flash is what most sites use to show videos. Months ago I read a couple of blog posts with nice hacks to make Firefox able to play youtube videos without using the flash player. Some recent discussions with colleagues at work got me itching to try my hand at something similar for Epiphany.

[![HTML5Tube working][1]][2]

So I went ahead, and wrote [a new extension that does just that][3] &#8211; in youtube video pages, it finds the flash player element, and replaces it with an HTML5 video tag pointing to the actual movie file. This causes the internal HTML5 media player built into WebKitGTK+, that is based on GStreamer, to play the movie. That means you only need to have the necessary GStreamer magic, and the extension enabled, to enjoy the movie.

There are some caveats &#8211; in-video text messages are gone (though I&#8217;m pretty sure we could get them added somehow), playlists, and other places which display videos other than the &#8216;normal&#8217; video watching page are not handled, youtube needs to think you have the flash plugin installed, at least, so the only way to make it work right now is to actually have a flash plugin installed. I think we could probably get away with the last problem somehow, by looking at what the totem youtube plugin code does, for instance, and replicating it.

 [1]: /media/webkit/html5tube.thumb.png
 [2]: /media/webkit/html5tube.png
 [3]: https://bugzilla.gnome.org/show_bug.cgi?id=607034