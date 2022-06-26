---
title: "JPG images getting saved as JFIF? Here's how to fix."
date: "2020-02-24"
category: 
  - "useful-tips"
summary: Learn how to download JPEG images instead of JFIF from webpages.
slug: jpg-images-getting-saved-as-jfif-heres-how-to-fix
---

Recently I came across an annoying issue whenever I tried saving a .jpg file from the web. It always ended up getting saved as .jfif. JFIF is an image format for exchanging JPEG encoded files. JFIF files should open normally in all popular image editors and if it doesn't, you can always rename files from .jfif to .jpg. The reason this has started happening is because of Windows 10 Creators Update. I am not sure what was Microsoft thinking that saving all .jpg files in a non-standard format is the way to go. Anyway, there is a simple registry fix for that.

Follow this method only if you are comfortable with editing the Windows Registry. If you are not then skip to the end of this post where I have a simpler method that would still edit your registry but it will be done in a safe way.

1. Launch Registry Editor using Windows Search
2. Navigate to **HKEY\_CLASSES\_ROOT > MIME > Database > Content Type > image/jpeg**
3. In the right panel, double click on the Extension key. Its value should read as .jfif.
4. Change the value to .jpg and click Okay.
5. Close the Registry Editor.

The change should be instant and you should now be able to save JPG files from the web in .jpg format as it should be.

### The Safe Way

So you are not comfortable editing the Windows Registry yourself. Good choice. To apply the fix in a safe way, download the file below and save it someplace safe on your PC. Double click on the file in the explorer and accept the changes. That's it. You should now be able to save files as .jpg again.

[jpgNOTjfif.reg](/download/jpgNOTjfif.reg)

I am hoping Microsoft will fix this bug in the future unless it was intentional in which case it is stupid.
