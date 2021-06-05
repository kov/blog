---
title: Google’s pacman doodle in Epiphany/Midori?
author: kov
type: post
date: 2010-05-21T15:59:32+00:00
url: /2010/05/googles-pacman-doodle-in-epiphanymidori/
categories:
  - English
  - Google
  - webkit

---
Google has had a very nice idea today, to celebrate Pacman&#8217;s aniversary: they made their logo become a playable <del datetime="2010-05-21T22:17:07+00:00">HTML5</del> pacman. If you&#8217;re wondering why your WebKitGTK+ browser is not being able to play the game here&#8217;s why: Google is doing User-Agent sniffing and denying you the fun, sending a static image that you can click to perform a search instead of the game.

If you make Epiphany or Midori identify themselves as Chrome or Firefox, the game will work. I really don&#8217;t get this User Agent sniffing bullshit coming from Google. If you go to gconf-editor, and under epiphany->general set the user_agent key to &#8220;Mozilla/5.0 (X11; U; Linux; en-gb; rv:1.9.0.2) Gecko/2008092313 Firefox/3.8&#8221; it works. I&#8217;m starting to seriously consider User Agent spoofing for *.google.com as a quirk on WebKitGTK+. Lame.

**Update:** as a protest, I&#8217;m making blog.kov.eti.br and kov.eti.br say Chrome/Chromium are not supported, by doing User Agent sniffing.  

**Update2:** it&#8217;s been pointed out to me that the game is not HTML5 &#8211; it&#8217;s actually smart usage of divs, and flash \*urgh\* for the audio  

**Update3:** thanks to a friend who works at Google getting in touch with pacman&#8217;s designer, it looks like it now works without faking U-A &#8211; I&#8217;m happy for this, thank you! Despite this good step forward, google is still denying us the nice fade in effect, and still sees us as &#8216;unsupported&#8217; in Wave and similar products, so I&#8217;ll keep my protest for now.