---
title: My experience with GNOME 3 so far
author: kov
type: post
date: 2011-05-13T23:56:32+00:00
url: /2011/05/my-experience-with-gnome-3-so-far/
openid_comments:
  - 'a:1:{i:0;s:4:"8089";}'
categories:
  - English
  - gnome

---
You know, GNOME Shell and I are not really strangers to each other for a long time now. I have been using it almost daily as my main desktop since late 2009, when I started shipping it to Debian experimental. That means that I had ample opportunity to both get used to it and witness the huge improvements it had with every new release.

My general feeling towards GNOME 3 is this: ♥. Yes, I love it! I love the new themes, I love the window borders, I love the top panel, the overview, the dynamic workspaces, I love the Me menu, I love the clock and the calendar, the system indicators with the beautiful symbolic icons, being able to search for applications in such a nice way, the window animations, the multi-screen support, the new nautilus, the dash, looking glass. It&#8217;s hard for me to even express how thankful I am and how much admiration I have for the awesome folks who helped bring this to life. Thanks so much!

![My GNOME3 desktop](/wp-content/uploads/2011/05/MyGNOME3.png "My GNOME3 desktop")

After all this time, there are only two things I can say I dislike about GNOME3, apart from some minor wishlists: the alt-tab behaviour and the message tray. Let me expand on those.

### Message Tray

Of the very few things I dislike, there is only 1 I hate and cannot see myself living with: the accordion animation in the message tray. No, really, it&#8217;s such a terrible, terrible idea. Every single time I try to use that thing I overshoot while moving the mouse to the left, then overshoot again moving the mouse to the right because the frigging icon has moved. It&#8217;s no good knowing that I can click in the text or in the empty area to its right, it feels wrong. It&#8217;s terrible that my actual target is moving at all. Every single time I use it is a [small frustration][2] for me &#8211; it&#8217;s as if the message tray was playing games with me, laughing at me for not having good enough mouse pointer driving skills &#8211; even more than the infamous sub-menus used to. And I had to go through that penance whenever I wanted to find the person I was chating with to resume the conversation.

I wrote [an extension][3] that disables the accordion animation by simply not showing the title at all when you hover the icon, and I patched GNOME Shell&#8217;s CSS to make the icons a bit bigger, so that it&#8217;s easier for me to hit them with the mouse. It&#8217;s clearly not ideal, and you still have to click the various &#8220;people&#8221; icons to figure out which of your friends who were lazy enough to not add a picture to their IM profiles is that one, but it&#8217;s still much better than chasing the (smaller) ones around to figure that out. Perhaps we should have the icons be bigger and always have the title bellow them? I don&#8217;t know, I trust the awesome designers who designed the awesomess that&#8217;s everywhere else will come up with a great idea.

![My Message Tray](/wp-content/uploads/2011/05/message-tray.png "My Message Tray")

### Alt Tab

The number 1 feature of workspaces for me has always been locality &#8211; being able to **not** see all of the other applications and windows that are open elsewhere. This lowers the amount of noise when I&#8217;m trying to find something. The overview is very nice in this matter &#8211; only windows in the current workspace are shown, and even when you have an extra screen, the windows in there appear in that screen.

The alt-tab behaviour, on the other hand, of showing all windows and apps, even with the separator, bothers me. It&#8217;s really useful to have when you want to go to a specific window no matter where it is (I usually use the dash for that, though), but it adds noise when you want to go to a specific window \_in this\_ workspace, which is the most common use case for my usage. So I copied the alt-tab code over to an extension and modified it so that only windows in the current workspace would be considered.

In addition to that, with the windows in the extra screen always being there no matter what workspace you&#8217;re in (which I think is an awesome idea), they are effectively in all workspaces, so they add constant noise even with my extension. It&#8217;s also weird to have to look at the main screen to switch to a window in the extra screen. There&#8217;s a huge discontinuity. What I would prefer is having alt-tab to follow the mouse regarding screens &#8211; only show windows in the extra screen if you hit alt-tab in there, making sure the selector thing appears in there as well.

 [1]: http://blog.kov.eti.br/wp-content/uploads/2011/05/MyGNOME3.png
 [2]: http://www.joelonsoftware.com/uibook/chapters/fog0000000057.html
 [3]: http://kov.eti.br/~kov/kovtray.tar.gz
 [4]: http://blog.kov.eti.br/wp-content/uploads/2011/05/message-tray.png