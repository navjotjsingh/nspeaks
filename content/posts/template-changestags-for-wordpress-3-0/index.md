---
title: "Template Changes/Tags for Wordpress 3.0"
date: "2010-06-02"
category: 
  - "wordpress"
summary: Learn about all the Template changes coming with WordPress 3.0 release.
slug: template-changestags-for-wordpress-3-0
---

By now everybody must be knowing that WordPress 3.0 will soon be released with some heavy-duty features which will enable WordPress to directly compete with other CMS systems like Joomla and Drupal. And as happens with any other new version of WordPress, there are bound to be some new features/functions available for WordPress which Theme/Plugin authors need to keep themselves updated with. Let me tell you about all the new template changes required for WordPress 3.0 which the theme authors should keep themselves updated with.

## Custom Menus

This new feature enables you to have custom menus in Wordpress which can have pages/categories/external links or even links to posts in a single menu. The following function prints the custom menu in Wordpress 3.0:

```php
wp_nav_menu();
```

wp\_nav\_menu() function has the following parameters:

- id – The menu id. Defaults to blank.
- slug – The menu slug. Defaults to blank.
- menu\_class – CSS class to use for the div container of the menu list. Defaults to 'menu'.
- format – Whether to format the ul. Defaults to 'div'.
- fallback\_cb – If the menu doesn’t exists, a callback function will fire. Defaults to 'wp\_page\_menu'.
- before\_link – Output text before the link.
- after\_link – Output text after the link.
- before\_title – Output text before the link text.
- before\_title – Output text after the link text.
- echo – Whether to echo the menu or return it. Defaults to echo.

But before using the above command to print the custom menu, you need to add support of Custom Menus to your theme by adding the following code in your theme's function.php file:

```php
add_theme_support( 'nav-menus' );
```

This function has been deprecated in favor of `register_nav_menu(s)()`.

