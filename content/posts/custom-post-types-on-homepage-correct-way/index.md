---
title: "Custom Post Types on Homepage - Correct way"
date: "2012-10-06"
categories: 
  - "wordpress"
summary: Learn how to show Custom Post Types on your WordPress site's homepage.
---

There are numerous tutorials on the web about showing Custom Post types on Homepage. Wordpress by default shows only Posts on the homepage. Custom Post types are not automatically shown on the homepage or the feed. Now while searching for the method you must have seen the following code everywhere:

```php
add_filter( 'pre_get_posts', 'my_get_posts' );
function my_get_posts( $query ) {
if ( is_home() && false == $query->query_vars['suppress_filters'] )
$query->set( 'post_type', array( 'post', 'page', 'album', 'movie', 'quote', 'attachment' ) );
return $query;
}
```

This code you can put in either your theme's function.php file or a custom plugin file you created for your site. This code is all good but not many sites will tell you that this might break just any other loop that you might be using on your homepage. For instance, if your homepage is showing a custom menu, then it will stop working. This code infact converted all my custom loop widgets to use this very loop. So practically all widgets started showing the same posts as were shown on the homepage and my custom menu disappeared.

So my suggestion is to not to use this code to show custom post types on your homepage. Stephen Harris in the comments has suggested an even better solution. Here is the code.

```php
add_filter( 'pre_get_posts', 'my_get_posts' );
function my_get_posts( $query ) {
	if ( is_home() && $query->is_main_query())
		$query->set( 'post_type', array( 'post', 'page', 'album', 'movie', 'quote', 'attachment' ) );
	return $query;
}
```

In the above code, we check whether the query we want to modify is the main query or not. All other queries are left untouched in this process. Since [is\_main\_query()](http://codex.wordpress.org/Function_Reference/is_main_query) function was introduced with WordPress 3.3, so the above method won't work with versions below it.

I have removed my original alternative in wake of the above solution as it is good enough and does not require you to modify the queries. You can check the full solution mentioned at [WordPress StackExchange](http://wordpress.stackexchange.com/a/45256/5730).
