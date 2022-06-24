---
title: "Category descriptions in Wordpress"
date: "2009-01-20"
categories: 
  - "wordpress"
---

One thing which I have seen many bloggers missing completely(including me until now) is the category description field. Most bloggers I have seen create relevant categories but forget to fill the description field either due to ignorance or deliberately to save time. But this field can also be important both with respect to SEO and also user experience enhancing effect.

In terms of user experience enhancing effect, many times users are not able to understand what type of posts are included in the category you have created. So it's better to give a proper description of each category. And in terms of SEO effect, most SEO plugins of SEO including All in One SEO and Platinum SEO Pack pick category descriptions as meta description tags for the category archives.

Now the question arises of how to show category descriptions on the category archive page. The following code when used in your theme file pulls the category description and shows it to the user.

```

<?php 
         echo category_description();  
?>
```

Now, this code should go into the archive.php of your theme file which is responsible for generating the category list of posts. Check with your theme for the proper location where you should insert this code.

And for SEO effect, if you are using plugins like All in One SEO or Platinum SEO plugins, they will pick up the category description automatically and if that's not the case, you can use the following code in your header.php

```

<?php 
/* If this is a category archive */ 
if (is_category()) { ?>
<meta name="description" content="<?php echo category_description();  ?>" />
<?php } ?>
```

Remember this code should appear before </head> in header.php

So now I hope you will use the above method to make your site, even more, user-friendly and SEO optimized by giving useful descriptions to each of your categories. I am yet to do it on this blog though since this thought came when I was working on one of my blogs.
