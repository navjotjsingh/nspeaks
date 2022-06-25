---
title: "Add Custom Taxonomy Columns to Edit Posts Page"
date: "2010-04-05"
categories: 
  - "wordpress"
summary: Add Custom Columns to Edit WordPress Posts page showing custom taxonomies.
---

Wordpress's Edit Posts Page pretty much shows the information you need to use. But with the inclusion of Custom Taxonomies from WordPress 2.8, it becomes difficult to check which posts are using which Taxonomies unless you edit each post manually. To make the job of editing custom taxonomies easy, it's better if we could see all our custom taxonomies on the Edit Posts page too. Well, there are several tutorials on the web that will tell you how to add custom columns to the Edit Posts Admin page but none tells you an exact method for adding the Custom Taxonomies column. Let me take a walkthrough of it.

Big Help to [This post at Themehybrid](https://themehybrid.com/support/topic/add-custom-taxonomy-to-editphp) for the idea and code. The following tutorial adds 2 columns to add Films and Language (with the custom taxonomy slugs as film and lang) taxonomies to Edit Posts Admin Page. (Works on WordPress 2.9 and 3.0)

Open your Theme's functions.php file and add the following code just before the last `?>`.

```php
add_filter( 'manage_posts_columns', 'govid_columns' ); //Filter out Post Columns with 2 custom columns

function govid_columns($defaults) {
    $defaults['language'] = __('Language'); //Language and Films is name of column
    $defaults['film'] = __('Films');
    return $defaults;
}

add_action('manage_posts_custom_column', 'govid_custom_column', 10, 2); //Just need a single function to add multiple columns

function govid_custom_column($column_name, $post_id) {
    global $wpdb;
    if( $column_name == 'language' ) { 
			$tags = get_the_terms($post->ID, 'lang'); //lang is the first custom taxonomy slug
			if ( !empty( $tags ) ) {
				$out = array();
				foreach ( $tags as $c )
					$out[] = "<a href='edit.php?lang=$c->slug'> " . esc_html(sanitize_term_field('name', $c->name, $c->term_id, 'lang', 'display')) . "</a>";
				echo join( ', ', $out );
			} else {
				_e('No Languages.');  //No Taxonomy term defined
			}
		} else if( $column_name == 'film' ) {   //Adding 2nd column
			$tags = get_the_terms($post->ID, 'film'); //film is the next taxonomy slug for column Films
			if ( !empty( $tags ) ) {
				$out = array();
				foreach ( $tags as $c )
					$out[] = "<a href='edit.php?film=$c->slug'> " . esc_html(sanitize_term_field('name', $c->name, $c->term_id, 'film', 'display')) . "</a>";
				echo join( ', ', $out );
			} else {
				_e('No Related Films.'); //no Films found
			}
		} else {
            echo '<i>'.__('None').'</i>'; //Only 2 columns, blank now.
        }
    }
?>
```

To learn about Custom Taxonomy, I would suggest reading this [post by Justin Tadlock](https://justintadlock.com/archives/2009/05/06/custom-taxonomies-in-wordpress-28) covering basics of Custom Taxonomies in WordPress 2.8.
