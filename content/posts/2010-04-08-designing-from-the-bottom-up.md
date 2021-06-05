---
title: Designing from the bottom up
author: kov
type: post
date: 2010-04-09T02:29:13+00:00
url: /2010/04/designing-from-the-bottom-up/
categories:
  - Collabora
  - development
  - English
  - gnome

---
Have you ever seen, while dealing in a support channel with a novice that just got in touch with the power of UNIX a conversation that goes like this?:

> <novice> How can I process the output of a command, so that any number of spaces gets turned into a newline?  
> <seasoned> What are you trying to do?  
> <novice> I want to list the contents of a directory, but I want one per line.  
> <seasoned> ls -1

I have seen this numerous times, even as one of the actors. At times I was the novice, and many times in #debian-br I was the seasoned person trying to get the novice to focus on the problem they were trying to solve, not on the solution they thought was right.

While reading Máirín Duffy&#8217;s [awesome paper][1] about contributing to Free Software as a designer I couldn&#8217;t help but get that image brought to my memory again, and again. Specially when I read this part:

> This means the language and even the approach FLOSS projects take to solving problems tend to be focused on implementation and technology rather than starting with a real-life user problem to solve and determining appropriate implementation afterwards.

That does sound like us, and it does sound like many of the solutions we come up with. While I was reading her paper, there was a reference I got very interested in checking. It&#8217;s a PDF with no links in it, so I only had the number of the reference. What I would have to do is I would have to scroll to the end of the paper, and find the reference, then somehow come back to the place I was looking at.

My most immediate thought was &#8216;you know, maybe evince should have tabs&#8217;. Why? Because I could open a new tab, go to the place the reference was at, and to &#8216;go back&#8217; I just needed to close the new tab. Other options require much more effort &#8211; remembering the page I was at, or maybe the scroll offset more or less, and scan for the part of the text I was at. But those are not the only options! I could have the application set a marker on where I am, and have an easy command to go back to that marker, for instance, or evince could provide a way of &#8216;looking ahead&#8217; without throwing away the current state at all. I&#8217;m pretty sure if I look around enough I will find tools that solve this problem in a fairly good way.

Now, I think that is exactly how we ended up with tabs in so many places they do not make sense in, and with so many ad-hoc solutions that solve our problems in half-assed ways. Even in browsers, we tend to use tabs as ad-hoc solutions to real problems we have no real solution to handle yet, such as &#8220;I want to check this other thing out real quick, but I do not want to lose any state of this page&#8221;, or &#8220;I want to check this out, but not right now, so let me open it, and then I&#8217;ll come back to it&#8221;, or maybe even &#8220;I want to look at this now, but since it is going to take a while to load, I might as well let it load in the background, and when I finish reading this I can go look at it&#8221;. These are the real problems we have, and I think we need better designs that solve them for real, instead of just patching them with the ad-hoc solution that tabs are.

The other extreme of the spectrum is, of course, not doing something, or even anything for lack of the perfect solution. Using &#8216;this is not a real solution&#8217; as an excuse to not implement something that could serve as a temporary solution to a problem may cause more frustration than having to deal with the ad-hoc solution that is tested, and being applied to other applications for some time. After all, in many cases the ad-hoc solution can be later replaced with a proper one.

I guess this is another instance of the very difficult problem of balancing different realities: proper design is not always available to start something up, specially if the application is backed by individuals and not by a company or a bigger project that could bring in designers to work on it from the start. In this case having something up and running is usually a very important first step in a free software project &#8211; usually required to get enough interest to make it worth designing for.

 [1]: http://mairin.wordpress.com/2010/04/06/contributing-to-free-open-source-software-as-a-designer/ "Máirín's paper"