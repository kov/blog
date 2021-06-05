---
title: A clutter port of WebKit
author: kov
type: post
date: 2011-02-18T17:23:53+00:00
url: /2011/02/a-clutter-port-of-webkit/
categories:
  - development
  - English
  - webkit

---
In case you missed the news on webkit-dev, Collabora has been working on developing a clutter port of WebKit. It shares the build system with EFL, and most of the backend code comes from the GTK+ port. That means networking is handled by soup, drawing by cairo, multimedia by GStreamer, and so on.

If you&#8217;d like to give it a try, you can clone the repository from gitorious:

```
$ git clone git://gitorious.org/webkit-clutter/webkit-clutter.git
```

Then to build it you use cmake. From inside the source code directory do this:

```
$ mkdir build
$ cd build
$ cmake .. -DPORT=Clutter -DSHARED_CORE=1 -DBUILD_MX_LIB=1
$ make
```

The BUILD\_MX\_LIB option is optional &#8211; it will build what we call the &#8220;Mx toolkit library&#8221; in addition to the vanilla one. Then you can test that the library is built and working by running the programs inside the &#8220;Programs&#8221; directory. Enjoy!