For Details on how to create Navigation Menus, I suggest [reading tutorial by Justin Tadlock](https://justintadlock.com/archives/2010/06/01/goodbye-headaches-hello-menus).

## Custom Post Types plus Custom Taxonomy Improvements

Earlier we could only create Posts or Pages. But now with Custom Post Types, we can create lot more types of posts like Gallery Items, Movie Information Posts, etc. which will be independent in style from a normal post. Custom taxonomies which got introduced in Wordpress 2.8 got a small upgrade which now allows them to be hierarchial too just like a normal Category.

It's difficult to explain all this in a small post plus there are already awesome tutorials on this. Just Linking to them:

- Custom posts explained by Kovshenin - [Part 1](https://kovshenin.com/archives/custom-post-types-in-wordpress-3-0/) and [Part 2](https://kovshenin.com/archives/extending-custom-post-types-in-wordpress-3-0/)
- [Guide to Custom Post Types by Justin Tadlock](https://justintadlock.com/archives/2010/04/29/custom-post-types-in-wordpress)
- [Show Custom post Types](https://justintadlock.com/archives/2010/02/02/showing-custom-post-types-on-your-home-blog-page) on your Homepage by Justin Tadlock

## Custom Background

Wordpress 3.0 comes with inbuilt support of changing background of your Wordpress themes. To Add support for Custom background, drop the following line in your theme's function.php file:

```php
add_custom_background();
```

This tag adds a custom body style tag inline in between your `<head></head>` tags.

## Standard Comments Tag

Wordpress 3.0 introduces a new standard comment form tag that enables any theme to have a full-blown comment form by just modifying/specifying a few parameters.

Here is the standard comment form function in action.

```php
<?php comment_form(); ?>
```

`comment_form()` can be changed for any theme using filters and custom styles. For More Details on this, read [this post by OttoPress](https://ottopress.com/2010/wordpress-3-0-theme-tip-the-comment-form/).

## Automatic Feed Links

Earlier theme authors had to add the feed links manually in the header.php. But now with Wordpress 3.0, this can be done automatically. Just add this line in your theme's function.php file:

```php
add_theme_support('automatic-feed-links');
```

## Custom Header Improvements

Earlier there was a single option where users could upload a custom header for a theme. But with WordPress 3.0, themes can give users a choice to select some of the pre-made headers too apart from uploaded headers. To enable this feature, just use the following example code(taken from **twentythen** theme):

```php
register_default_headers( array (
		'path' => array (
			'url' => '%s/images/headers/path.jpg',
			'thumbnail_url' => '%s/images/headers/path-thumbnail.jpg',
			'description' => __( 'Path', 'twentyten' )
		),
		'sunset' => array (
			'url' => '%s/images/headers/sunset.jpg',
			'thumbnail_url' => '%s/images/headers/sunset-thumbnail.jpg',
			'description' => __( 'Sunset', 'twentyten' )
		)
	) );
```

Make sure to replace twentyten with the theme's folder name while using the above code. You can add as many pre-made headers to your themes for your users.

## ShortLinks

For Displaying the Shortlink of a post, you need to use the following code.

```php
the_shortlink( $text, $title, $before, $after );
```

`$text` is link text or html to be displayed which defaults to **'This is the short link.'** `$title` is the tooltip for the link. `$before` and `$after` are html which can be shown before and after the shortlink respectively. All parameters are optional.

One can filter `the_shortlink()` function to display custom created shortlinks using `pre_get_shortlink()` and `get_shortlink()` hooks. More Information by [WPEngineer](https://wpengineer.com/shortlinks-with-wordpress-3-0/) and [OttoPress](https://ottopress.com/2010/shortlink-trick/).

## Login Form

Wordpress 3.0 introduces a new login form template tag which you can place anywhere in your template to get readymade Login form for your visitors to signup/register to your blog. The following template tag outputs the Login form:

```php
wp_login_form( $args ); 
```

Following are the arguments of the tag:

- echo - Display the results. Defaults to true.
- redirect - Url to redirect to. Defaults to current page.
- form\_id - Form id. Defaults to loginform.
- label\_username - Label to give to Username. Defaults to 'Username'.
- label\_password - Label to give to Password. Defaults to 'Password'.
- label\_remember - Label to give to Remember me field. Defaults to 'Remember Me'.
- label\_log\_in - Label to give to Login Button. Defaults to 'Log In'.
- id\_username - Id for the Username field. Defaults to 'user\_login'.
- id\_password - Id for the Password field. Defaults to 'user\_pass'.
- id\_remember - Id for the remember me field. Defaults to 'rememberme'.
- id\_submit - Id for the Submit button. Defaults to 'wp-submit'.
- remember - Whether to remember the values. Defaults to true.
- value\_remember - Whether to tick the Remember me field or not. Defaults to 'false'.
- value\_username - Value of Username field. Defaults to last used value.

## New Url Functions to Refer to Blog, Homepage and Admin

Earlier we used bloginfo('url') or get\_option('site\_url') to get blog or site's urls. But wordpress 3.0 comes with new functions to get urls easily. They are listed below:

```php
home_url(); // retrieve the home url of the current site
get_home_url(); // return the home url of the current site
site_url(); // retrieve the url of current site
get_site_url(); // return the url of current site
get_admin_url(); // retrieve the url to the admin area for a GIVEN site, defaults to current admin_url() was introduced earlier.
```

## Improved Author Template Support

Now we can create a simple file like author-name.php or author-id.php and it will show a customized Author template for that author.

## Modular Template Support

Most of The template files contain repeated code like the loop code over and over again. But Wordpres 3.0 introduces a function get\_template\_part() with which you can include some code from a file wherever you want just with a single line. In its simplest form, one needs to use it like this:

```php
get_template_part( 'navi');. //Looks for a file named navi.php
get_template_part( 'navi', 'sidebar' );. //Looks for a file named navi-sidebar.php
```

For More Information read article on Modular Templates by Daryl Koopersmith ( [Part 1](https://drylk.com/2010/04/06/modular-themes-why/) and [Part 2](https://drylk.com/2010/04/06/modular-themes-organization/) ) and by [WPEngineer](https://wpengineer.com/use-more-flexibility-in-wordpress-templates/).

## Theme Organisation Update

Earlier if your theme just had an index.php and a style.css file, your blog still ran perfectly without any errors as it considered the default theme as its parent theme. But with WordPress 3.0, the default theme has been removed. If you still want to run your theme perfectly on Wordpress 3.0, just keep the default theme (It won't come with WordPress 3.0 package) and include the following line at the top of your style.css.

```css
Template: default
```

In future versions, compatibility support like this will be removed. So you need to make sure either the parent theme contains all the relevant files or the theme itself.

More Information from [Westi](https://westi.wordpress.com/2010/05/03/themes-and-wordpress-3-0-some-important-changes/).

**Note:** To maintain backward compatibility, use `function_exists()` to check whether the theme is being used on WordPress 3.0 or not otherwise your theme will break on WordPress 2.9 and below versions on using the codes above.
