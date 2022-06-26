---
title: "Integrating Coppermine Gallery with your Wordpress"
date: "2009-05-25"
category: 
  - "wordpress"
summary: Learn how to integrate Coppermine Photo Gallery with your WordPress blog.
slug: integrating-coppermine-gallery-wordpress
---

**Updated 25 June, 2022** - This trick may or may not work with current versions of Coppermine and WordPress. I won't be able to help since I don't use any of these softwares. Sorry.

Well, I have setup a new blog which required putting up a lot of photos (to start with 100+ pics in 2 days), so the obvious and the easiest choice was to go with [Coppermine Photo Gallery](https://coppermine-gallery.net/). But then I realized, it does not go down well with Wordpress and there is no official Wordpress plugin or Coppermine bridge available on their respective sites. Though this had been tried a lot of times, but never anybody was successfully able to integrate Coppermine into Wordpress or vice-versa.

Well for this, fortunately there exists a plugin which you won't find on Wordpress Plugins Directory. Find the plugin at [MesSouvenirs.com Blog](https://messouvenirs.net/blog/2008/12/21/coppermine-integration-2/). Installing the Plugin is simple. Extract the zip file and upload the `\coppermine-4.6.2.7\wp-content\plugins\` directory to your wordpress blog. I am using CopperMine Gallery version 1.4.x so upload the two files from `\coppermine-4.6.2.7\cpg\1.4x\` to your gallery root directory. In my case, gallery was installed on the same domain as the blog. Example if the blog's url is `www.blogurl.com` then gallery was installed at `www.blogurl.com/gallery/`

Activate the plugins: Coppermine, Coppermine Widget and Coppermine - Lightbox2 plugins. Well widget and Lightbox plugins are optional. Widget plugin is required if you want to display the images uploaded in gallery in your wordpress sidebar and Lightbox plugin is to show the images on your blog with lightbox effect.

After activating the plugin go to Settings>>Coppermine Integration and insert the Coppermine gallery details manually. For me the automatic search somehow didn't work even after it detected the gallery's configuration file at `/gallery/includes/config.inc.php` - You will also find the details to insert at the page at the above file only.

![Configuring Coppermine Details](images/coppermine_connection.jpg#center "Configuring Coppermine Details")

Now after inserting details and saving, just test the connection. And if its ok, just reload the same settings page and you will see a long page with plethora of options.

[![Coppermine Plugin Page](images/coppermine_integration.jpg#center "Coppermine Plugin Page - Click to View the full image")](images/coppermine_integration.jpg)

If you want to insert the images in your post, just enable the integrate editor option and now you can upload and insert images from your Add New Post Page inside your wordpress.

Hope you will find the plugin useful as much as I found it.

It would have been great if somehow we could show complete gallery in a wordpress page without compromising on any of the features of Coppermine. I know, there have been many wordpress themes ported to Wordpress but that still does look good. It would be wonderful if Coppermine developers could come out with something like this.

Overall this plugin is pretty useful for your basic needs on integrating Wordpress with Coppermine. One more thing, this plugin can also share Wordpress and Coppermine users. I didn’t need this functionality hence I have not mentioned about it here.
