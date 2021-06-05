---
title: QtWebKit is no more, what now?
author: kov
type: post
date: 2014-03-18T19:44:07+00:00
url: /2014/03/qtwebkit-is-no-more-what-now/
categories:
  - Collabora
  - webkit

---
Driven by the technical choices of some of our early clients, QtWebKit was one of the first web engines [Collabora][1] worked on, building the initial support for NPAPI plugins and more. Since then we had kept in touch with the project from time to time when helping clients with specific issues, hardware or software integration, and particularly GStreamer-related work.

With Google forking Blink off WebKit, a decision had to be made by all vendors of browsers and platform APIs based on WebKit on whether to stay or follow Google instead. After quite a bit of consideration and prototyping, the Qt team decided to take the second option and build the [QtWebEngine][2] library to replace QtWebKit.

The main advantage of WebKit over Blink for engine vendors is the ability to implement custom platform support. That meant QtWebKit was able to use Qt graphics and networking APIs and other Qt technologies for all of the platform-integration needs. It also enjoyed the great flexibility of using GStreamer to implement HTML5 media. GStreamer brings hardware-acceleration capabilities, support for several media formats and the ability to expand that support without having to change the engine itself. 

People who are using QtWebKit because of its being Gstreamer-powered will probably be better served by switching to one of the remaining GStreamer-based ports, such as WebKitGTK+. Those who don&#8217;t care about the underlying technologies but really need or want to use Qt APIs will be better served by porting to the new QtWebEngine.

It&#8217;s important to note though that QtWebEngine drops support for Android and iOS as well as several features that allowed tight integration with the Qt platform, such as DOM manipulation through the [QWebElement APIs][3], making [QObject instances available to web applications][4], and the ability to [set the QNetworkAccessManager][5] used for downloading resources, which allowed for fine-grained control of the requests and sharing of cookies and cache.

It might also make sense to go Chromium/Blink, either by using the [Chrome Content API][6], or switching to one its siblings (QtWebEngine included) if the goal is to make a browser which needs no integration with existing toolkits or environments. You will be limited to the formats supported by Chrome and the hardware platforms targeted by Google. Blink does not allow multiple implementations of the platform support layer, so you are stuck with what upstream decides to ship, or with a fork to maintain.

It is a good alternative when Android itself is the main target. That is the technology used to build its main browser. The main advantage here is you get to follow Chrome&#8217;s fast-paced development and great support for the targeted hardware out of the box. If you need to support custom hardware or to be flexible on the kinds of media you would like to support, then WebKit still makes more sense in the long run, since that support can be maintained upstream.

At Collabora we&#8217;ve dealt with several WebKit ports over the years, and still actively maintain the custom WebKit Clutter port out of tree for clients. We have also done quite a bit of work on Chromium-powered projects. Some of the decisions you have to make are not easy and we believe we can help. Not sure what to do next? If you have that on your plate, [get in touch][7]!

 [1]: http://collabora.com/ "Collabora"
 [2]: http://qt-project.org/wiki/QtWebEngine "QtWebEngine"
 [3]: http://qt-project.org/doc/qt-5/qwebelement.html
 [4]: http://qt-project.org/doc/qt-5/qtwebkit-bridge.html
 [5]: http://qt-project.org/doc/qt-5/qwebpage.html#setNetworkAccessManager
 [6]: http://www.chromium.org/developers/content-module/content-api
 [7]: https://www.collabora.com/contact/