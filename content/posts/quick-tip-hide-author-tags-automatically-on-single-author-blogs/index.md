---
title: "Quick Tip: Hide Author Tags automatically on Single author blogs"
date: "2012-10-04"
categories: 
  - "wordpress"
---

Many themes now come up with elegant author information boxes beneath the single posts and multiple other enhancements so that authors get featured all across your blog. But what if you are using that theme on your personal blog or a blog with only one author. Listing author information then repeatedly doesn't look good and is more of a repetition. 

An about page works better on a single author/personal blog. In such cases, I would suggest you all use [is\_multi\_author()](http://codex.wordpress.org/Function_Reference/is_multi_author) function. It returns true on blogs where more than 1 author has published a post and false on single-author blogs. You can always wrap up the author description tags around this tag.

Usage:

```php
<?php if ( get_the_author_meta( 'description' ) && is_multi_author() ) : ?>
---Display the author info and box--
<?php endif; ?>
```

**Note:** Since this function was released with WordPress 3.2, wrap this function inside a `function_exists()` conditional tag if you are working on an old version of WordPress or if you are working on a theme which you want it to work with old versions.
