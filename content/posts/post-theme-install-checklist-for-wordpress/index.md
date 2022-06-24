---
title: "Post Theme Install Checklist for Wordpress"
date: "2009-06-22"
categories: 
  - "wordpress"
---

**Note:** This article is obsolete now. Refer to the [updated article on the same](http://nspeaks.com/post-theme-install-checklist-things-to-do-after-you-install-a-new-theme/ "Post Theme Install Checklist – Things to do after you install a new theme").

If you got your blog a new design recently, then this Checklist is for you. Just making sure its working properly won't complete your task of installing a new theme on your blog. You should take care of few points which will help improve the performance of your theme which I am listing below:

1. **FeedBurner Links Across the Theme:** 99.9% Chance is that you are using Feedburner for your site. Though many themes have an option page nowadays asking for your feedburner id but even sometimes their implementation is incomplete. Incomplete Implementation refers to that these themes update feed link on the frontend of the blog but don’t touch the autodiscovery feed links. Even if a theme have such an option or not, you should manually check the feed link across the blog. For making your feedburner link uniform across your site, check my post on [proper implementation of Feedburner url](http://nspeaks.com/24/switch-your-feeds-to-feedburner-guide/).
2. **Analytics and Other Service Codes:** If you are using services like Analytics, Infolinks, Kontera or any other service which requires you to add a code into your site’s header/footer, you need to place these codes manually in either header.php or footer.php wherever required. There are few themes who ask for analytics id which will update only analytics code but there are themes which ask for full analytics code. Here in such case along with your analytics code, you can also embed all such codes in same option box.
3. **Test All Pages of Site:** Make sure that all pages on your site are working. Start by visiting all pages like Index, Single Post, Pages, Category archive, date/month archives, Search, 404, Sitemap, Contact Page or any other explicit page which you are using.
4. **Website Speed and Optimisation Test:** You should also test your website for speed and loading times. Some themes include a lot of javascript and css which can add to the loading time. I would recommend two of the following services:
    
    - [**PingDom**](http://tools.pingdom.com/fpt/)
    - **[Web Page Analyzer](http://www.websiteoptimization.com/services/analyze/) from WebsiteOptimisation.com**
    
    Though the second service provides all features plus even more than the first, still I would recommend using Pingdom too as it allows you to save reports of your website and you can know about the progress you are making in your site better. Web Page Analyzer gives a lot of recommendations and details about your website’s loading time and size details. If your theme starts becoming heavier, I would also recommend using [Script Compressor Plugin](http://wordpress.org/extend/plugins/script-compressor/) which compresses the CSS and Javascripts loaded with your blog.
5. **Plugin Compatibility:** And last but not the least, don’t forget to add the respective plugin codes into your theme. Some themes require certain plugins to be pre-installed for working. Check for such plugins. If you don’t require such plugins, remove their codes from your theme files. Also check for plugins that need a bit of modification of css of your theme.
