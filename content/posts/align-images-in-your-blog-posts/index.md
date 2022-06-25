---
title: "Align Images in your Blog posts"
date: "2007-09-17"
categories: 
  - "wordpress"
summary: Correctly align images in your WordPress posts.
---

If you find aligning images in your blog posts difficult and you can't line up your text alongside smaller logos or images, you should continue reading.

I will start the post with examples of poorly aligned and nicely aligned images in blog posts.

The following screenshot displays a poorly aligned image in a blog post.

![Poorly Aligned Image in Blog post](images/poor_align.jpg#center)

See how the image wastes precious spaces by not allowing the text to align with the image. Now we observe a nicely aligned image in a blog post.

![Nicely Aligned Image in Blog post](images/nice_align.jpg#center)

See the difference it creates? Plus it adds more to the visual appeal of your blog. How to achieve this? Well, it's a simple edit in your theme's stylesheet file.

**NOTE:** Make a backup of your theme's **style.css** file before this in case some problem occurs you can always revert back to the old one.

Open your theme's **style.css** file and add the following line at bottom of your css file.

```css
img.right {
float: right;
margin:10px 0 0 10px;
}
img.left {
float: left;
margin:0 10px 10px 0;
}
```

Now, whenever you want to post a small image that you want to be aligned with the text in a post just add **class="right"** or **class="left"** in `<img>` tag while writing the post.This code should work on almost all themes.
