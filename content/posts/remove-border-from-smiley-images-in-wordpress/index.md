---
title: "Remove Border from smiley images in Wordpress"
date: "2007-09-05"
categories: 
  - "useful-tips"
  - "wordpress"
---

The idea for this post came because my blog's new theme had borders on all smileys which looked a bit odd. So I thought why not make a post about it.

Open your theme's style.css file and add this in last line:

```
img.wp-smiley{ border:none;}
```

That's all. Nothing more. It will remove borders on all smileys and works with all themes in WordPress and for most versions of WordPress.
