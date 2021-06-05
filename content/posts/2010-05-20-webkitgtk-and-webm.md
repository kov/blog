---
title: WebKitGTK+ and WebM
author: kov
type: post
date: 2010-05-20T18:24:30+00:00
url: /2010/05/webkitgtk-and-webm/
categories:
  - Collabora
  - English
  - Epiphany
  - webkit

---
So you probably heard about [WebM][1], right? It&#8217;s the awesome new media format being pushed by Google and a large number of partners, including [Collabora][2], following the release of the VP8 video codec free of royalties and patents, along with a Free Software implementation.

It turns out that if you are a user or developer of applications that use the GStreamer framework, you can start taking advantage of all that freedom right away! Collabora Multimedia has developed, along with Entropy Wave [GStreamer support for the new format][3], and the code has already landed in the public repositories, and is already being packaged for some distributions.

I just couldn&#8217;t wait the few days it will take for the support to be properly landed in Debian unstable, so I went ahead and downloaded all of the current packages from the [pkg-gstreamer svn repository][4], built everything after having the libvpx-dev package installed, and went straight to a rather unknown, small video site called Youtube with my GStreamer-powered WebKitGTK+-based browser, Epiphany!:

[  
![Youtube showing a webm video in Epiphany][5]  
][6] 

If you&#8217;re running Debian unstable, or any of the other distributions which will be lucky to get the new codecs, and support packages soon, you should be able to get this working out of the box real soon now. Check the [tips on WebM&#8217;s web site][7] on how to find WebM videos on youtube.

 [1]: http://www.webmproject.org/
 [2]: http://www.collabora.co.uk/
 [3]: http://www.collabora.co.uk/press/2010/05/join-google-webm-project.html
 [4]: http://svn.debian.org/viewsvn/pkg-gstreamer/
 [5]: http://kov.eti.br/media/webkit/webm-youtube.small.png
 [6]: http://kov.eti.br/media/webkit/webm-youtube.png
 [7]: http://www.webmproject.org/users/