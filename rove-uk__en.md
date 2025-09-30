# 📊 Performance Optimization Plan

**Date**: September 30, 2025  
**Analysis**: Google PageSpeed Insights  
**URL**: https://rovelab.uk  
**Repository URL**: https://github.com/RoveLab/ecommerce-shopify-rove-uk  
**PageSpeed Insights**: https://pagespeed.web.dev/analysis/https-rovelab-uk/zqofduni06?form_factor=mobile  

<br>

---

<br>

This document gathers the main findings from **PageSpeed** and splits them into two categories:

- **Actions that can be taken by the developer**  
- **Items related to external apps** (no direct intervention possible in Shopify)

<br>

---

<br>

## ✅ Developer Actionable Items

### 1. Render-Blocking Files

**The issue:** Some CSS and JS files delay the initial display of the website, increasing the time to show the main content.  

**Identified files:**
- `animations.min.js` – can be removed if not essential.  
- `multi-image.css` – can be optimized to load non-blocking.  
- `bundle.js` (Intelligems) – confirm if the app is still in use; if not, it can be removed.  

**Cannot be changed:** Shopify files (`accelerated-checkout-backwards-compat.css` and `advanced-switcher.css`).  

**Expected impact:** Reduced perceived loading time for users.  

---

### 2. Image Optimization

**The issue:** Some images are larger in dimensions or format than necessary (**~300 KiB savings**).  

