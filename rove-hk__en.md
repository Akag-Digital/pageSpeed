
# 📊 Performance Optimization Plan
**Date**: September 30, 2025  
**Analysis**: Google PageSpeed Insights   
**URL**: https://rovelab.uk  
**Repository URL**: https://github.com/RoveLab/ecommerce-shopify-rove-uk  
**PageSpeed Insights**: https://pagespeed.web.dev/analysis/https-rovelab-uk/zqofduni06?form_factor=mobile 

<br>

---

<br>

This document summarizes the main issues identified in **PageSpeed** and organizes them into two categories:

- **Actions that can be implemented by the developer**  
- **Items related to external apps** (cannot be directly adjusted in Shopify)  

<br>

---

<br>

## ✅ Developer Actionable Items

### 1. Render-blocking files
**The problem:** Some CSS and JS files delay the site’s initial rendering, increasing the time it takes for main content to appear.  

**Files identified:**  
- `announcement-bar.css` – can be optimized to load non-blocking.  

**What cannot be changed:** Shopify files (`accelerated-checkout-backwards-compat.css` and `advanced-switcher.css`).  

**Expected impact:** Reduced perceived load time for the user.  

---

### 2. Image optimization
**The problem:** Some images are being loaded at larger dimensions or formats than necessary (**~300 KiB savings possible**).  

