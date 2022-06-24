---
title: "How to stop Windows 10 from forcing Edge and Bing"
date: "2021-07-24"
categories: 
  - "useful-tips"
---

Before moving ahead with the article, one thing you should know. Never try to remove or disable the Microsoft Edge browser. Doing so can cause a number of issues because it is so deeply embedded into the OS, that things might stop working and the system can become unstable.

Ever since Microsoft launched the stable Edge Chromium browser, it has made it the default browser of choice for every link that you click from within any Windows App or Search. Taking away a user's choice of default browser is not a good idea in my opinion and will cause more distrust even though Edge is a decent browser and even better than Chrome in some ways.

In this tutorial, I will discuss how you can stop this behavior of Windows. Also, I will show how you can stop default searches done from inside Cortana to use the search engine of your choice instead of Bing. This will require you to install software and a browser extension in a two-step process.

## Step 1 - Replace Edge

For the first step, you need to download a tool called EdgeDeflector. It is a lightweight application that intercepts any call made to Edge for links. It redirects those calls to the browser of your choice.

Download the latest version of EdgeDeflector from its [Github releases page](https://github.com/da2x/EdgeDeflector/releases). Install it. After installation, you should see a dialogue pop up asking how you want to open this. Select EdgeDeflector and click Ok. After that, all forced Microsoft Edge links will open automatically in the browser of your choice. If you don't get the dialogue, you can set the option manually.

![Edge Deflector Selection Screen](images/edge-deflector-post-install.webp#center)

For the manual setup, open Windows settings (Win key + I) and open Apps >> Default apps >> Choose default apps by protocol. Find the entry labeled "MICROSOFT-EDGE" and select Choose a default. Select EdgeDeflector in the menu that pops up.

![EdgeDeflector Setup - Default App Settings](images/edgedeflector_setup.png#center)

From now on, every link from within a Windows app will redirect to the system's default browser and not Edge.

## Step 2 - Replace Bing

The second step is to redirect any Bing.com search. This will require you to install a browser extension. For Chrome and Chrome-based browsers, install the extension [Chrometana Pro](https://chrome.google.com/webstore/detail/chrometana-pro-redirect-c/lllggmgeiphnciplalhefnbpddbadfdi/related). And if you are using Firefox, then install the [Foxtrana](https://addons.mozilla.org/en-GB/firefox/addon/foxtana-pro-redirect-cortana/) extension. You can now redirect any Bing.com search to either Google, Yahoo, DuckDuckGo, or Baidu. You can also redirect the search to any custom Search Engine of your choice by giving its URL and search parameter.

All Bing searches, regardless of where they originate from, will be redirected to Google now. This includes both, search through Cortana or the Start Menu as well as any manual Bing searches or Bing searches that Edge browser forces.
