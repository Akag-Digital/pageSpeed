# 📊 Performance Optimization Plan
**Date**: September 29, 2025  
**Analysis**: Google PageSpeed Insights  
**URL**: https://rovelab.com  
**Repository URL**: https://github.com/RoveLab/ecommerce-shopify-rove-us  
**PageSpeed Insights**: https://pagespeed.web.dev/analysis/https-rovelab-com/2v6pnjmpoh?form_factor=mobile  

---

<br>

This document gathers the key points identified in **PageSpeed** and divides them into two categories:

- **Actions that can be performed by the developer**  
- **Items related to external apps** (not directly adjustable in Shopify)  

<br>

---

<br>

## ✅ Developer Actions (Adjustable)

### 1. Render-blocking files
**The problem:** Some CSS and JS files delay the site’s initial render, increasing the time it takes to display the main content.  

**Identified files:**  
- `animations.min.js` – can be removed if not essential.  
- `multi-image.css` – can be optimized to load non-blocking.  
- `bundle.js` (Intelligems) – confirm if the app is still in use; if not, it can be removed.  

**Cannot be changed:** Shopify’s own files (`accelerated-checkout-backwards-compat.css` and `native-geo-redirects.min.css`).  

**Expected impact:** Reduced time for users to see initial content.  

---

### 2. Image optimization
**The problem:** Some images are larger than necessary in dimensions or format (**~607 KiB savings possible**).  

**Identified files:**  
1. `M_Hero_1180x1740_crop_center.jpg` – **169.5 KiB**  
2. `Corduroy_Velvet_Clay_Swatch_1.jpg` – **303.7 KiB**  
3. `Corduroy_Velvet_Silkstone_Swatch_1.jpg` – **284.7 KiB**  
4. `M_HERO_BANNER_8_1_1180x1740_crop_center.jpg` – **111.4 KiB**  
5. `SWATCH_PW_Krypton_Green-min.jpg` – **29.6 KiB**  
6. `SWATCH_PW_Platinum_Grey-min_1.jpg` – **29.0 KiB**  
7. `M1_2-Seat-Sofa-Loveseat_PW_Platinum_Grey_1…770x_crop_center.jpg`  
8. `SWATCH_PW_Silicon_Sand-min_1.jpg` – **26.8 KiB**  
9. `SWATCH_PW_Helium_Cloud-min_1.jpg` – **24.8 KiB**  
10. `SWATCH_PW_Hydrongen_Blue_2-min.jpg` – **20.1 KiB**  

**What can be done:**  
- Create smaller, responsive versions.  
- Convert to WebP or AVIF.  
- Apply optimized JPG compression.  

**Expected impact:** Significant reduction in total page weight and faster perceived load times.  

---

### 3. Offscreen images (lazy loading)
**The problem:** Some small preview images load even though they are outside the initial viewport.  

