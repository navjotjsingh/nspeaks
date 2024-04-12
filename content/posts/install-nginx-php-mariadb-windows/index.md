---
title: "Install Nginx, PHP 7.2, MariaDB (LEMP) on Windows"
date: "2018-08-19"
category:
  - "tutorials"
tag:
  - "lemp"
  - windows
summary: How to Install Nginx, PHP, MariaDB (LEMP) on a Windows PC.
slug: install-nginx-php-mariadb-windows
---

**UPDATE: 23 February, 2020** - Fixed a Lot of missing instructions and errors including commands to restart services.

**UPDATE: 24 June, 2022** - Fixed formatting errors in the code and updated the version and other information.

**Update: 12 April, 2024** - Fixed Nginx and PHP configuration errors.

There is an abundance of tutorials in case you want to install nginx, PHP and MariaDB on Linux but when it comes to installing these tools on Windows, tutorials are either outdated or scattered. Anyway, let's see how we can setup Nginx, PHP and MariaDB on Windows.

## Download Nginx

This is fairly easy. Just head to [Nginx's Download page](http://nginx.org/en/download.html) and download the zip file which says **nginx/Windows-1.15.2** which is the current version at the time of writing this tutorial. Once you have downloaded it, extract the zip file to `C:\nginx` folder or anywhere you would like. But keep it in the root directory for simplicity sake.

## Download PHP

Head to [PHP's download site for Windows](https://windows.php.net/download) and download the NTS version of PHP 7.2.9 (the latest version available at the time of this tutorial). Non Thread safe version of PHP is usually recommended if you are going to use with Nginx.

Extract all the files inside the zip to `C:\nginx\php` directory.

## Download MariaDB

Visit the [MariaDB download page](https://downloads.mariadb.org/) and get the zip file for the latest MariaDB version (10.3.9 at the time of writing this tutorial). And yes, I know there is an installer but we are going to stick to the zip file for this one.

Extract all the files inside the zip to `C:\nginx\mariadb` directory.

## Setup Nginx

The configuration file for Nginx can be found in `C:\nginx\conf` directory named `nginx.conf`. There is only one executable file namely `nginx.exe` in the main directory. Unlike Apache, Nginx doesn't install itself as a Windows Service. Bummer, right?

But fret not. We can convert Nginx into a Windows Service. But before we do that, let's take a quick look at the command line shortcuts for Nginx's executable file.

```bash
nginx -s stop           fast shutdown
nginx -s quit           graceful shutdown
nginx -s reload         config change, graceful shutdown of old process and start new process with changed config
nginx -s reopen         re-opening log files
```

Out of these, we would be using `nginx -s stop` and `nginx -s reload` a couple of times in this tutorial.

### Download WinSW

WinSW is a Windows Service wrapper that allows us to use any executable as a Windows Service. Grab the binary file from the [latest Release page](https://github.com/kohsuke/winsw/releases/latest). There are multiple binary files for .NET 2+ and .NET 4+ versions. Assuming that you are using Windows 8 and above, grab the .NET461 executable.

Copy the executable into the `C:\nginx` directory and rename it as _nginxservice.exe_.

Create a file named _nginxservice.xml_ and paste the following code into it.

{{< gist navjotjsingh 13b8ee9cbb33e42829a8c5d669b4f8c7 >}}

Now, launch Powershell or Command center in Administrator mode and run the following command

`nginxservice.exe install`

**PS:** When running from Powershell, if you want to run an executable that is in the current directory, use `./` in front of the file name or it won't run. This is not required if you are running an executable from the current directory in Command Center.

To start the service run the command: `net start nginx` and to stop the service use `net stop nginx`. You can also see Nginx listed as a service if you launch the Services app from the Start Menu.

Just run `http://localhost` in your browser to see if Nginx is running successfully. You should get the following page which tells us that the installation was successful.

![Nginx Default Home page](images/chrome_2018-08-20_01-52-16-e1534712504497.png#center)

Your HTML and other files will go by default in the `C:/nginx/html` directory. It is time to set up PHP now.

## Set up PHP

PHP under Nginx runs as a CGI application. Like Nginx, PHP also doesn't run as a Windows service. Therefore, we follow similar steps as we did for Nginx. Copy the WinSW executable to _C:\\nginx\\php_ directory and rename it as _phpservice.exe_.

First, rename _php.ini-production_ to _php.ini_. Also, create an empty directory **php** under _C:/nginx/logs_. Then create a file named _php-stop.cmd_ in php directory and add the following line to it `taskkill /f /IM php-cgi.exe` Now create an empty file named _phpservice.xml_ and copy the following lines to it.

{{< gist navjotjsingh 642113c000b5ed118dab4be031a28ce7 >}}

I have used the port number 9999 above. You can use any port number you like to start PHP under.

Next, launch Powershell or Command centre in Administrator mode and run the following command

```powershell
phpservice.exe install
```

Now you can start the PHP service with the command `net start php`.

Now that we have set up PHP, let's do a bit of a configuration so that PHP and Nginx can work together.

First make a backup of _C:\\nginx\\conf\\nginx.conf_ file and then open it and uncomment the following lines. Also change the fastcgi\_param as indicated below.

```nginx
location ~ .php$
  {
    root html;
    fastcgi_pass 127.0.0.1:9999;
    fastcgi_index index.php;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    include fastcgi_params;
  }
```

Also, change the following line

```nginx
location /
  {
    root   html;
    index  index.html index.htm;
  }
```

to

```nginx
location /
  {
    root html;
    index index.php index.html index.htm;
  }
```

If you are using .htaccess files, then uncomment the following lines as well.

```nginx
location ~ /.ht
  {
    deny all;
  }
```

This will ensure that PHP code is loaded properly inside Nginx. root html means that you need to keep your content in _C:\\nginx\\html_ directory. You can change it accordingly. If you want to use a directory that's outside the Nginx folder then you need to specify the full pathname.

Now we also need to make some adjustments to the php.ini file that we had saved earlier. Open _C:\\nginx\\php\\php.ini_ and scroll down to the Paths and Directories section and uncomment the following setting to set the directory for PHP extensions.

```ini
extension_dir = "ext"
```

Next, scroll to the Dynamic Extensions section and uncomment the following lines.

```ini
extension=bz2
extension=curl
extension=gd2
extension=gettext
extension=imap
extension=mbstring
extension=exif
extension=mysqli
extension=openssl
extension=sqlite3
```

You can uncomment more extensions depending upon your needs. One last thing is to increase the memory limits. You can do so by adjusting the following variables

```ini
post_max_size = 8M
...
upload_max_filesize = 2M
...
memory_limit = 128M`
```

Adjust the values accordingly according to the system/server you are using and what works for you. The value of the `upload_max_filesize` variable should be equal or less than the `post_max_size`. The `upload_max_filesize` controls the limit for the files uploaded but for all other forms of POST data, the size is controlled by the `post_max_size` variable. But if you set the POST limit less than the file size limit, then the size of the file will depend on POST data variable.

Now that both Nginx and PHP are set up, run the following commands on Commandline.

```powershell
net stop nginx
net start nginx
net stop php
net start php
```

If you are using Powershell, you can use a single command to restart.

```powershell
Restart-Service -Name nginx Restart-Service -Name PHP
```

## Setup MariaDB

This is the final phase of our tutorial. Run the following command on Command Prompt/Powershell after entering the directory _C:\\nginx\\mariadb\\bin_

```powershell
mysql_install_db.exe --datadir=C:\nginx\mariadb\data --service=MariaDB --password=secret
```

This command sets the data directory for databases as well as sets up the MariaDB service and the root password. And that's it. MariaDB is working now.

And that's all folks. Now you have an Nginx/PHP/MariaDB based server setup on your Localhost.

To start or stop any service use the commands

```powershell
net start/stop nginx
net start/stop mariadb
net start/stop php
```

You can also do that by launching Services app by searching for it in the Start menu.

And that is the end of the tutorial. If you have any questions, contact me.

Source: [Stackexchange](https://stackoverflow.com/a/13875396/772709) and [Nginx forum](https://forum.nginx.org/read.php?2,236376) for pointers on setting up Nginx and PHP as windows services.
