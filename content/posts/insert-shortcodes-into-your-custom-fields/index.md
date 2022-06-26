---
title: "Parse Shortcodes in your Custom Fields"
date: "2010-01-11"
category: 
  - "wordpress"
summary: Parse shortcodes in your Custom Fields inside WordPress posts.
slug: insert-shortcodes-into-your-custom-fields
---

[Shortcodes](https://codex.wordpress.org/Shortcode_API) and [Custom Fields](https://wordpress.org/support/article/custom-fields/) are two very powerful features of WordPress which allow you to insert any custom content anywhere on your site. But in certain cases, you may need to insert a shortcode into your Custom field. This can happen if a plugin you use only offers shortcodes but you need to insert that shortcode only on a certain portion of the theme (e.g. `single.php`).

Now custom fields can be placed at any place of the theme but Shortcodes only appear in post content. And if you include shortcode directly as a custom field value, it will be displayed as it is without getting parsed. 

For example, if you insert `[youtube]VIDEOURL[/youtube]` in your custom field to display 's video (In the Viper Video Tags Plugin), it will show up as `[youtube]VIDEOURL[/youtube]` instead of the video which is because it does not parse shortcodes. To get around the problem use the following code in your theme's template file.

```php
<?php
if ( get_post_meta($post->ID, 'cfield', true) )
echo do_shortcode(get_post_meta($post->ID, 'cfield', $single = true));
?>
```

In the above code, the custom field **cfield's** value is directly passed to the [`do_shortcode()` function](https://developer.wordpress.org/reference/functions/do_shortcode/) which parses the shortcode in the custom field's value and is echoed to the browser.
