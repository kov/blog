---
title: Content-Encoding in soup – all your gzip are belong to us
author: kov
type: post
date: 2010-01-17T13:28:51+00:00
url: /2010/01/content-encoding-in-soup-all-your-gzip-are-belong-to-us/
categories:
  - Collabora
  - English
  - webkit

---
One thing everyone forgot to talk about the WebKitGTK+ hackfest was that master Dan Winship added basic Content-Encoding support to libsoup, and patched WebKitGTK+ to use it. If you are using a recent enough version of those you will finally be able to visit web sites that send gzipped content despite the browser saying it could not handle it, like the [Internet Archive][1].

This was one of those cases in which the web shows all of its potential to behave weirdly. The [HTTP/1.1 RFC][2] says that if an _Accept-Encoding_ header is not present, the server MAY assume the client accepts any encoding, so we were having many sites send us gzip content even though we did not support it. We then [started sending][3] a header saying &#8220;we support identity, and nothing else!&#8221;.

It turns out the web sucks, so many servers were not happy with a full header, and started giving us angry looks (slashdot, for instance, would not render correctly because it started sending encoded CSS files!). We then [simplified][4] the header we were sending, which made those servers happy again. Some sites, though, completely ignored our saying we didn&#8217;t support anything except identity, and sent us gzipped content anyway. Most of these were misbehaving caches (this was the case for Wikipedia), so would work after you asked for a forced reload, which would ignore the cache, but some servers, such as the Internet Archive&#8217;s didn&#8217;t really want to talk about encodings &#8211; they only wanted to send gzip-encoded content.

So, in the end, our only way out was implementing the damn encoding support, which finally [happened][5] during the hackfest. Take that, web!

 [1]: http://www.archive.org/
 [2]: http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.3
 [3]: http://trac.webkit.org/changeset/43832
 [4]: http://trac.webkit.org/changeset/44254
 [5]: http://trac.webkit.org/changeset/52208