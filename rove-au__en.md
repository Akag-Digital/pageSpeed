# 📊 Performance Optimization Plan  
**Date**: September 30, 2025  
**Analysis**: Google PageSpeed Insights  
**URL**: https://rovelab.au  
**Repository URL**: https://github.com/RoveLab/ecommerce-shopify-rove-au  
**PageSpeed Insights**: https://pagespeed.web.dev/analysis/https-rovelab-au/mzbpim9uep?form_factor=mobile  

<br>

---

<br>

This document brings together the main points identified in **PageSpeed** and divides them into two categories:

- **Actions that can be performed by the developer**  
- **Items related to external apps** (cannot be directly adjusted in Shopify)  

<br>

---

<br>

## ✅ Items that can be addressed (developer)

### 1. Render-blocking files  
**The problem:** Some CSS and JS files delay the initial rendering of the site, increasing the time needed to display the main content.  

**Files identified:**  
- `animations.min.js` – can be removed if not essential.  
- `multi-image.css` – can be optimized to load in a non-blocking way.  
- `bundle.js` (Intelligems) – confirm if the app is still in use; if not, it can be removed.  

**Cannot be done:** Shopify files (`accelerated-checkout-backwards-compat.css` and `native-geo-redirects.min.css`).  

**Expected impact:** Reduced perceived load time for the user.  

---

### 2. Image optimization  
**The problem:** Some images are larger than necessary in dimensions or format (**~859 KiB savings**).  

