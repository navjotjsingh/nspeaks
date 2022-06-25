---
title: "Customize Microsoft Office setup to install and keep only selected Apps"
date: "2020-04-10"
categories: 
  - "useful-tips"
summary: Learn to customize MS Office setup.
---

Since Office 2010, the installer changed which doesn't allow you to select which office Apps you want to install. Fortunately, there is a method. This method will work both for newer installs and existing installs as well. Head over to [AskVG](https://www.askvg.com/tip-customize-microsoft-office-click-to-run-c2r-setup-to-install-selected-programs-only/) to read all about it.

To customize an existing install, run this command instead of the configure command.

```powershell
setup.exe /customize installapppreferences.xml
```
