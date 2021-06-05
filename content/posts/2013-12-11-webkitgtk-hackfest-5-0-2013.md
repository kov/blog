---
title: WebKitGTK+ hackfest 5.0 (2013)!
author: kov
type: post
date: 2013-12-11T09:47:26+00:00
url: /2013/12/webkitgtk-hackfest-5-0-2013/
categories:
  - development
  - English
  - Epiphany
  - gnome
  - webkit

---
For the fifth year in a row the fearless WebKitGTK+ hackers have gathered in A Coruña to bring GNOME and the web closer. Igalia has organized and hosted it as usual, welcoming a record 30 people to its office. The GNOME foundation has sponsored my trip allowing me to fly the cool 18 seats propeller airplane from Lisbon to A Coruña, which is a nice adventure, and have pulpo a feira for dinner, which I simply love! That in addition to enjoying the company of so many great hackers.

![Web with wider tabs and the new prefs dialog](/wp-content/uploads/2013/12/webprefs.png "Web with wider tabs and the new prefs dialog")

The goals for the hackfest have been ambitious, as usual, but we made good headway on them. Web the browser (AKA Epiphany) has seen a ton of little improvements, with Carlos splitting the shell search provider to a separate binary, which allowed us to remove some hacks from the session management code from the browser. It also makes testing changes to Web more convenient again. Jon McCan has been pounding at Web&#8217;s UI making it more sleek, with tabs that expand to make better use of available horizontal space in the tab bar, new dialogs for preferences, cookies and password handling. I have made my [tiny contribution][2] by making it not keep tabs that were created just for what turned out to be a download around. For this last day of hackfest I plan to also fix an issue with text encoding detection and help track down a hang that happens upon page load.

![Martin Robinson and Dan Winship hack](/wp-content/uploads/2013/12/mrobinson-danw.jpg "Martin Robinson and Dan Winship hack")

Martin Robinson and myself have as usual dived into the more disgusting and wide-reaching maintainership tasks that we have lots of trouble pushing forward on our day-to-day lives. Porting our build system to CMake has been one of these long-term goals, not because we love CMake (we don&#8217;t) or because we hate autotools (we do), but because it should make people&#8217;s lives easier when adding new files to the build, and should also make our build less hacky and quicker &#8211; it is sad to see how slow our build can be when compared to something like Chromium, and we think a big part of the problem lies on how complex and dumb autotools and make can be. We have picked up a few of our old branches, brought them up-to-date and landed, which now lets us build the main WebKit2GTK+ library through cmake in trunk. This is an important first step, but [there&#8217;s plenty to do][4].

![Hackers take advantage of the icecream network for faster builds](/wp-content/uploads/2013/12/IMG_20131209_193824_154.jpg "Hackers take advantage of the icecream network for faster builds")

Under the hood, Dan Winship has been pushing HTTP2 support for libsoup forward, with a dead-tree version of the spec by his side. He is refactoring libsoup internals to accomodate the new code paths. Still on the HTTP front, I have been updating soup&#8217;s MIME type sniffing support to match the [newest living specification][6], which includes specification for several new types and [a new security feature introduced by Internet Explorer][7] and later adopted by other browsers. The huge task of preparing the ground for a one process per tab (or other kinds of process separation, this will still be topic for discussion for a while) has been pushed forward by several hackers, with Carlos Garcia and Andy Wingo leading the charge.

![Jon and Guillaume battling code](/wp-content/uploads/2013/12/mccan.jpg "Jon and Guillaume battling code")

Other than that I have been putting in some more work on improving the integration of the new Web Inspector with WebKitGTK+. Carlos has reviewed the patch to allow attaching the inspector to the right side of the window, but we have decided to split it in two, one providing the functionality and one the API that will allow browsers to customize how that is done. There&#8217;s a lot of work to be done here, I plan to land at least this first patch durign the hackfest. I have also [fought one more battle in the never-ending User-Agent sniffing war][9], in which we cannot win, it looks like.

![Hackers chillin&#039; at A Coruña](/wp-content/uploads/2013/12/hackers-acoruna.jpg "Hackers chillin&#039; at A Coruña")

I am very happy to be here for the fifth year in a row, and I hope we will be meeting here for many more years to come! Thanks a lot to Igalia for sponsoring and hosting the hackfest, and to the GNOME foundation for making it possible for me to attend! See you in 2014!

[![](https://www.seekpng.com/png/full/430-4308845_i-can-only-thank-igalia-for-sponsoring-my.png)](http://www.igalia.com/)
[![](https://foundation.gnome.org/wp-content/uploads/sites/12/2021/03/gnome-logos-1.png)](http://foundation.gnome.org/)

 [1]: /wp-content/uploads/2013/12/webprefs.png
 [2]: https://git.gnome.org/browse/epiphany/commit/?id=29c990d5a4662467e4708db1ab1cc06c4901fc7f
 [3]: /wp-content/uploads/2013/12/mrobinson-danw.jpg
 [4]: https://bugs.webkit.org/show_bug.cgi?id=115966
 [5]: /wp-content/uploads/2013/12/IMG_20131209_193824_154.jpg
 [6]: http://mimesniff.spec.whatwg.org/
 [7]: http://msdn.microsoft.com/en-us/library/ie/gg622941(v=vs.85).aspx
 [8]: /wp-content/uploads/2013/12/mccan.jpg
 [9]: https://bugs.webkit.org/show_bug.cgi?id=125444
 [10]: /wp-content/uploads/2013/12/hackers-acoruna.jpg