**Files identified:**  
1. [`SWATCH_PW_Copper_Tan.jpg`](https://rovelab.au/cdn/shop/files/SWATCH_PW_Copper_Tan.jpg?v=1736181995) – 358.9 KiB  
2. [`M_HERO_BANNER_7_1_07a2db7a-f369-4776-b989-f1c724f87ad2_1180x1740_crop_center.jpg`](https://rovelab.au/cdn/shop/files/M_HERO_BANNER_7_1_07a2db7a-f369-4776-b989-f1c724f87ad2_1180x1740_crop_center.jpg?v=1758549532) – 202.2 KiB  
3. [`SWATCH_PW_Krypton_Green.jpg`](https://rovelab.au/cdn/shop/files/SWATCH_PW_Krypton_Green.jpg?v=1736181995) – 284.3 KiB  
4. [`SWATCH_PW_Platinum_Grey.jpg`](https://rovelab.au/cdn/shop/files/SWATCH_PW_Platinum_Grey.jpg?v=1736181994) – 261.2 KiB  
5. [`SWATCH_PW_Silicon_Sand.jpg`](https://rovelab.au/cdn/shop/files/SWATCH_PW_Silicon_Sand.jpg?v=1736181995) – 261.0 KiB  
6. [`SWATCH_PW_Helium_Cloud.jpg`](https://rovelab.au/cdn/shop/files/SWATCH_PW_Helium_Cloud.jpg?v=1736181995) – 234.1 KiB  
7. [`M1_2-Seat-Sofa-Loveseat_PW_Copper_Tan_1_770x_crop_center.jpg`](https://rovelab.au/cdn/shop/files/M1_2-Seat-Sofa-Loveseat_PW_Copper_Tan_1_770x_crop_center.jpg?v=1746622571) – 25.1 KiB  
8. [`m1-modular-sectional-sofa-krypton-green-front-view_770x_crop_center.webp`]() – 22.6 KiB  
9. [`SWATCH_PW_Hydrongen_Blue_3.jpg`](https://rovelab.au/cdn/shop/files/SWATCH_PW_Hydrongen_Blue_3.jpg?v=1749077562) – 183.7 KiB  
10. [`M1_3-Seat-Sofa_PW_Helium_Cloud_1_770x_crop_center.jpg`](https://rovelab.au/cdn/shop/files/M1_3-Seat-Sofa_PW_Helium_Cloud_1_770x_crop_center.jpg?v=1745536953) – 14.7 KiB  

**What can be done:**  
- Create smaller, responsive versions.  
- Convert to WebP or AVIF.  
- Apply optimized JPG compression.  

**Expected impact:** Significant reduction in total weight and noticeable improvement in loading speed.  

---

### 3. Offscreen images (lazy loading)  
**The problem:** Some small preview images load even though they are offscreen.  

**Files identified:**  
- [2499e01a7aba4bbbb47761687a94..._690x.jpg](https://rovelab.au/cdn/shop/files/preview_images/2499e01a7aba4bbbb47761687a942e2c.thumbnail.0000000000_690x.jpg?v=1744201916) – 7.5 KiB  
- [fdb4117785ab4b4aa5439e1c63eeb5..._690x.jpg](https://rovelab.au/cdn/shop/files/preview_images/fdb4117785ab4b4aa5439e1c63eeb5f8.thumbnail.0000000000_690x.jpg?v=1744201992) – 2.3 KiB  

**What can be done:**  
- Enable `loading="lazy"`.  

**Expected impact:** Minimal (~10 KiB), but avoids unnecessary downloads during initial load.  

---

### 4. Excessive payload size (22.92 MB)  
**The problem:** The site loads **22.92 MB of resources**, much of it heavy images and videos.  

**Files identified:**  
1. [2499e01a7aba4bbbb47761687a942e2c.HD-1080p-7.2Mbps-45643103.mp4](https://rovelab.au/cdn/shop/videos/c/vp/2499e01a7aba4bbbb47761687a942e2c/2499e01a7aba4bbbb47761687a942e2c.HD-1080p-7.2Mbps-45643103.mp4?v=0) — 9,044.3 KiB  
2. [fdb4117785ab4b4aa5439e1c63eeb5f8.HD-1080p-7.2Mbps-45643177.mp4](https://rovelab.au/cdn/shop/videos/c/vp/fdb4117785ab4b4aa5439e1c63eeb5f8/fdb4117785ab4b4aa5439e1c63eeb5f8.HD-1080p-7.2Mbps-45643177.mp4?v=0) — 6,380.4 KiB  
3. [SWATCH_PW_Copper_Tan.jpg](https://rovelab.au/cdn/shop/files/SWATCH_PW_Copper_Tan.jpg?v=1736181995) — 360.3 KiB  
4. [SWATCH_PW_Krypton_Green.jpg](https://rovelab.au/cdn/shop/files/SWATCH_PW_Krypton_Green.jpg?v=1736181995) — 285.8 KiB  
5. [D_BANNER_SPRING_SALE_AU_1_2800x1200_crop_center.jpg](https://rovelab.au/cdn/shop/files/D_BANNER_SPRING_SALE_AU_1_2800x1200_crop_center.jpg?v=1758549533) — 272.6 KiB  
6. [SWATCH_PW_Platinum_Grey.jpg](https://rovelab.au/cdn/shop/files/SWATCH_PW_Platinum_Grey.jpg?v=1736181994) — 262.7 KiB  
7. [SWATCH_PW_Silicon_Sand.jpg](https://rovelab.au/cdn/shop/files/SWATCH_PW_Silicon_Sand.jpg?v=1736181995) — 262.5 KiB  
8. [SWATCH_PW_Helium_Cloud.jpg](https://rovelab.au/cdn/shop/files/SWATCH_PW_Helium_Cloud.jpg?v=1736181995) — 235.6 KiB  
9. [M_HERO_BANNER_7_1_07a2db7a-f369-4776-b989-f1c724f87ad2_1180x1740_crop_center.jpg](https://rovelab.au/cdn/shop/files/M_HERO_BANNER_7_1_07a2db7a-f369-4776-b989-f1c724f87ad2_1180x1740_crop_center.jpg?v=1758549532) — 203.7 KiB  

**What can be done:**  
- Compress images.  
- Create responsive versions.  
- Convert to WebP/AVIF.  

**Expected impact:** Significant reduction in total page weight.  

---

### 5. LCP request discovery  

**The problem:**  
The main LCP (Largest Contentful Paint) image is correctly configured with `fetchpriority="high"` and `loading="eager"`. However, the report signals that detection could be further optimized to ensure the browser identifies this image as critical even sooner. This can be improved by adding a *preload* in the Liquid image block.  

**Element identified:**  

```html
<img 
  loading="eager" 
  class="single-image--mobile lazyautosizes ls-is-cached lazyloaded" 
  width="1180" 
  height="1740" 
  data-sizes="auto" 
  src="https://rovelab.au/cdn/shop/files/M_HERO_BANNER_7_1_07a2db7a-f369-4776-b98…" 
  data-srcset="//rovelab.au/cdn/shop/files/M_HERO_BANNER_7_1_07a2db7a-f369-4776-b989-f1c7…" 
  fetchpriority="high" 
  alt="" 
  style="object-position: 50.0% 50.0%;" 
  sizes="412px" 
  srcset="//rovelab.au/cdn/shop/files/M_HERO_BANNER_7_1_07a2db7a-f369-4776-b989-f1c7…"
>

```

---

<br>

## 📊 Priority Table (Possible Actions)

| Priority | Task                                  | Expected Impact | Time Estimate       |
|----------|---------------------------------------|-----------------|---------------------|
| 🔴 High  | Image optimization (Items 2 and 4)    | Very high       | 3–4 hours           |
| 🔴 High  | LCP request discovery (Item 5)        | High            | 30 min – 1 hour     |
| 🟠 Medium| Script removal/deferral (Item 1)      | Medium          | 30 min – 1 hour     |
| 🟡 Low   | Lazy loading for images (Item 3)      | Low             | 30 min – 1 hour     |

---

<br>

## ⚠️ Items Related to External Apps (not adjustable)

These items were reported by PageSpeed, but **cannot be directly optimized**, since the files come from third-party apps.  
The only alternative would be to **remove or replace the apps**, if desired.  

### Short cache duration for external resources  
- Intelligems – 180 KiB (cache 5 min)  
- Facebook Pixel – 117 KiB (cache 0–20 min)  
- Widde – 87 KiB (cache 1 hour)  
- Loox (reviews) – 214 KiB (cache 1 day 12 h)  
- 9gtb (external script) – 58 KiB (cache 1 day)  

---

### External fonts without `font-display: swap`  
- Rebuy (`fa-light-300.woff2`) – ~110 ms delay  

---

### Legacy JavaScript (77 KiB)  
- Klaviyo – 17.8 KiB  
- Rebuy – 58.7 KiB  
- Facebook Pixel – 12.5 KiB  
- TikTok – 17.5 KiB  

---

### Unused JavaScript (1.43 MB)  
- Google Tag Manager – 533.8 KiB  
- Rebuy – 367.0 KiB  
- TikTok – 199.1 KiB  
- Loox – 171.2 KiB  

---

### Unused CSS (28 KiB)  
- Rebuy – 19.5 KiB  
- app.css (store theme) – 13.2 KiB  

<br>

---

<br><br>

# 📌 Conclusion
A large part of the reported issues are related to **external apps**, which cannot be adjusted within Shopify.  
The main performance improvements will come from **image optimizations, layout stability fixes (CLS), and adjustments in scripts/initial loading**, which can be implemented by the development team.  
