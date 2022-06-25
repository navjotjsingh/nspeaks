---
title: "[Fix] Trashing a WordPress post redirects to the Site homepage"
date: "2021-01-25"
categories: 
  - "wordpress"
summary: Fix WordPress's post trashing redirection bug.
---

For a very long time, I have been facing this issue where whenever I trash any post from the WordPress dashboard, instead of redirecting me to the same post page, I always end up getting redirected to the site's homepage. It is not a bug which breaks anything or causes any world-ending issue but it was annoying as hell. For months, I was ignoring it since I only delete a handful of posts or old drafts.

But today I decided to investigate and seems I found the reason and a permanent fix for it. This happens because the server blocks the exact referrer information which is sent.

To give you a short course, when you open any page of any site, the server sends referrer information which contains the URL of the page where the request you made. For example, you are browsing `https://example.com` and you decide to click on its Contact link and open `https://example.com/contact` then the server will send referrer information saying that the request was made from `https://example.com`. This is useful for site analytics and determining where the visitor to your site is coming from. But there are several dangers like you can inadvertently leak a sensitive page of your site.

To prevent all this, you can control what referrer information is sent with each request using the [Referrer-Policy HTTP header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Referrer-Policy). Coming back to the Trash post issue, this happens when you have set the Referrer-policy as **'origin'** or **'strict-origin'**. In both these cases, the server only sends the root domain as the referrer and not the entire path which is what happens when you click the Trash button. WordPress tries to return you back to the original referred page but since in both cases, the referer was set to the root domain and not the full path, you return back to the homepage and not the actual page from where you trashed the posts.

To fix the problem, you need to change the Referrer-policy in your server settings to use **'same-origin'** or **'strict-origin-when-cross-origin'**. You may need to consult your server software documentation on how to do it.
