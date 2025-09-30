# 📊 Performance Optimization Plan  
**Date**: September 29, 2025  
**Analysis**: Google PageSpeed Insights  
**URL**: https://rovelab.ca  
**Repository URL**: https://github.com/RoveLab/ecommerce-shopify-rove-ca  
**PageSpeed Insights**: https://pagespeed.web.dev/analysis/https-rovelab-ca/t1kptem4hg?form_factor=mobile

<br>

---

<br>

This document summarizes the main points identified in **Lighthouse/PageSpeed** and organizes them into two categories:

- **Actions that can be taken by the developer**  
- **Items related to external apps** (no direct intervention possible in Shopify)

<br>

---

<br>

## ✅ Items that can be addressed (developer)


### 1. Render-blocking resources
**Issue:** Some CSS and JS files are delaying the initial render of the site, increasing the time for users to see the main content.  

**Files identified:**  
- `animations.min.js` – can be removed if not essential.  
- `multi-image.css` – can be optimized to load non-blocking.  
- `bundle.js` (Intelligems) – confirm if the app is still used; if not, it can be removed.  

**Not possible to change:** Shopify system files (`accelerated-checkout-backwards-compat.css` and `native-geo-redirects.min.css`).  

**Expected impact:** Faster perceived loading time for users.  

---

### 2. Image optimization
**Issue:** Some images are loaded in larger dimensions or formats than needed (**~607 KiB savings**).  

**Files identified:**  
1. `M_Hero_1180x1740_crop_center.jpg` – 169.5 KiB  
2. `Corduroy_Velvet_Clay_Swatch_1.jpg` – 303.7 KiB  
3. `Corduroy_Velvet_Silkstone_Swatch_1.jpg` – 284.7 KiB  
4. `M_HERO_BANNER_8_1_1180x1740_crop_center.jpg` – 111.4 KiB  
5. `SWATCH_PW_Krypton_Green-min.jpg` – 29.6 KiB  
6. `SWATCH_PW_Platinum_Grey-min_1.jpg` – 29.0 KiB  
7. `M1_2-Seat-Sofa-Loveseat_PW_Platinum_Grey_1…770x_crop_center.jpg` – 19.5 KiB  
8. `SWATCH_PW_Silicon_Sand-min_1.jpg` – 26.8 KiB  
9. `SWATCH_PW_Helium_Cloud-min_1.jpg` – 24.8 KiB  
10. `SWATCH_PW_Hydrongen_Blue_2-min.jpg` – 20.1 KiB  

**What can be done:**  
- Generate smaller responsive versions.  
- Convert to WebP or AVIF.  
- Apply optimized JPG compression.  

**Expected impact:** Significant weight reduction and noticeable improvement in load speed.  

---

### 3. Layout stability (CLS)
**Issue:** Layout shifts occur during loading due to images without fixed dimensions and font loading behavior.  

**Files identified:**  
- `M_Hero_1180x1740_crop_center.jpg` – missing fixed dimensions.  
- Featured products section (`shopify-section-featured_collection`).  
- **Poppins (woff2)** font.  

**What can be done:**  
- Define width/height on images.  
- Use `font-display: swap` for fonts.  
- Reserve space for featured sections.  

**Expected impact:** Improved visual stability and smoother navigation.  

---

### 4. Offscreen images (lazy loading)
**Issue:** Some preview images are loaded even though they are not visible on the initial viewport.  

**Files identified:**  
- [75b9d7d221f04e32a19dc387d6c8c754…_690x.jpg](https://rovelab.ca/cdn/shop/files/preview_images/75b9d7d221f04e32a19dc387d6c8c754.thumbnail.0000000000_690x.jpg?v=1725888914) – 7.5 KiB  
- [0c2f9128f931419292fcbea199437511…_690x.jpg](https://rovelab.ca/cdn/shop/files/preview_images/0c2f9128f931419292fcbea199437511.thumbnail.0000000000_690x.jpg?v=1725889262) – 2.3 KiB  

**What can be done:**  
- Enable `loading="lazy"`.  

**Expected impact:** Minimal (~10 KiB), but prevents unnecessary downloads on initial load.  

---

### 5. Excessive network payload (8.4 MB total)
**Issue:** The site loads **8.4 MB of resources**, mostly heavy images.  

**Files identified:**  
1. [Corduroy_Velvet_Clay_Swatch_1.jpg](https://rovelab.ca/cdn/shop/files/Corduroy_Velvet_Clay_Swatch_1.jpg?v=1753987241) – 304.7 KiB  
2. [Corduroy_Velvet_Silkstone_Swatch_1.jpg](https://rovelab.ca/cdn/shop/files/Corduroy_Velvet_Silkstone_Swatch_1.jpg?v=1753987224) – 285.7 KiB  
3. [D_Hero_3_2800x1200_crop_center.jpg](https://rovelab.ca/cdn/shop/files/D_Hero_3_2800x1200_crop_center.jpg?v=1758915004) – 265.8 KiB  
4. [D_HERO_BANNER_6_1_2800x1200_crop_center.jpg](https://rovelab.ca/cdn/shop/files/D_HERO_BANNER_6_1_83ae70aa-2bee-4e3d-96f7-f2ffad09d375_2800x1200_crop_center.jpg?v=1758585436) – 196.6 KiB  

**What can be done:**  
- Compress images.  
- Generate responsive versions.  
- Convert to WebP/AVIF.  

**Expected impact:** Significant page weight reduction.  

---

<br>

## 📊 Priority Table (Developer actions)

| Priority  | Task                                   | Expected Impact | Time Estimate       |
|-----------|----------------------------------------|-----------------|---------------------|
| 🔴 High   | Image optimization (Items 2 & 5)       | Very High       | 3–4 hours           |
| 🔴 High   | Layout stability (CLS – Item 3)        | High            | 2–3 hours           |
| 🟠 Medium | Script removal/deferral (Item 1)       | Medium          | 30 min – 1 hour     |
| 🟡 Low    | Lazy loading (Item 4)                  | Low             | 30 min – 1 hour     |

---

<br>

## ⚠️ Items related to external apps (not adjustable)

These items were reported by Lighthouse but **cannot be directly optimized** since they are controlled by third-party apps. The only alternatives are to **remove or replace the apps** or accept their impact.  

### Short cache lifetimes  
- Intelligems – 180 KiB (cache 5 min)  
- Facebook Pixel – 117 KiB (cache 0–20 min)  
- Loox (reviews) – 214 KiB (cache 1 day 12 h)  
- 9gtb (external script) – 58 KiB (cache 1 day)  
- Snapchat Analytics – 24 KiB (cache 10 min)  

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
- Google Tag Manager – 715.1 KiB  
- Rebuy – 378.6 KiB  
- TikTok – 199.4 KiB  
- Loox – 147.5 KiB  

---

### Unused CSS (28 KiB)  
- Rebuy – 19.5 KiB  
- app.css (store theme) – 13.2 KiB  

<br>

---

<br><br>

# 📌 Conclusion
A large portion of the issues identified are related to **external apps**, which cannot be directly optimized within Shopify.  
The main performance improvements will come from **image optimization, layout stability (CLS), and adjustments to scripts/render-blocking resources**, which can be addressed by the development team.  

