---
title: "Difference between is_home() and is_front_page()"
date: "2010-10-11"
category: 
  - "wordpress"
summary: Learn the difference between is_home() and is_front_page() WordPress template tags.
slug: difference-between-is_home-and-is_front_page
---

Most of the Wordpress Conditional Tags are easy to understand and pretty self-explanatory with their terminology but these two tags - is\_home() and is\_front\_page() were two which left me confused. So I searched for the difference between two and found that both are actually very different in their approaches. Though both of them give same result in few cases but it doesn't mean both tags mean the same thing.

### is\_home()

Though the tag says home but it actually looks for the page where all your recent posts are shown. If you are on the homepage and you are showing recent posts there, is\_home() will return true. And if your front page is set static, is\_home() will return false for it but will return true for any inner page where your recent posts are being shown.

### is\_front\_page()

This tag actually checks if you are on the front page or not. If you are on the frontpage of the blog whether it shows recent posts or is a static page, it will return true. Any inner page even if it shows recent posts will give false for is\_front\_page().
