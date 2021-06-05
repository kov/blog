---
title: CEF on Wayland
author: kov
type: post
date: 2017-12-22T11:25:40+00:00
url: /2017/12/cef-on-wayland/
categories:
  - CEF
  - Chromium
  - Collabora
  - English

---
TL;DR: [we have patches for CEF][1] to enable its usage on Wayland and X11 through the Mus/Ozone infrastructure that is to become Chromium&#8217;s streamlined future. And also for [Content Shell][2]!

At Collabora we recently assisted a customer who wanted to upgrade their system from X11 to Wayland. The problem: they use CEF as a runtime for web applications and CEF was not Wayland-ready. They also wanted to have something which was as future-proof and as upstreamable as possible, so the Chromium team&#8217;s plans were quite relevant.

Chromium is at the same time very modular and quite monolithic. It supports several platforms and has slightly different code paths in each, while at the same time acting as a desktop shell for Chromium OS. To make it even more complex, the Chromium team is constantly rewriting bits or doing major refactorings.

That means you&#8217;ll often find several different and incompatible ways of doing something in the code base. You will usually not find clear and stable interfaces, which is where tools like CEF come in, to provide some stability to users of the framework. CEF neutralizes some of the instability, providing a more stable API.

So we started by looking at 1) where is Chromium headed and 2) what kind of integration CEF needed with Chromium&#8217;s guts to work with Wayland? We quickly found that the Chromium team is trying to streamline some of the infrastructure so that it can be better shared among the several use cases, reducing duplication and complexity.

That&#8217;s where the mus+ash (pronounced &#8220;mustache&#8221;) project comes in. It wants to make a better split of the window management and shell functionalities of Chrome OS from the browser while at the same time replacing obsolete IPC systems with Mojo. That should allow a lot more code sharing with the &#8220;Linux Desktop&#8221; version. It also meant that we needed to get CEF to talk Mus.

Chromium already has Wayland support that was built by Intel a while ago for the Ozone display platform abstraction layer. More recently, the [ozone-wayland-dev][3] branch was started by our friends at Igalia to integrate that work with mus+ash, implementing the necessary Mus and Mojo interfaces, window decorations, menus and so on. That looked like the right base to use for our CEF changes.

It took quite a bit of effort and several Collaborans participated in the effort, but [we eventually managed to convince CEF][4] to properly start the necessary processes and set them up for running with Mus and Ozone. Then we moved on to make the use cases our customer cared about stable and to port their internal runtime code.

We contributed touch support for the Wayland Ozone backend, [which we are in the process of upstreaming][5], reported a few bugs on the Mus/Ozone integration, and did some debugging for others, which we still need to figure out [better][6] [fixes][7] for.

For instance, the way Wayland fd polling works does not integrate nicely with the Chromium run loop, since [there needs to be some locking involved][8]. If you don&#8217;t lock/unlock the display for polling, you may end up in a situation in which you&#8217;re told there is something to read and before you actually do the read the GL stack may do it in another thread, causing your blocking read to hang forever (or until there is something to read, like a mouse move). As a work-around, [we avoided the Chromium run loop entirely][9] for Wayland polling.

More recently, we have start working on an internal project for adding [Mus/Ozone support to Content Shell][2], which is a test shell simpler than Chromium the browser. We think it will be useful as a test bed for future work that uses Mus/Ozone and the content API but not the browser UI, since it lives inside the Chromium code base. We are looking forward to upstreaming it soon!

PS: if you want to build it and try it out, here are some instructions:

```
# Check out Google build tools and put them on the path
$ git clone https://chromium.googlesource.com/a/chromium/tools/depot_tools.git
$ export PATH=$PATH:`pwd`/depot_tools

# Check out chromium; note the 'src' after the git command, it is important
$ mkdir chromium; cd chromium
$ git clone -b cef-wayland https://gitlab.collabora.com/web/chromium.git src
$ gclient sync  --jobs 16 --with_branch_heads

# To use CEF, download it and look at or use the script we put in the repository
$ cd src # cef goes inside the chromium source tree
$ git clone -b cef-wayland https://gitlab.collabora.com/web/cef.git
$ sh ./cef/build.sh # NOTE: you may need to edit this script to adapt to your directory structure
$ out/Release_GN_x64/cefsimple --mus --use-views

# To build Content Shell you do not need to download CEF, just switch to the branch and build
$ cd src
$ git checkout -b content_shell_mus_support origin/content_shell_mus_support
$ gn args out/Default --args="use_ozone=true enable_mus=true use_xkbcommon=true"
$ ninja -C out/Default content_shell
$ ./out/Default/content_shell --mus --ozone-platform=wayland
```

 [1]: https://bitbucket.org/chromiumembedded/cef/pull-requests/128/santosh-cef-mus-support/diff "Pull request for CEF/Mus/Ozone"
 [2]: https://gitlab.collabora.com/web/chromium/tree/content_shell_mus_support "Content Shell Mus/Ozone support"
 [3]: https://github.com/Igalia/chromium/tree/ozone-wayland-dev "Ozone Wayland dev branch"
 [4]: https://gitlab.collabora.com/web/cef/commit/1165a2107203c350998dc4521aa529d401b49d94 "First version of working Mus support"
 [5]: https://chromium-review.googlesource.com/c/chromium/src/+/754703 "Patch to enable touch for Wayland/Ozone"
 [6]: https://gitlab.collabora.com/web/chromium/commit/77ee9e5dd93470bc1ff41d507bbf68e7f88062ba "Separate thread for Wayland poll"
 [7]: https://git.collabora.com/cgit/cef-wayland/chromium.git/commit/?h=cef-wayland&id=7aaf1b4a91dc15e9d89085c83d7c992f03519d02 "Work-around for startup hang"
 [8]: https://wayland.freedesktop.org/docs/html/apb.html#Client-classwl__display_1a040dca18775e3177883f06bd6fdf395f "Wayland prepare_read"
 [9]: https://gitlab.collabora.com/web/chromium/commit/77ee9e5dd93470bc1ff41d507bbf68e7f88062ba "Wayland thread"