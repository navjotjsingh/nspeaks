---
title: "How to Enable Group Policy Editor on Windows 10 Home Edition"
date: "2015-08-15"
categories: 
  - "useful-tips"
cover: 
  image: "images/2015-08-15_14-20-57.png"
---

Group Policy Editor is one of the most powerful tools that allow users to manage hidden settings used to enable or disable some pretty useful features of Windows. Since Windows 10, Microsoft has shipped Group Policy Editor only for the Pro and Enterprise versions of Windows but not the home versions.

The following tutorial will allow you to use Group Policy Editor on Windows 10 Home Editions as well. The Tutorial is valid for Windows 7, 8, 8.1 as well. For this we will use Group Policy Editor created by a user "davehc" on [Windows 7 Forums](http://www.w7forums.com/threads/install-gpedit-on-win-7-home-editions.10839/) \[available via Drudger @ [DeviantArt](http://drudger.deviantart.com/art/Add-GPEDIT-msc-215792914)\]

1. Download the Installer from [DeviantArt Page](http://drudger.deviantart.com/art/Add-GPEDIT-msc-215792914).

2. Run the Installer.

3. Don't close the installer if you have a 64 bit system i.e. Don't press Finish. Go to `%WinDir%\Temp` folder and copy the files **gpedit.dll**, **fde.dll**, **gptext.dll**, **appmgr.dll**, **fdeploy.dll** and **gpedit.msc** to the `%WinDir%\System32` folder. You also need to copy the **GroupPolicy**, **GroupPolicyUsers** and **GPBAK** folders from `%WinDir%\SysWOW64` to the `%WinDir%\System32` directory.

That's all. The above method works fine on a 64-bit system but I can't confirm the same for a 32-bit system yet though I don't see a reason why it shouldn't work.

**Note:** Not all options might work since the gpedit.msc file was designed for Windows 7.
