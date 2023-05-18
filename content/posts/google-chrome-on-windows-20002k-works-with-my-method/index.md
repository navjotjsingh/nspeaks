---
title: "Google Chrome on Windows 2000/2k? Works with my method"
date: "2008-09-07"
category:
  - "technology"
tag:
  - "google-chrome"
summary: Learn how to use Google Chrome Browser on a Windows 2000 Machine (Does not work anymore).
slug: google-chrome-on-windows-20002k-works-with-my-method
---

**Disclaimer (18 May 2023):** This method does not work anymore. This post is just for informational purpose. You should upgrade to a newer OS. It is not safe to use Windows 2000. This post won't be updated again.

Google Chrome works only on Windows XP and above. Installer won't even run on Windows 2000. But a [japanese guy found a method to make it work on Windows 2000](http://blog.livedoor.jp/blackwingcat/archives/360097.html) too. I am listing it below for you.

So here is my method to make Chrome work on Windows 2000.

1. Make sure you have the following patch in 2000 Installed: [Update Rollup 1 for Windows 2000 SP4](https://www.catalog.update.microsoft.com/Search.aspx?q=Windows%202000%20Service%20Pack%204) ([KB891861](https://www.catalog.update.microsoft.com/Search.aspx?q=KB891861) and [KB935839](https://www.catalog.update.microsoft.com/Search.aspx?q=KB935839))
2. ~~You would also need this patch - [KB915985.](https://support.microsoft.com/kb/915985) If your system has it already then you can skip this step.~~
3. Download the [crm2k70.cab](/download/crm2k70.cab) file (Tested upto Chrome v32).
4. Run the CHROME2K.exe file after extracting from the cab file and apply settings accordingly.
5. Download Chrome using the button in CHROME2K.exe after applying the compatible registry and user agent. Do set locale for Chrome before downloading.
6. And voila, Chrome now runs in Windows 2000.
7. To uninstall Chrome run the offical uninstaller from Add Remove Programs and then run CHROME2K.exe file, check Display for uninstall and then select Clean up setting registry and Uninstall wrapper registry.

Please report if the above method does not work. Or a fix still needed for above method. Hope my method works for everybody.

**Unfortunately its impossible to manage the comments on this post so am shutting down the comments. Most likely you may find the workarounds to make Google Chrome work in this post. If not then I won't be able to help you out as I don't use a Windows 2000 machine.**