**Identified files:**
1. **SWATCH_PW_Krypton_Green** – 284.3 KiB → [link](https://rovelab.uk/cdn/shop/files/SWATCH_PW_Krypton_Green_1ee08ff4-7977-45e5-803b-65a42f458d69.jpg?v=1752525705)  
2. **SWATCH_PW_Silicon_Sand** – 261.0 KiB → [link](https://rovelab.uk/cdn/shop/files/SWATCH_PW_Silicon_Sand_5cb5a981-eca7-4610-bb8a-82efacbc99eb.jpg?v=1752525705)  
3. **M_HERO_BANNER_9_1_1180x1740_crop_center** – 104.6 KiB → [link](https://rovelab.uk/cdn/shop/files/M_HERO_BANNER_9_1_1180x1740_crop_center.jpg?v=1758661597)  
4. **SWATCH_PW_Helium_Cloud** – 234.1 KiB → [link](https://rovelab.uk/cdn/shop/files/SWATCH_PW_Helium_Cloud_924e0025-b7d2-4d80-8a6b-39d3105db749.jpg?v=1752525705)  
5. **m1-2-seat-sofa-loveseat-krypton-green-front-view** – 29.3 KiB → [link](https://rovelab.uk/cdn/shop/files/m1-2-seat-sofa-loveseat-krypton-green-front-view_f67ce228-c5a4-4e77-b46e-fd6d151a865a_770x_crop_center.webp?v=1749242136)  
6. **SWATCH_PW_Hydrongen_Blue** – 183.7 KiB → [link](https://rovelab.uk/cdn/shop/files/SWATCH_PW_Hydrongen_Blue_d45301aa-1f6d-4af1-ba51-1df7444f63d5.jpg?v=1752525705)  
7. **RoveLab_Logo_v.1_512_x_72_px** – 4.6 KiB → [link](https://cdn.shopify.com/s/files/1/0724/4041/4365/files/RoveLab_Logo_v.1_512_x_72_px_2_1309da4c-9d52-4b20-9def-47be80f23b51.png?v=1754584349)  

**What can be done:**
- Create smaller, responsive versions.  
- Convert to WebP or AVIF.  
- Apply optimized JPG compression.  

**Expected impact:** Significant reduction in total weight and noticeable improvement in load speed.  

---

### 3. Offscreen Images (Lazy Loading)

**The issue:** Some small preview images load even though they are outside the initial viewport.  

**Identified files:**
1. [c4e6644749ee4011a625aadf02a823ba.thumbnail.0000000000_690x.jpg](https://rovelab.uk/cdn/shop/files/preview_images/c4e6644749ee4011a625aadf02a823ba.thumbnail.0000000000_690x.jpg?v=1753473261) – 7.5 KiB  
2. [3d72ffe4441f44fcad05279720f76837.thumbnail.0000000000_690x.jpg](https://rovelab.uk/cdn/shop/files/preview_images/3d72ffe4441f44fcad05279720f76837.thumbnail.0000000000_690x.jpg?v=1756934874) – 2.5 KiB  

**What can be done:**
- Enable `loading="lazy"`.  

**Expected impact:** Potential savings of ~10 KiB.  

---

### 4. Excessive Payload Size (13.24 MB)

**The issue:** The site loads **13.24 MB of resources**, mostly large images and videos.  

**Identified files:**
1. **c4e6644….HD-1080p-7.2Mbps-52035919.mp4** – 9,044.9 KiB → [link](https://rovelab.uk/cdn/shop/videos/c/vp/c4e6644749ee4011a625aadf02a823ba/c4e6644749ee4011a625aadf02a823ba.HD-1080p-7.2Mbps-52035919.mp4?v=0)  
2. **3d72ffe….HD-1080p-3.3Mbps-56755690.mp4** – 2,935.5 KiB → [link](https://rovelab.uk/cdn/shop/videos/c/vp/3d72ffe4441f44fcad05279720f76837/3d72ffe4441f44fcad05279720f76837.HD-1080p-3.3Mbps-56755690.mp4?v=0)  
3. **SWATCH_PW_Krypton_Green.jpg** – 285.8 KiB → [link](https://rovelab.uk/cdn/shop/files/SWATCH_PW_Krypton_Green_1ee08ff4-7977-45e5-803b-65a42f458d69.jpg?v=1752525705)  
4. **SWATCH_PW_Silicon_Sand.jpg** – 262.4 KiB → [link](https://rovelab.uk/cdn/shop/files/SWATCH_PW_Silicon_Sand_5cb5a981-eca7-4610-bb8a-82efacbc99eb.jpg?v=1752525705)  
5. **SWATCH_PW_Helium_Cloud.jpg** – 235.6 KiB → [link](https://rovelab.uk/cdn/shop/files/SWATCH_PW_Helium_Cloud_924e0025-b7d2-4d80-8a6b-39d3105db749.jpg?v=1752525705)  
6. **SWATCH_PW_Hydrongen_Blue.jpg** – 185.1 KiB → [link](https://rovelab.uk/cdn/shop/files/SWATCH_PW_Hydrongen_Blue_d45301aa-1f6d-4af1-ba51-1df7444f63d5.jpg?v=1752525705)  
7. **m1-2-seat-sofa-loveseat-krypton-green-angled-view.webp** – 298.8 KiB → [link](https://cdn.shopify.com/s/files/1/0724/4041/4365/files/m1-2-seat-sofa-loveseat-krypton-green-angled-view_e67e01b1-ec07-4ba1-955b-e3e80892ccd7.webp?v=1749242136)  

**What can be done:**
- Compress images and videos.  
- Generate responsive versions.  
- Convert to WebP/AVIF.  

**Expected impact:** Significant reduction of total page weight.  

---

### 5. LCP Request Discovery

**The issue:** The main LCP (Largest Contentful Paint) image is already configured with `fetchpriority="high"` and `loading="eager"`. However, the report indicates detection could be further optimized to ensure the browser identifies this image as critical even earlier. Adding a *preload* in the Liquid template for this image can improve prioritization.  

**Identified element:** 


```html
<img 
  loading="eager" 
  class="single-image--mobile lazyautosizes lazyloaded" 
  width="1180" 
  height="1740" 
  data-sizes="auto" 
  src="https://rovelab.uk/cdn/shop/files/M_HERO_BANNER_9_1_1180x1740_crop_center.…" 
  data-srcset="//rovelab.uk/cdn/shop/files/M_HERO_BANNER_9_1_1180x1740_crop_center.jpg?v=…" 
  fetchpriority="high" 
  alt="" 
  style="object-position: 50.0% 50.0%;" 
  sizes="412px" 
  srcset="//rovelab.uk/cdn/shop/files/M_HERO_BANNER_9_1_1180x1740_crop_center.jpg?v=…"
>

```
---

### 6. Layout Stability (CLS)  
**The issue:** The report identified layout shifts during loading, mainly caused by images without reserved dimensions, the announcement marquee bar, and late loading of the **Poppins** font.  

**Identified elements/files:**  
- **Announcement bar** (`.announcement-bar--marquee`) – no fixed height defined, causing shifts when the animation starts.  
- **Hero image** (`M_HERO_BANNER_9_1_1180x1740_crop_center.jpg`) – flagged as an *unsized image element*.  
- **Featured products section** (`shopify-section-featured_collection`) – cards load without pre-allocated space.  
- **Redirects Icon** – image has `width/height` in HTML but is overridden by CSS.  
- **Poppins font (woff2)** – missing `font-display: swap`, causing font swap after load.  

**What can be done:**  
- Define width, height, or `aspect-ratio` for images (hero, icons, and products).  
- Add `font-display: swap` to external fonts.  
- Set a fixed height for the announcement bar.  
- Define minimum height for cards in the featured products section.  
- Adjust CSS to avoid overriding declared image dimensions.  

**Expected impact:** A more stable experience, without sudden layout shifts during loading, resulting in a lower **CLS** score and better usability.  

---

<br>

## 📊 Priority Table (Actionable Items)

| Priority | Task                                 | Expected Impact | Time Estimate      |
|----------|--------------------------------------|-----------------|--------------------|
| 🔴 High  | Image optimization (Items 2 and 4)   | Very high       | 4-5 hours          |
| 🔴 High  | LCP request discovery (Item 5)       | High            | 30 min – 1 hour    |
| 🔴 High  | Layout stability (CLS – Item 6)      | High            | 4–5 hours          |
| 🟠 Medium| Script removal/deferral (Item 1)     | Medium          | 30 min – 1 hour    |
| 🟡 Low   | Lazy loading on images (Item 3)      | Low             | 1-2 hours         |

---

<br>

## ⚠️ Items Related to External Apps (Not Adjustable)

These items were reported by PageSpeed but **cannot be optimized directly**, since the files come from third-party apps. The only alternative would be to **remove or replace apps**, if desired.  

### Short cache lifetime of external resources  
- Intelligems – 91 KiB (cache 5 min)  
- Facebook Pixel – 117 KiB (cache 0–20 min)  
- Widde – 87 KiB (cache 1 h)  
- Loox (reviews) – 214 KiB (cache 1 day 12 h)  
- 9gtb (external script) – 58 KiB (cache 1 day)  
- Clarity – 32 KiB (cache 1 day)  
- Klaviyo – 3 KiB (cache 1 s)  

---

### External fonts without `font-display: swap`  
- Rebuy (`fa-light-300.woff2`) – ~30 ms delay  

---

### Legacy JavaScript (79 KiB)  
- Klaviyo – 17.8 KiB  
- Rebuy – 48.4 KiB  
- Facebook Pixel – 12.5 KiB  

---

### Unused JavaScript (782 KiB)  
- Google Tag Manager – 407.6 KiB  
- Rebuy – 367.0 KiB  
- Converge – 41.8 KiB  
- Widde – 85.9 KiB  
- Facebook – 82.7 KiB  
- Loox – 171.2 KiB  
- Intelligems – 87.3 KiB  

---

### Unused CSS (27 KiB)  
- Rebuy – 19.5 KiB  
- app.css (theme) – 13.2 KiB  

<br>

---

<br><br>

# 📌 Conclusion
A large portion of the issues raised are related to **external apps**, with no possibility of direct adjustments in Shopify.  
The main performance improvements will come from **image optimization, layout stability (CLS) improvements, and script/loading adjustments**, which can be handled by the development team.  

