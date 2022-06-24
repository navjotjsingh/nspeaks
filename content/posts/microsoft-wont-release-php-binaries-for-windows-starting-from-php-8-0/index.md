---
title: "Microsoft won't release PHP binaries for Windows starting from PHP 8.0"
date: "2020-12-01"
categories: 
  - "technology"
tags: 
  - "php"
---

With [PHP 8.0 being released](https://www.php.net/archive/2020.php#2020-11-26-3) recently, Microsoft made an [important announcement](https://news-web.php.net/php.internals/110907) a few months back. Going forward, Microsoft won't release PHP 8.0 binaries anymore. PHP 7.4 will be the last supported PHP version that Microsoft will work on. So what it does really mean? The Windows binary packages available from https://windows.php.net were being built by Microsoft all these years. And yes, surprisingly, it was news for me as well. So going forward, Microsoft will stop working on them from PHP 8.0 onwards.

So what does it mean for the developers? Well, nothing will change. Why? Because just because Microsoft won't build them doesn't mean nobody else will. And as you can see [PHP 8.0 binaries for Windows](https://windows.php.net/download/) are already up and they run just fine.

So why did Microsoft do it? Well nobody knows for sure but some people think that given now Windows Subsystem for Linux (WSL) works nicely which allows you to run and install any PHP version of your choice, there is a little incentive for Microsoft to continue working on a native binary anymore.

To be honest, you shouldn't be using Windows PHP binaries anymore. There are better ways to run and install PHP on Windows. First one is using Docker. Docker allows you to run any server of your choice along with PHP with just a few commands and allows you to transfer it on a production environment with ease. Or you can use WSL to set up your development environment. I will soon publish tutorials related to Docker regarding how to set up a local development environment and how to transfer your site from development to production environments.

Anyway, be safe and happy holidays.
