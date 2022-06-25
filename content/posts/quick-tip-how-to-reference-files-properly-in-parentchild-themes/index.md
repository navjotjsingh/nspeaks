---
title: "How to reference files properly in parent/child themes?"
date: "2013-06-13"
categories: 
  - "wordpress"
summary: Learn how to refer to files properly in parent and child WordPress themes.
---

Child theme is arguably the best thing that happened to WordPress themes. But still I see many people who create a child theme incorrectly use wordpress functions to refer to the images and stylesheets which result in broken urls. WordPress provides four functions to refer to the different files inside parent and child themes: [get\_template\_directory()](http://codex.wordpress.org/Function_Reference/get_template_directory), [get\_template\_directory\_uri()](http://codex.wordpress.org/Function_Reference/get_template_directory_uri), [get\_stylesheet\_directory()](http://codex.wordpress.org/Function_Reference/get_stylesheet_directory) and [get\_stylesheet\_directory\_uri()](http://codex.wordpress.org/Function_Reference/get_stylesheet_directory_uri).

### get\_template\_directory()/get\_template\_directory\_uri()

Now these tag are self explanatory. They are used to get the template directory's path and url. These tags will always refer to parent theme's path even when used from inside the child theme.

### get\_stylesheet\_directory()/get\_stylesheet\_directory\_uri()

Again self explanatory. Right? But they are a bit different as they return always the current theme's path. It means they will return the parent theme or child theme's paths depending on from where they have been called.

Let me present you all this in a tabular form for easy understanding.

| When Inside         |   Want to refer to?   |            Use               |
| --------------------| ----------------------| -----------------------------|
| Parent theme        | Current Theme         | get\_template\_directory()   |
| Parent theme        |   Child theme         | get\_stylesheet\_directory() |
| Child theme         |  Parent theme         | get\_template\_directory()   |
| Child theme         |   Child theme         | get\_stylesheet\_directory() |
| Theme with no child |     Itself            | get\_template\_directory()   |
