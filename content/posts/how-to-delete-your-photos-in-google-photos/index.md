---
title: "How to delete all your photos  in Google Photos"
date: "2019-07-26"
categories: 
  - "useful-tips"
tags: 
  - "google"
  - "tips"
---

My hatred of Google keeps growing day by day. Off late the company keeps shutting down services which are still being used (Inbox) or just randomly take away features for no logical reasons.

One such feature that was removed was the [Google Drive sync with Google photos](https://blog.google/products/photos/simplifying-google-photos-and-google-drive/). It was a pretty neat feature. Upload pics in Drive and they will show up in Google photos and vice versa. And you can delete from one place and they will be gone from the other. This actually allowed anyone to empty their Google photos library pretty easily. Just create a Google photos folder in your drive and empty it. It was that easy.

But Google being Google thought let's remove it because why not? So now how does one go by deleting thousands of pics that keeps collecting which you don't need anymore or are probably shifting elsewhere.

A guy named [Rishab Manocha](https://www.rishabmanocha.com/) wrote a [cool piece of javascript](https://github.com/mrishab/google-photos-delete-tool) that makes this process a bit easy. The script automatically selects batch of photos and deletes them. It will keep on doing it till all photos are gone. The process requires you to keep your browser window open. So if you have couple thousands photos, I suggest you go get that cup of coffee.

First step is to Open Google photos page. You should see all the photos on that page. Now, launch Developer Tools in your browser. The shortcut key is usually Ctrl + Shift + I (or Command + Shift + I on MAC).

Switch to the Console Tab and paste the following piece of code and press enter.

**Note to Firefox Users:** Recent versions of Firefox don't allow pasting of code directly because it is disabled. To enable it, type 'allow pasting' in your console window and press enter. After that you should be able to use the code below as usual.

**Note:** The original script listed by Rishabh has a [long standing bug](https://github.com/mrishab/google-photos-delete-tool/issues/5) which hasn't been fixed. So don't use his code. Copy the fixed code from below.

```javascript
// Selector for Images and buttons
const ELEMENT_SELECTORS = {
    checkboxClass: '.ckGgle',
    deleteButton: 'button[title="Delete"]',
    confirmationButton: '#yDmH0d > div.llhEMd.iWO5td > div > div.g3VIld.V639qd.bvQPzd.oEOLpc.Up8vH.J9Nfi.A9Uzve.iWO5td > div.XfpsVe.J9fJmf > button.VfPpkd-LgbsSe.VfPpkd-LgbsSe-OWXEXe-k8QpJ.nCP5yc.DuMIQc.kHssdc.HvOprf'
}

// Time Configuration (in milliseconds)
const TIME_CONFIG = {
    delete_cycle: 7000,
    press_button_delay: 1000
};

let imageCount = 0;

let checkboxes;
let buttons = {
    deleteButton: null,
    confirmationButton: null
}

let deleteTask = setInterval(() => {

    checkboxes = document.querySelectorAll(ELEMENT_SELECTORS['checkboxClass']);

    if (checkboxes.length <= 0) {
        console.log("[INFO] No more images to delete.");
        clearInterval(deleteTask);
        console.log("[SUCCESS] Tool exited.");
        return;
    }

    imageCount += checkboxes.length;

    checkboxes.forEach((checkbox) => { checkbox.click() });
    console.log("[INFO] Deleting", checkboxes.length, "images");

    setTimeout(() => {

        buttons.deleteButton = document.querySelector(ELEMENT_SELECTORS['deleteButton']);
        buttons.deleteButton.click();

        setTimeout(() => {
            buttons.confirmation_button = document.querySelector(ELEMENT_SELECTORS['confirmationButton']);
            buttons.confirmation_button.click();
        }, TIME_CONFIG['press_button_delay']);
    }, TIME_CONFIG['press_button_delay']);
}, TIME_CONFIG['delete_cycle']);
```

Now you just need to sit back and relax and let the script do its job.

And if you are wondering why isn't there an easier way like a desktop app or something which can do the job without loading the Google photos site, again blame Google. Why? Because there is no API to use or automate this job. Fuck you, Google.