**Files identified:**  
1. [`luxury-living-room-mobile_1180x1300_crop_center.jpg`](https://rovelab.hk/cdn/shop/files/luxury-living-room-mobile_1180x1300_crop_center.jpg?v=1730148988) – 151.1 KiB  
2. [`M_BANNER_DEFAULT_2_1_1080x1740_crop_center.jpg`](https://rovelab.hk/cdn/shop/files/M_BANNER_DEFAULT_2_1_1080x1740_crop_center.jpg?v=1738154508) – 104.6 KiB  
3. [`c760184679e94704b04db72e70988ccd.thumbnail.0000000000_690x.jpg`](https://rovelab.hk/cdn/shop/files/preview_images/c760184679e94704b04db72e70988ccd.thumbnail.0000000000_690x.jpg?v=1730149102) – 7.5 KiB  

**Files identified in the Cevoid Gallery app (check if images can be modified there):**  
1. [`39GIXyFihp2i5CUzhaUsZ/thumbnail.png`](https://cevoidcontent.com/images/39GIXyFihp2i5CUzhaUsZ/thumbnail.png?class=400) – 37.7 KiB  
2. [`c80YZm3P5xTIb3CUwJufB/thumbnail.png`](https://cevoidcontent.com/images/c80YZm3P5xTIb3CUwJufB/thumbnail.png?class=400) – 31.1 KiB  
3. [`oAngAg3pTJjVcUhIqbpSN/original.jpeg`](https://cevoidcontent.com/images/oAngAg3pTJjVcUhIqbpSN/original.jpeg?class=400) – 21.4 KiB  
4. [`RyX9FEfUIgaC24GL4-wbK/original.jpeg`](https://cevoidcontent.com/images/RyX9FEfUIgaC24GL4-wbK/original.jpeg?class=400) – 21.0 KiB  
5. [`sPByAmb782-6q24cpyXRE/original.jpeg`](https://cevoidcontent.com/images/sPByAmb782-6q24cpyXRE/original.jpeg?class=400) – 20.9 KiB  

**Possible actions:**  
- Generate smaller, responsive versions.  
- Convert to WebP or AVIF.  
- Apply optimized JPG compression.  

**Expected impact:** Significant reduction in total weight and noticeable improvement in loading speed.  

---

### 3. Offscreen images (lazy loading)
**The problem:** Some small preview images are loading even when outside the initial viewport.  

**Files identified:**  
1. [preview_images/c760184679e94704b04db72e70988ccd.thumbnail.0000000000_690x.jpg](https://rovelab.hk/cdn/shop/files/preview_images/c760184679e94704b04db72e70988ccd.thumbnail.0000000000_690x.jpg?v=1730149102) – 7.5 KiB  
2. [M1_2-Seat-Sofa-Loveseat_PW_Krypton_Green_2_375x_crop_center.jpg](https://rovelab.hk/cdn/shop/files/M1_2-Seat-Sofa-Loveseat_PW_Krypton_Green_2_375x_crop_center.jpg?v=1729896261) – 7.0 KiB  
3. [M1_3-Seat-Sofa_PW_Platinum_Grey_1_64508b7d-4b51-4c00-a40a-2b9b64fb52ad_375x_crop_center.jpg](https://rovelab.hk/cdn/shop/files/M1_3-Seat-Sofa_PW_Platinum_Grey_1_64508b7d-4b51-4c00-a40a-2b9b64fb52ad_375x_crop_center.jpg?v=1732049989) – 5.2 KiB  
4. [M1_Sectional_PW_Carbon_Black_3_375x_crop_center.jpg](https://rovelab.hk/cdn/shop/files/M1_Sectional_PW_Carbon_Black_3_375x_crop_center.jpg?v=1729896544) – 4.0 KiB  
5. [bazaar_1_20x4_crop_center.svg](https://rovelab.hk/cdn/shop/files/bazaar_1_20x4_crop_center.svg?v=1730149389) – 4.3 KiB (3.8 KiB in another variation)  
6. [preview_images/cd3fef1140ee48f89fb6d1c2dfd0377d.thumbnail.0000000000_690x.jpg](https://rovelab.hk/cdn/shop/files/preview_images/cd3fef1140ee48f89fb6d1c2dfd0377d.thumbnail.0000000000_690x.jpg?v=1730149170) – 2.3 KiB  

**Possible actions:**  
- Enable `loading="lazy"`.  

**Expected impact:** Potential savings of ~30 KiB.  

---

### 4. Excessive payload size (15.7 MB)
**The problem:** The site loads **15.7 MB of resources**, mostly from heavy images and videos.  

**Files identified:**  
1. [c760184679e94704b04db72e7098...3904.mp4](https://rovelab.hk/cdn/shop/videos/c/vp/c760184679e94704b04db72e70988ccd/c760184679e94704b04db72e70988ccd.HD-1080p-7.2Mbps-37243904.mp4?v=0) – 9,047.8 KiB  
2. [cd3fef1140ee48f89fb6d1c2dfd03...944.mp4](https://rovelab.hk/cdn/shop/videos/c/vp/cd3fef1140ee48f89fb6d1c2dfd0377d/cd3fef1140ee48f89fb6d1c2dfd0377d.HD-1080p-7.2Mbps-37243944.mp4?v=0) – 6,395.3 KiB  
3. [luxury-living-room-mobile...center.jpg](https://rovelab.hk/cdn/shop/files/luxury-living-room-mobile_1180x1300_crop_center.jpg?v=1730148988) – 152.5 KiB  
4. [M_BANNER_DEFAULT_2_1...center.jpg](https://rovelab.hk/cdn/shop/files/M_BANNER_DEFAULT_2_1_1080x1740_crop_center.jpg?v=1738154508) – 106.0 KiB  

**Possible actions:**  
- Compress images and videos.  
- Generate responsive versions.  
- Convert to WebP/AVIF.  

**Expected impact:** Significant reduction in total page weight.  

---

### 5. LCP request discovery
**The problem:**  
The main LCP (Largest Contentful Paint) image is configured correctly with `fetchpriority="high"` and `loading="eager"`. However, the report suggests detection could be optimized to ensure the browser identifies this image as critical even earlier. This can be improved by adding a *preload* directive in the Liquid template.  

**Element identified:**  

```html
<img 
  class="single-image--mobile lazyautosizes ls-is-cached lazyloaded" 
  width="1080" 
  height="1740" 
  data-sizes="auto" 
  src="https://rovelab.hk/cdn/shop/files/M_BANNER_DEFAULT_2_1_1080x1740_crop_cent…" 
  data-srcset="//rovelab.hk/cdn/shop/files/M_BANNER_DEFAULT_2_1_1080x1740_crop_center.jpg…" 
  fetchpriority="high" 
  alt="" 
  style="object-position: 50.0% 50.0%;" 
  sizes="412px" 
  srcset="//rovelab.hk/cdn/shop/files/M_BANNER_DEFAULT_2_1_1080x1740_crop_center.jpg…"
>

```
---

<br>

## 📊 Priority Table (Possible Actions)

| Priority | Task                                   | Expected Impact | Time Estimate       |
|----------|----------------------------------------|-----------------|---------------------|
| 🔴 High  | Image optimization (Items 2 and 4)     | Very high       | 4-5 hours           |
| 🔴 High  | LCP request discovery (Item 5)         | High            | 30 min – 1 hour     |
| 🟠 Medium| Script removal/deferral (Item 1)       | Medium          | 30 min – 1 hour     |
| 🟡 Low   | Lazy loading for images (Item 3)       | Low             | 1-2 hours           |

---

<br>

## ⚠️ Items related to external apps (not adjustable)

These items were flagged by PageSpeed but **cannot be directly optimized**, as the files come from third-party apps. The only alternative would be to **remove or replace the apps**, if desired.  

### Short cache lifetime of external resources  
- Facebook Pixel – 117 KiB (cache 0–20 min)  
- Loox (reviews) – 41 KiB (cache 1 day and 12 h)  
- Clarity – 32 KiB (cache 1 day)  
- Klaviyo – 3 KiB (cache 1 s)  

---

### Legacy JavaScript (77 KiB)  
- Klaviyo – 17.8 KiB  
- Rebuy – 27.3 KiB  
- Facebook Pixel – 12.5 KiB  

---

### Unused JavaScript (1.43 MB)  
- Google Tag Manager – 275.2 KiB  
- Rebuy – 213.8 KiB  
- Converge – 189.8 KiB  
- Klaviyo – 49.8 KiB  
- Facebook – 82.7 KiB  
- Loox – 40.5 KiB  

---

### Unused CSS (28 KiB)  
- Rebuy – 19.5 KiB  
- app.css (store theme) – 13.2 KiB  

<br>

---

<br><br>

# 📌 Conclusion
A large portion of the reported issues are related to **external apps**, which cannot be adjusted in Shopify.  
The main performance gains will come from **image optimization, layout improvements (CLS), and script/initial load adjustments**, which can be handled by the development team.  