**Identified files:**  
- [ba917f7….thumbnail.000…_690x.jpg](https://rovelab.com/cdn/shop/files/preview_images/ba917f75f7b74f298c20755bea863f83.thumbnail.0000000000_690x.jpg?v=1749666405) – 7.5 KiB  
- [f447ae2….thumbnail.000…_690x.jpg](https://rovelab.com/cdn/shop/files/preview_images/f447ae2b31624113b77e55f4c8761400.thumbnail.0000000000_690x.jpg?v=1749666235) – 2.5 KiB  

**What can be done:**  
- Enable `loading="lazy"`.  

**Expected impact:** Minimal (~10 KiB), but avoids unnecessary downloads during initial load.  

---

### 4. Large payload size (12.47 MB)
**The problem:** The site loads **12.47 MB of resources**, much of it heavy videos and images.  

**Identified files:**  
1. Video – [ba917f75f7b74f298c20755bea863f83.HD-1080p-3.3Mbps-49220334.mp4](https://rovelab.com/cdn/shop/videos/c/vp/ba917f75f7b74f298c20755bea863f83/ba917f75f7b74f298c20755bea863f83.HD-1080p-3.3Mbps-49220334.mp4?v=0) (**3,629.5 KiB**)  
2. Video – [f447ae2b31624113b77e55f4c8761400.HD-1080p-3.3Mbps-49220206.mp4](https://rovelab.com/cdn/shop/videos/c/vp/f447ae2b31624113b77e55f4c8761400/f447ae2b31624113b77e55f4c8761400.HD-1080p-3.3Mbps-49220206.mp4?v=0) (**2,935.0 KiB**)  
3. Image – [M_HERO_BANNER_5_2_1_1180x1740_crop_center.jpg](https://rovelab.com/cdn/shop/files/M_HERO_BANNER_5_2_1_1180x1740_crop_center.jpg?v=1758744258) (**147.2 KiB**)  

**What can be done:**  
- Compress or replace the videos with lighter/optimized versions.  
- Reassess whether 1080p videos are necessary for initial load.  
- Optimize banner image into modern formats (WebP/AVIF) and responsive sizes.  

**Expected impact:** Significant reduction in total page size.  

---

### 5. LCP request discovery
**The problem:**  
The main LCP (Largest Contentful Paint) image is correctly set with `fetchpriority="high"` and `loading="eager"`. However, the report suggests that detection could be further optimized so the browser recognizes the image as critical even earlier. This can be improved by adding *preload* to the image block in Liquid.  

**Identified element:**

```html
<img 
  loading="eager"
  class="single-image--mobile lazyautosizes ls-is-cached lazyloaded"
  width="1180" 
  height="1740" 
  data-sizes="auto" 
  src="https://rovelab.com/cdn/shop/files/M_HERO_BANNER_5_2_1_1180x1740_crop_center.jpg?v=1758744258"
  fetchpriority="high"
  alt=""
  style="object-position: 50.0% 50.0%;"
  sizes="412px"
  srcset="//rovelab.com/cdn/shop/files/M_HERO_BANNER_5_2_1_1180x1740_crop_center.jpg?v=1758744258"
/>

```

---

<br>

## 📊 Priority Table (Possible Actions)

| Priority | Task                                | Expected Impact | Time Estimate       |
|----------|-------------------------------------|-----------------|---------------------|
| 🔴 High  | Image and video optimization (Items 2 and 4) | Very high       | 3–4 hours           |
| 🔴 High  | LCP request discovery (Item 5)      | High            | 30 min – 1 hour     |
| 🟠 Medium| Script removal/deferral (Item 1)    | Medium          | 30 min – 1 hour     |
| 🟡 Low   | Lazy loading for images (Item 3)    | Low             | 30 min – 1 hour     |

---

<br>

## ⚠️ Items related to external apps (not adjustable)

These items were flagged by PageSpeed but **cannot be optimized directly**, since the files come from third-party apps. The only alternative would be to **remove or replace the apps**, if desired.  

- **External resource caching**  
  - Intelligems (180 KiB – cache 5 min)  
  - Facebook Pixel (117 KiB – cache 0–20 min)  
  - Loox (214 KiB – cache 1d12h)  
  - Snapchat Analytics (24 KiB – cache 10 min)  

- **Legacy JavaScript**  
  - Klaviyo (17.8 KiB)  
  - Rebuy (58.7 KiB)  
  - Facebook Pixel (12.5 KiB)  
  - TikTok (17.5 KiB)  

- **Unused JavaScript**  
  - Google Tag Manager (715.1 KiB / savings: 359 KiB)  
  - Rebuy (378.6 KiB / savings: 263 KiB)  
  - TikTok (199.4 KiB / savings: 126 KiB)  
  - Loox (147.5 KiB / savings: 111.6 KiB)  

- **Unused CSS**  
  - Rebuy (19.5 KiB / savings: 17.1 KiB)  
  - app.css (13.2 KiB / savings: 10.8 KiB)  

- **External fonts loaded by apps**  
  - Rebuy (`fa-light-300.woff2`) without `font-display: swap`.  

<br>

---

<br><br>

# 📌 Conclusion
Most of the flagged issues are related to **external apps**, which cannot be adjusted within Shopify.  
The main performance improvements will come from **image optimizations and adjustments to scripts/initial loading**, which can be handled by the development team.  


