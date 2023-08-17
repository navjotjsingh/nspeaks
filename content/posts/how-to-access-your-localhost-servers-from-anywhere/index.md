---
title: "How to Access your Localhost Servers from anywhere"
date: "2019-03-20"
category:
  - "useful-tips"
tag:
  - localhost
summary: How to make your Localhost Server public on the Internet.
slug: how-to-access-your-localhost-servers-from-anywhere
---

Developing Apps and Sites for Clients on Localhost is easy. But the next step i.e. showing the clients what you did can be painful. It involves uploading all the work to your server and then modifying the urls so that everything works perfectly.

Luckily, there is an easier way to do it where you can show the clients your work without uploading it anywhere. There are several services that will allow you to make your localhost go public but we will be sticking to ngrok.

### What is ngrok?

[ngrok](https://ngrok.com) is a tunneling service that exposes your localhost webserver to public over secure tunnels. ngrok will provide you with a publicly accessible url which you can share with your clients and will forward all requests to the url to your localhost server.

ngrok is available for Windows, Linux and Mac OS systems. Basic features are available for free with some restrictions. Paid accounts will allow you to set custom subdomains/domains for your tunnel. Remember, with free account, your tunnel url will change on every restart of the service.

![ngrok demo](images/ngrok-demo-static.png#center)

### How to use ngrok

```bash
./ngrok authtoken your_auth_token_here
```

Head over to ngrok.com to create your account. Then extract the zip file on your system using your preferred archive software and then fire up the terminal/command prompt(if you are on Windows). Run the following command

**Mac OS:** `/Users/example/.ngrok2/ngrok.yml`

**Linux:** `/home/example/.ngrok2/ngrok.yml`

**Windows:** `C:\Users\example\.ngrok2\ngrok.yml`

This command will link your ngrok account to the app you have installed by creating a configuration file named `ngrok.yml` on your system. This file can be found at following directories depending upon your systems

Run the following command to start ngrok on your system which will automatically generate a random url where you can access your localhost server.

```bash
./ngrok http 80
```

This command links up the 80 port of your localhost to its own service which allows it to forward all requests made to your localhost server over to ngrok's tunnel. You can modify the command according to the port you are using for your localhost server.

That's it. Your local site is now live on the web. But wait. That's not all? If you want, you can protect your online url by enabling HTTP authentication. Use the following command for it

```bash
ngrok http -auth="username:password" 80
```

If you are a paid user who has access to sub-domains, then use the following command to launch the tunnel on a custom sub-domain.

```bash
ngrok http -subdomain=nspeaks 80
```

There are lots more features which you can check in its extensive [documentation](https://ngrok.com/docs).

You may face one hurdle with ngrok and that is it does not run as a service. Unless you are using ngrok link service, there is really no native support for running ngrok as a service. Fortunately, someone at [Stackexchange](https://stackoverflow.com/a/50808709) figured out a way and has posted a solution. Head over to there if you want to know how to run ngrok as a service.
