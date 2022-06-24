---
title: "Add Page Templates Column to Edit Pages Screen"
date: "2012-06-20"
categories: 
  - "wordpress"
---

Recently someone asked this question over at [WPQuestions](http://wpquestions.com/question/showLoggedIn/id/4130). I thought its nice to write about it as well as this is a nice addition some of you might like to have. Its cumbersome everytime to check each page individually just to check which template they are using. There are two ways of achieving this:

### Using Reveal Page Templates Plugin

Installing a plugin is the easiest way to achieve. [Reveal Page Templates](http://wordpress.org/extend/plugins/reveal-page-templates/) plugin will list the file name of the template in a separate column on Edit Pages Screen.

![Reveal Page Templates plugin screenshot](images/reveal-page-templates.jpg#center "Reveal Page Templates")

### Second Method - Custom Code

If you don't want to install a plugin for a minor modification, you can always add the code to your theme's `function.php` file. But I won't recommend that since this code is not theme-specific. I would suggest you add the code to a custom plugin if you use it for your site or create your own plugin. I always prefer creating a simple custom plugin for every WordPress site of mine which includes custom hacks I need. Any theme-specific code should go into your theme's `function.php` and the rest should go to your custom plugin.

Here is the code you should be adding for revealing page templates in your Edit Pages Screen.

```php
add_action('manage_pages_columns', 'add_page_text_column');
add_action('manage_pages_custom_column','show_page_template_column',10,2);

function add_page_text_column($page_columns) {
  $page_columns['page_template'] = "Page template";
  return $page_columns;
}

function show_page_template_column($column_name, $post_id){
if( $column_name == 'page_template' ) {
    $page_template 	= get_post_meta($post_id, '_wp_page_template', true); // file name
    $result = array_search($page_template, get_page_templates()); // get template nice name
    if( !$result )
       echo ''.__('default').'';
    echo $result;
}
}
```

Page Template File Names are saved inside a custom field named `_wp_page_template`. You don't see this custom field while you are editing your page because of the underscore at the starting. Yes, if you don't want your custom field to be visible, just insert an underscore before its name. But this custom field only stores the file name, not the Template Name. For example, you will see front-page.php as the value of this field and not its Template Name which can be something like 'Theme Name's Front Page'. Now **[get\_page\_templates()](http://codex.wordpress.org/Function_Reference/get_page_templates)** function returns an array containing the Template Names as the key name and the file name as its value. We use the array\_search() function to retrieve the key name(File Name) if it matches the file name.

Note: Now if the file name doesn't exist in that array which can happen if the template file is of an old theme that you were using but that is not present in the new theme, you will see the default value. If you use that plugin above, you will still see the file name which you might not find in your current theme. Custom Field's value once set does not change if you change the theme. So if you want to list the page templates if they are present in your current theme, use the above code. If you want to list any previous template it was using even if they are not present in your current theme, use the plugin or use the following modified code.

```php
add_action('manage_pages_columns', 'add_page_text_column');
add_action('manage_pages_custom_column','show_page_template_column',10,2);

function add_page_text_column($page_columns) {
  $page_columns['page_template'] = "Page template";
  return $page_columns;
}

function show_page_template_column($column_name, $post_id){
if( $column_name == 'page_template' ) {
    $page_template 	= get_post_meta($post_id, '_wp_page_template', true); // file name
    if( !$page_template )
       echo ''.__('default').'';
    echo $page_template;
}
}
```

Notice how we have removed the code to search for the nice name entirely and using the `$page_template` variable which holds the file name directly.

Hoping you might find the above tutorial useful in your projects.
