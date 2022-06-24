---
title: "Run a Localhost WordPress Installation on Local Area Network"
date: "2019-07-31"
categories: 
  - "useful-tips"
---

Localhost WordPress installs are incredibly useful for doing all the development work before making the site live. Normally most of us have more than one machines at our workplace or homes. Now if you have a localhost environment setup at one, you would want to replicate the same at the other machine. For that you would need to access the same server and install from your primary machine. There's an easy way to do that.

In a LAN setup, each machine is assigned an IP address which falls in the range `192.168.1.x`. On Windows go to command prompt and run 'ipconfig' command to find the Ip address your machine has been assigned. It would show something like this

![ipconfig command](images/2014-07-16_14-08-12.png#center)

Once you have got this ip, you can enter this IP from any other machine and access the Localhost installation at the host PC.

But this is not all if you want to make WordPress work properly on the secondary machine as well. If you have set the WordPress and site url as `http://localhost/site` then you won't be able to see any CSS or post any comment or even login from the secondary machine. To do that, you will need to change the blog and site urls to use the IP address \(`192.168.1.34` in this case\).

You can do that by entering the following lines in your `wp-config.php` file

```php
define( 'WP_SITEURL', 'http://192.168.1.34/wordpress' );
define( 'WP_HOME', 'http://192.168.1.34/wordpress' );
```

You can also make the change in your database but I would recommend just editing your config file. Makes it easier in case your LAN url changes for some reason.

Now you are all set to access WordPress or any site locally hosted across LAN.
