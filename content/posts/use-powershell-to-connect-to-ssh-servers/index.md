---
title: "Use Powershell to connect to SSH Servers"
date: "2020-02-09"
category: 
  - "useful-tips"
tag: 
  - "powershell"
  - "ssh"
summary: Learn to connect to an SSH server using MS Powershell.
slug: use-powershell-to-connect-to-ssh-servers
showToc: false
---

For years, I have stuck to using Putty for my SSH needs on Windows 10. It works well but I was looking into alternative SSH clients which is when I found that Windows 10 ships with an OpenSSH Client/Server package. So if you want to try an alternative, this can work as a solid tool. Do note that Putty has much more features than this so if your needs are restricted to using SSH, this should be sufficient.

If you have all the recent Windows 10 Updates, then OpenSSH client is already available for you. But do check before proceeding with the tutorial. To do that, open **Windows Settings > Apps & Features > Optional Features**. Scroll the list of installed features to see if you have OpenSSH Client installed. Here's how it shows on my Windows.

![OpenSSH Client Installed on Windows 10](images/ApplicationFrameHost_2020-01-29_18-22-10-e1580302821620.png#center)

If you don't see it in the list, then proceed to Add a Feature and Install it from there. You can also install it via Powershell. Launch Powershell in Administrator mode and enter the following command to install the SSH Client.

```powershell
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0
```

If your server uses Password only Authentication, you can proceed straight away to connect to it.

```bash
ssh user1@example.com
# UPN syntax works...
ssh user1@domain1@example.com
# ...as does NetBIOS syntax
ssh user1\domain1@example.com
```

## Key Based Authentication

We need few more steps for key based authentication. For the purpose of the tutorial, you should have your server's Private and Public keys available with you on the System.

We will use _ssh-agent_ service to store the private keys. This will store the keys and automatically supply them when you use the ssh command to connect.

Copy both the public and private keys of the server to _C:/Users/<Username>/.ssh_ directory. Installation of the OpenSSH Client automatically creates the _.ssh_ directory. If its not there for some reason, create it manually.

Use the following the command to add the private key.

```bash
ssh-add C:\Users\<Username>\.ssh\id_rsa
```

OR

```bash
ssh-add ~\.ssh\id_rsa
```

**~** refers to the Current User directory in Windows just like it does in Linux.

Now you can simply use `ssh user1@example.com` command and your key is used automatically.
