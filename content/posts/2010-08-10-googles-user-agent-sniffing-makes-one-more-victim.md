---
title: Google’s User-Agent sniffing makes one more victim
author: kov
type: post
date: 2010-08-10T20:38:15+00:00
url: /2010/08/googles-user-agent-sniffing-makes-one-more-victim/
categories:
  - English
  - Epiphany
  - Estupidez humana
  - Google
  - webkit

---
Remember when I said [Epiphany worked out of the box with Youtube&#8217;s WebM][1]? Well, Google has recently decided to [deny us WebM][2], like it did before with Wave, the [Pacman doodle][3], and who knows what else? \o/

Wouldn&#8217;t it be nice if Google practiced what they [preach][4]?

**Update:** so it looks like my message went through to the people who needed to see it, and they found a filtering error in the User Agent sniffing code that made it think Epiphany was a too old Safari &#8211; I&#8217;m told the change will land in Youtube soon, thanks for those paying attention, and working on this! User Agent sniffing keeps being a problem, of course, and there are other stuff to fix, so I will probably still push my patch to spoof the user agent to google services which are still mishandling Epiphany, but it&#8217;s good to see some progress being made!

**Update2:** I started shipping a patch to send the Chrome user agent string to google domains in the Debian package for WebKitGTK+, when the &#8220;enable-site-specific-quirks&#8221; setting is enabled (which is the case for Epiphany); I already found something we were missing out on =D Google Images seems to have been greatly improved, and now faking being Chrome we are also able to enjoy it:

![Google Images improved](/media/webkit/google-images.png)

 [1]: /2010/05/webkitgtk-and-webm/ "Epiphany + WebM"
 [2]: http://bugs.debian.org/cgi-bin/bugreport.cgi?bug=567311#55
 [3]: /2010/05/googles-pacman-doodle-in-epiphanymidori/ "Pacman on Epiphany?"
 [4]: http://code.google.com/p/doctype/wiki/ArticleGoogleChromeCompatFAQ#UserAgent_Detection "Talk is cheap."