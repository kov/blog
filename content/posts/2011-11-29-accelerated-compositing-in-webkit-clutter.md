---
title: Accelerated Compositing in webkit-clutter
author: kov
type: post
date: 2011-11-29T17:55:03+00:00
url: /2011/11/accelerated-compositing-in-webkit-clutter/
categories:
  - Collabora
  - English
  - gnome
  - webkit

---
For a while now my fellow Collaboran Joone Hur has been working on implementing the Accelerated Compositing infrastructure available in WebKit in webkit-clutter, so that we can use Clutter&#8217;s powers for compositing separate layers and perform animations. This work is being done by Collabora and is sponsored by BOSCH, whom I&#8217;d like to thank! What does all this mean, you ask? Let me tell me a bit about it.

The way animations usually work in WebKit is by repainting parts of the page every few milliseconds. What that means in technical terms is that an area of the page gets invalidated, and since the whole page is one big image, all of the pieces that are in that part of the page have to be repainted: the background, any divs, images, text that are at that part of the page.

What the accelerated compositing code paths allow is the creation of separate pieces to represent some of the layers, allowing the composition to happen on the GPU, removing the need to perform lots of cairo paint operations per second in many cases. So if we have a semi-transparent video moving around the page, we can have that video be a separate texture that is layered on top of the page, made transparent and animated by the GPU. In webkit-clutter&#8217;s case this is done by having separate actors for each of the layers.

I have been looking at this code on and off, and recently joined Joone in the implementation of some of the pieces. The accelerated compositing infrastructure was originally built by Apple and is, for that reason, works in a way that is very similar to Core Animation. The code is still a bit over the place as we work on figuring out how to best translate the concepts into clutter concepts and there are several bugs, but some cool demos are already possible! Bellow you have one of the CSS3 demos that were made by Apple to demo this new functionality running on our MxLauncher test browser.

{{< youtube y3EtFUS15qo >}}

You can also see that the non-Accelerated version is unable to represent the 3D space correctly. Also, can you guess which of the two MxLauncher instances is spending less CPU? 😉 In this second video I show the debug borders being painted around the actors that were created to represent layers.

{{< youtube 3IkZixEjUbo >}}

The code, should you like to peek or test is available in the ac2 branch of our webkit-clutter repository: <http://gitorious.org/webkit-clutter/webkit-clutter/commits/ac2>

We still have plenty of work to do, so expect to hear more about it. During our annual hackfest in A Coruña we plan to discuss how this work could be integrated also in the WebKitGTK+ port, perhaps by taking advantage of clutter-gtk, which would benefit both ports, by sharing code and maintenance, and providing this great functionality to Epiphany users. Stay tuned!