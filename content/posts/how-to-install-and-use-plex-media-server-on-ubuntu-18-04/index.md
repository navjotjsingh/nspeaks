---
title: "How to Install and Use Plex Media Server on Ubuntu 18.04"
date: "2019-08-26"
category: 
  - "tutorials"
tag: 
  - "linux"
  - "plex-media-server"
  - "ubuntu"
summary: Install Plex Media Server on Ubuntu for all your movie and music streaming needs.
slug: how-to-install-and-use-plex-media-server-on-ubuntu-18-04
---

## Introduction

Plex is a media server that allows you to stream your digital content from anywhere in the world. You can watch movies, tv shows and listen to music from anywhere in the world. This guide will show you how to install Plex on to your Ubuntu 18.04 based server as well as how to connect to from Plex's client application.

## Prerequisites

1. You would need a Ubuntu 18.04 based Server. You can create one at [Digitalocean](https://www.digitalocean.com/?refcode=574d284bdcd2), [Vultr](http://www.vultr.com/?ref=6816937) or [Linode](https://www.linode.com/?r=1c24fcff0ee546aa68e30ca169714f4ffd8555fe).
2. Ensure that your system is up to date.

    ```bash
    sudo apt update && sudo apt upgrade
    ```

3. If you will be transcoding media on the server, then you should opt for bigger servers with two or more cores and at least four or more gigabytes of RAM. If you won't be transcoding, then you should be fine with anything above 1GB of RAM and with 1 CPU core or more.
4. You would need a Plex account for this guide. [Create one](https://www.plex.tv/sign-up/?forward=web) if you don't have.

## Installation

1. Start by importing the Plex repository's GPG keys.

    ```bash
    curl https://downloads.plex.tv/plex-keys/PlexSign.key | sudo apt-key add -
    ```

2. Add the Plex repository to your server's repositories list.

    ```bash
    echo deb https://downloads.plex.tv/repo/deb public main | sudo tee /etc/apt/sources.list.d/plexmediaserver.list
    ```

3. Update Your Server's Repository List.

    ```bash
    sudo apt update
    ```

4. Install Plex.

    ```bash
    sudo apt install plexmediaserver
    ```

5. Verify that Plex is running.

    ```bash
    sudo systemctl status plexmediaserver
    ```

    The output should look something like

    ```bash
    plexmediaserver.service - Plex Media Server
    Loaded: loaded (/lib/systemd/system/plexmediaserver.service; enabled; vendor preset: enabled)
    Active: active (running) since Tue 2019-06-18 11:37:14 UTC; 17min ago
    Process: 13775 ExecStartPre=/bin/sh -c /usr/bin/test -d      
    "${PLEX_MEDIA_SERVER_APPLICATION_SUPPORT_DIR}" || /bin/mkdir -p   
    "${PLEX_MEDIA_SERVER_APPLICATION_SUPPORT_DIR}" (code=exited, status=0/SUCCESS)
    Main PID: 13782 (sh)
    Tasks: 129 (limit: 1152)
    CGroup: /system.slice/plexmediaserver.service
    ```

## Setup the Firewall

Plex Media Server requires certain ports to be open for it to work. We will be using UFW to manage firewall in this guide.

1. Create a new file in `/etc/ufw/applications.d/` directory named `plexmediaserver` with the following contents

    ```bash
    [plexmediaserver]
    title=Plex Media Server (Standard)
    description=The Plex Media Server
    ports=32400/tcp|3005/tcp|5353/udp|8324/tcp|32410:32414/udp
    
    [plexmediaserver-dlna]
    title=Plex Media Server (DLNA)
    description=The Plex Media Server (additional DLNA capability only)
    ports=1900/udp|32469/tcp
    
    [plexmediaserver-all]
    title=Plex Media Server (Standard + DLNA)
    description=The Plex Media Server (with additional DLNA capability)
    ports=32400/tcp|3005/tcp|5353/udp|8324/tcp|32410:32414/udp|1900/udp|32469/tcp
    ```

2. Save the file and then update it with UFW.

    ```bash
    sudo ufw app update plexmediaserver
    ```

3. Apply the rules.

    ```bash
    sudo ufw allow plexmediaserver-all
    ```

    If you just need to use Plex as a DLNA Server then apply the rule `plexmediaserver-dlna` and if you don't need DLNA, apply the `plexmediaserver` rule.
4. Check if the new rules are active.

    ```bash
    sudo ufw status
    ```

5. You would see something like this

    ```bash
    
     To                        Action        From
     -- ------ ----
     OpenSSH                   ALLOW       Anywhere
     plexmediaserver-all       ALLOW       Anywhere
     OpenSSH (v6)              ALLOW       Anywhere (v6)
     plexmediaserver-all (v6)  ALLOW       Anywhere (v6)
    ```

## Setup Plex Media Server

Before the setup, we need to first create directories where we will store the media files.

```bash
sudo mkdir -p /opt/plexmedia/{movies,shows}
```

The above command will create two directories `movies` and `shows` under `/opt/plexmedia`. You can choose any directory you want for your content.

Plex Media server uses `plex` user which should have read and write permissions on the directories we just created.

```bash
sudo chown -R plex:plex /opt/plexmedia
```

Now that the directories are all set up, we can proceed to set up our server. There is one more step left. Since Plex is installed on a remote server, for setting it up, you need to access it via an SSH Tunnel. This allows you to access Plex as if you are browsing it on localhost.

### Setup SSH Tunnel - Linux/Mac OS

On a Linux or a MAC OS system, you just need to use the following command in your terminal

```bash
ssh YOURSERVER_IP_ADDRESS -L 8888:localhost:32400
```

You should now be able to access Plex via `http://localhost:8888/web` as long as you are connected to the server. Request to your localhost will be redirected to your remote server.

### Setup SSH Tunnel - Windows

We will be using the Putty SSH client to set up our SSH Tunnel. Launch Putty and load your saved session as you normally would for connecting to your server. In the left-hand tree, open Connection>>SSH>>Tunnels and enter `8888` as the Source Port and `YOURSERVERIP:32400` as the destination. Checkmark both the boxes under Port Forwarding. Click Add and then click Open to connect to the server.

![Putty Client SSH Tunnel Settings](images/putty_2019-06-19_14-01-09.png#center "Putty Client SSH Tunnel Settings")

As long as you are connected to your server, you should now be able to access Plex by opening `http://localhost:8888/web` in your browser.

**Note:** You would need to use SSH Tunnel only for setting up the Plex Server. Once that is done, you can simply access it again by just visiting `http://YOURSERVER_IP:32400/web` or by logging in directly to `https://app.plex.tv/` in your browser.

### Configure Plex

When you first open the link, you will be greeted with the following screen

![Plex Welcome Page](images/firefox_2019-06-19_14-29-18.png#center "Plex Welcome Page")

You can create your Plex account from this screen if you still haven't so far. After logging in, you will see the following screen.

![Plex Welcome Page - Setup](images/chrome_2019-06-19_14-25-23.png#center "Plex Welcome Page - Setup")

Click **Got it** to proceed and dismiss the Plex Pass Popup that appears. You will now get the following screen.

![Plex Server Setup](images/chrome_2019-06-19_14-32-25.png#center "Plex Server Setup")

Give it any name you like and make sure the box along **Allow me to access my media outside my home** is checked. This will allow you to access Plex by just using your server IP address next time.

You will be asked to setup Library next.

![Plex Library Setup](images/chrome_2019-06-19_14-35-00.png#center "Plex Library Setup")

![Plex Library Setup](images/chrome_2019-06-19_14-37-05.png#center "Plex Library Setup")

Browse to the folder we created earlier to add a Library. You can add as many Libraries as you want.

![Plex Library Folder Selection](images/chrome_2019-06-19_14-40-44.png#center "Plex Library Folder Selection")

Click on the Add button and then on the Add Library. Click Next and then Done when you are finished adding Libraries.

You will now be greeted with the Plex Dashboard from where you can explore it more.

![Plex Home Page](images/chrome_2019-06-19_14-44-18-1024x495.png#center "Plex Home Page")

Once you are finished, you can now connect to your Plex Server from the desktop or from its apps.

This concludes our tutorial for setting up Plex Media Server for Ubuntu 18.04 Server.
