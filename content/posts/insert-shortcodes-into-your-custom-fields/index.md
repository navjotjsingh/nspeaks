---
title: "Parse Shortcodes in your Custom Fields"
date: "2010-01-11"
categories: 
  - "useful-tips"
  - "wordpress"
---

[Shortcodes](http://codex.wordpress.org/Shortcode_API) and [Custom Fields](http://codex.wordpress.org/Using_Custom_Fields) are two very powerful features of Wordpress which allow you to insert any custom content anywhere on your site. But in certain cases, you may need to insert a shortcode into your Custom field. This can happen if a plugin you use only offers shortcodes but you need to insert that shortcode only on a certain portion of the theme (e.g. single.php). Now custom fields can be placed at any place of the theme but Shortcodes only appear in post content. And if you include shortcode directly as a custom field value, it will be displayed as it is without getting parsed. For example, if you insert \[youtube\]VIDEOURL\[/youtube\] in your custom field to display 's video (In the Viper Video Tags Plugin), it will show up as \[youtube\]VIDEOURL\[/youtube\] instead of the video which is because it does not parse shortcodes. But then Wordpress is too great so don't worry, since a hack is there which will allow you to parse Shortcodes into your custom fields:

```

<?php
if ( get_post_meta($post->ID, 'cfield', true) )
echo do_shortcode(get_post_meta($post->ID, 'cfield', $single = true));
?>
```

In the above code, the custom field **cfield's** value is directly passed to the [do\_shortcode() function](http://codex.wordpress.org/Function_Reference/do_shortcode) which parses the shortcode in the custom field's value and is echoed to the browser.
