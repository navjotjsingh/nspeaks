---
title: "Does your browser support the new image format JPEG XL?"
date: "2021-05-10"
category:
  - "technology"
summary: Check if your browser supports the latest JPEG XL Image format.
slug: jpeg-xl-browser-support
---

<picture>
        <source srcset="images/nature_picture.jxl" type="image/jxl" align="center">
        <img src="images/nature_picture.jpg#center" alt="JPEG Fallback picture for JPEG XL format">
</picture>

**Note:** If your browser supports JPEG XL, you will see the image with the text saying it supports JPEG XL. Else, you will see a fallback JPEG image.

## What is JPEG XL?

JPEG XL is a next-generation image codec being in the process of being standardized by the JPEG Committee. It separates itself from other newer formats like WebP, AVIF, and HEIC as it is not based on any video codec like they are. This gives it an advantage as it has been specifically made to display images and offers much better quality and smaller file sizes than any other codec.

You can read more about JPEG XL at

1. [How JPEG XL Compares to Other Image Codecs](https://cloudinary.com/blog/how_jpeg_xl_compares_to_other_image_codecs)
2. [Time for Next-Gen Codecs to Dethrone JPEG](https://cloudinary.com/blog/time_for_next_gen_codecs_to_dethrone_jpeg)
3. [JPEG XL Caniuse](https://caniuse.com/jpegxl)
4. [JPEG XL Reddit](https://www.reddit.com/r/jpegxl/)
5. [JPEG XL Specification](https://jpeg.org/jpegxl/)
6. [JPEG XL Gitlab Repository](https://gitlab.com/wg1/jpeg-xl)

## Which browsers support JPEG XL?

As of [17 December 2022](https://caniuse.com/jpegxl), Firefox Beta 90+, Edge Stable 91+, Opera 77+, Brave 1.27.111+, and Vivaldi 4.1.2369.16 support the JPEG XL format. [Google removed support for JPEG-XL](https://bugs.chromium.org/p/chromium/issues/detail?id=1178058#c84) with v110. You can still enable use JPEG-XL in Chrome versions 91-109.

To enable JPEG XL format in Chrome versions v91-109, open Chrome flags ( chrome://flags ) settings page and enable the option **Enable JXL image format** ( [chrome://flags/#enable-jxl](chrome://flags/#enable-jxl)).

![Chrome Flag setting for enabling JPEG XL image format](images/chrome_yZPxuunM3d.png#center)

**Firefox Beta (90+)** supports JPEG XL. To achieve that, you need to set the value of `image.jxl.enabled` to `true` on the `about:config` page.

![Firefox Setting for enabling JPEG XL image format](images/firefox_gxs3xPsRAg.png#center)

**Firefox Nightly (91+)** offers a much easier way to enable support for the JXL image format. Open your Firefox settings page and visit the **Nightly Experiments** page and enable support for the JXL format from there.

![Firefox Nightly(91+) settings for enabling JPEG XL format](images/firefox_KiMQpKc7er.png#center)

**Firefox Stable** supports the JXL configuration flag but it doesn't work at the moment.

**Microsoft Edge Stable (91+)** supports JPEG XL as well. To enable the feature, you need to launch the browser executable with the parameter. `--enable-features=JXL`.

![Edge Shortcut explorer Properties box to add support for JPEG XL format](images/explorer_Pdl5NH3ZjO.png#center)

**Opera 77** added support for the JXL format. It works in the same way as it does in the Chrome browser. Visit the link ( opera://flags/#enable-jxl ) in your browser address bar.

![Opera Flag setting for enabling JPEG XL image format](images/vuTucaoQSZ.png#center)

**Vivaldi 4.1.2369.16** supports the JXL image format as well. It again works the same way as it does in the Chrome browser. Visit the link ( vivaldi://flags/#enable-jxl ) in your browser address bar.

![Vivaldi Flag setting for enabling JPEG XL image format](images/3mooLbSmgr.png#center)

**Brave 1.27.111** also added support for the JXL image format in the same way as the Chrome browser. Visit the link ( brave://flags/#enable-jxl ) in your browser address bar.

![Brave Flag setting for enabling JPEG XL image format](images/brave_zySKrmkIfW.png#center)

## How to convert your photos to JPEG XL format?

The easiest way to convert your photos to JPEG XL format is to use online converter apps.

1. **[Squoosh App](https://squoosh.app)**. It can be installed as a native PWA app on phones and web browsers. It can only convert one file and offer various quality settings along with a live preview.
2. [**jpegxl-converter**](https://jpegxl-converter.com/) - can convert one file at a time and offers lossless encoding along with quality settings. Lossless compression might not work with large files, though.
3. [**MConverter**](https://mconverter.eu/convert/to/jxl/) - It works very similarly to Squoosh and can be installed as a native app as well. It supports file sizes up to 500 MB.

## How to embed JPEG XL pics on your webpage

It would be best to use the HTML **<picture>** tag to embed JPEG XL pics on your site. Here is a sample code you can work with.

```html
<picture>
        <source srcset="https://example.com/images/picture.jxl" type="image/jxl">
        <img src="https://example.com/images/jpeg_fallback_picture.jpg" alt="JPEG Fallback picture for JPEG XL format">
</picture>
```

**Note:** This page will be continuously updated as and when more tools and browser support are available.

**Last Updated:** 17 December 2022
