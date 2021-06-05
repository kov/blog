---
title: WebKitGTK+ Debian packaging repository changes
author: kov
type: post
date: 2012-03-10T17:32:19+00:00
url: /2012/03/webkitgtk-packaging-repository-changes/
categories:
  - Debian
  - English
  - webkit

---
For a while now the git repository used for packaging WebKitGTK+ has been broken. Broken as in nobody was able to clone it. In addition to that, the packaging workflow had been changing over time, from a track-upstream-git/patches applied one to a import-orig-only/patches-not-applied one.

After spending some more time trying to unbreak the repository for the third time I decided it might be a good time for a clean up. I created a [new repository][1], imported all upstream versions for series 1.2.x (which is in squeeze), 1.6.x (unstable), and 1.7.x (experimental). I also imported packaging-related commis for those versions using git format-patch and black magic.

One of the good things about doing this move, and which should make hacking the WebKitGTK+ debian package more pleasant and accessible can be seen here:

```
kov@goiaba ~/s/debian-webkit> du -sh webkit/.git webkit.old/.git
27M     webkit/.git
1.6G    webkit.old/.git
```

If you care about the old repository, it&#8217;s on git.debian.org still, named [old-webkit.git][2]. Enjoy!

 [1]: http://anonscm.debian.org/gitweb/?p=pkg-webkit/webkit.git;a=summary
 [2]: http://anonscm.debian.org/gitweb/?p=pkg-webkit/old-webkit.git;a=summary