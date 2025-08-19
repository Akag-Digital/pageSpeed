# 📊 Plano de Otimização de Performance
**Data**: Agosto 2025  
**Análise**: Google PageSpeed Insights  
**URL**: https://rovelab.com  
**URL Repositorio**: https://github.com/RoveLab/ecommerce-shopify-rove-us  
**PageSpeed Insights**: https://pagespeed.web.dev/analysis/https-rovelab-com/fahnrilula?hl=pt&form_factor=desktop 


## 📑 Índice de Otimizações

### Performance
1. [🖼️ Melhorar a Entrega de Imagens](#1--melhorar-a-entrega-de-imagens-economia-2113-kib)
2. [🔤 Exibição de Fontes](#2--exibição-de-fontes-economia-40ms)
3. [🚫 Renderizar Solicitações de Bloqueio](#3--renderizar-solicitações-de-bloqueio-economia-40ms)
4. [📐 Causas da Troca de Layout](#4--causas-da-troca-de-layout-cls-0150)
5. [🔄 Reflow Forçado](#5--reflow-forçado-251ms-total)
6. [🎯 Descoberta de Solicitações de LCP](#6--descoberta-de-solicitações-de-lcp)
7. [🌐 Árvore de Dependência da Rede](#7--árvore-de-dependência-da-rede-7540ms)
8. [⚡ Reduzir Tempo de Execução JavaScript](#8--reduzir-tempo-de-execução-javascript-30s-economia)
9. [🗑️ Reduzir JavaScript Não Usado](#9-️-reduzir-javascript-não-usado-863-kib)

### Práticas Recomendadas
10. [🍪 Usar Cookies de Terceiros](#10--usar-cookies-de-terceiros)
11. [❌ Erros do Navegador no Console](#11--erros-do-navegador-no-console)
12. [⚠️ Issues do Chrome DevTools](#12-️-issues-do-chrome-devtools)
13. [📐 Imagens com Proporção Incorreta](#13--imagens-com-proporção-incorreta)

---

## 📈 Métricas Atuais
- **Performance Score**: Necessita melhoria urgente
- **Economia Potencial**: ~2.113 KiB em imagens + 863 KiB em JavaScript
- **Tempo de Bloqueio**: ~40ms em requisições render-blocking
- **Layout Shift**: 0.150 CLS
- **Reflow Forçado**: 251ms total
- **Caminho Crítico**: 7.540ms
- **Erros no Console**: Múltiplos erros (ReferenceError, SyntaxError, Failed Resources)
- **Cookies de Terceiros**: 1 (Snapchat - será bloqueado)
- **Imagens Distorcidas**: 1 banner principal com proporção incorreta

---

## 1. 🖼️ Melhorar a Entrega de Imagens (Economia: 2.113 KiB)

### Problemas Identificados:
1. **Imagem CloudFront** (1.880,7 KiB de economia)
   - `images/b2140c73-b4d4-4ae2-ad71-a1f8ac9fb0e0.gif`
   - Usar formato de vídeo (MP4/WebM) em vez de GIF animado

2. **Imagens de Produtos Rovelab** (múltiplas economias)
   - `files/D_BANNER_EG-1_2_1_2800x1200_crop_center.jpg` - 100,8 KiB economia
   - Várias imagens `SWATCH_PW` podem ser compactadas (10-20 KiB cada)

### ✅ Correções Necessárias:

#### A. Converter GIF para Vídeo
```liquid
<!-- ARQUIVO: sections/single-image.liquid -->
<!-- Procurar por imagens GIF animadas -->

<!-- ANTES -->
<img loading="eager" class="single-image--desktop lazyautosizes ls-is-cached lazyloaded" 
     width="2800" height="1200" data-sizes="auto" 
     src="https://rovelab.com/cdn/shop/files/D_BANNER_EG-1_2_1_2800x1200_crop_center.jpg">

<!-- DEPOIS -->
{%- assign file_extension = featured_media | split: '.' | last | downcase -%}
{%- if file_extension == 'gif' -%}
  <video autoplay loop muted playsinline loading="lazy" 
         width="2800" height="1200" 
         style="object-position: 50.0% 50.0%;">
    <source src="{{ featured_media | replace: '.gif', '.mp4' }}" type="video/mp4">
    <source src="{{ featured_media | replace: '.gif', '.webm' }}" type="video/webm">
    <img src="{{ featured_media }}" alt="{{ alt | escape }}" loading="lazy">
  </video>
{%- else -%}
  <img loading="eager" class="single-image--desktop lazyautosizes" 
       width="2800" height="1200" 
       src="{{ featured_media }}" 
       alt="{{ alt | escape }}">
{%- endif -%}
```

#### B. Implementar Responsive Images com Sizes Corretos
```liquid
<!-- ARQUIVO: snippets/responsive-image.liquid -->
<!-- Melhorar o sistema de imagens responsivas -->

<img 
  srcset="{{ image | img_url: '375x' }} 375w,
          {{ image | img_url: '750x' }} 750w,
          {{ image | img_url: '1125x' }} 1125w,
          {{ image | img_url: '1500x' }} 1500w,
          {{ image | img_url: '2800x' }} 2800w"
  sizes="(max-width: 768px) 100vw, 
         (max-width: 1200px) 50vw, 
         1335px"
  loading="lazy"
  decoding="async"
  fetchpriority="{{ priority | default: 'auto' }}"
  alt="{{ image.alt | escape }}">
```

#### C. Otimizar Imagens de Produtos (Swatches)
```css
/* ARQUIVO: assets/product.css */
/* Otimizar tamanho das imagens swatch */

.product-card-swatch img {
  width: 30px;
  height: 30px;
  object-fit: cover;
  border-radius: 50%;
}

/* Usar WebP quando suportado */
.webp .product-card-swatch img {
  content: url({{ image | img_url: '60x60' | replace: '.jpg', '.webp' | replace: '.png', '.webp' }});
}
```

---

## 2. 🔤 Exibição de Fontes (Economia: 40ms)

### Problema:
- Fonte `fa-light-300.woff2` do Rebuy Engine causando delay de 40ms
- Font-display não otimizado

### ✅ Correção:
```html
<!-- ARQUIVO: layout/theme.liquid -->
<!-- Adicionar no <head> -->

<link rel="preconnect" href="https://rebuyengine.com">
<link rel="dns-prefetch" href="https://rebuyengine.com">

<!-- Otimizar carregamento de fontes -->
<style>
  @font-face {
    font-family: 'FA Light';
    src: url('https://cdn.rebuyengine.com/webfonts/fa-light-300.woff2') format('woff2');
    font-display: swap; /* Critical for performance */
    font-weight: 300;
    font-style: normal;
  }
  
  /* Usar font-display swap para todas as fontes */
  @font-face {
    font-family: 'Urbanist';
    src: url('{{ "Urbanist-Medium.woff2" | asset_url }}') format('woff2');
    font-display: swap;
    font-weight: 500;
  }
</style>
```

---

## 3. 🚫 Renderizar Solicitações de Bloqueio (Economia: 40ms)

### Recursos Bloqueantes:
1. `single-image.css` (1,6 KiB, 150ms)
2. `accelerated-checkout-backwards-compat.css` (2,8 KiB, 150ms)
3. Scripts de terceiros (Shopify hosting, 460ms)

### ✅ Correções:

#### A. Defer CSS Não-Crítico
```liquid
<!-- ARQUIVO: layout/theme.liquid -->
<!-- Carregar CSS não-crítico de forma assíncrona -->

<!-- Critical CSS inline -->
<style>
  /* Critical CSS - apenas above-the-fold */
  .shopify-section { display: block; }
  .single-image { width: 100%; height: auto; }
  .product-card { display: block; min-height: 300px; }
</style>

<!-- CSS não-crítico async -->
<link rel="preload" href="{{ 'single-image.css' | asset_url }}" as="style" onload="this.onload=null;this.rel='stylesheet'">
<noscript><link rel="stylesheet" href="{{ 'single-image.css' | asset_url }}"></noscript>

<link rel="preload" href="{{ 'accelerated-checkout-backwards-compat.css' | asset_url }}" as="style" onload="this.onload=null;this.rel='stylesheet'">
<noscript><link rel="stylesheet" href="{{ 'accelerated-checkout-backwards-compat.css' | asset_url }}"></noscript>
```

#### B. Preconnect para Recursos Críticos
```html
<!-- ARQUIVO: layout/theme.liquid -->
<!-- Adicionar no início do <head> -->

<link rel="preconnect" href="https://cdn.shopify.com" crossorigin>
<link rel="preconnect" href="https://rovelab.com" crossorigin>
<link rel="dns-prefetch" href="https://rebuyengine.com">
```

---

## 4. 📐 Causas da Troca de Layout (CLS: 0.150)

### Elementos com Shift:
1. **Seção Featured Collection** (0.121)
   - `shopify-section-template--18667416879241__featured_collection_WtGPrd`
2. **Product Card Info** (0.020) 
   - `product-card-info` do M1 SOFA
3. **Imagens sem dimensões** (Unsized image elements)

### ✅ Correções:

#### A. Definir Aspect-Ratio e Dimensões
```css
/* ARQUIVO: assets/featured-collection.css */
/* Prevenir layout shift em product cards */

.product-card {
  min-height: 400px; /* Reservar espaço */
}

.product-card-info {
  min-height: 80px; /* Espaço para título e preço */
}

.product-primary-image {
  aspect-ratio: 1 / 1;
  width: 100%;
  height: auto;
  object-fit: cover;
}

/* Para imagens de banner */
.single-image img {
  aspect-ratio: 2800 / 1200; /* Proporção do banner */
  width: 100%;
  height: auto;
}

@media (max-width: 768px) {
  .single-image img {
    aspect-ratio: 4 / 5; /* Proporção mobile */
  }
}
```

#### B. Reservar Espaço para Elementos Dinâmicos
```liquid
<!-- ARQUIVO: sections/featured-collection.liquid -->
<!-- Adicionar skeleton/placeholder -->

<div class="product-card" style="min-height: 400px;">
  <div class="product-image-wrapper" style="aspect-ratio: 1/1; background: #f5f5f5;">
    {% if product.featured_image %}
      <img src="{{ product.featured_image | img_url: '400x400' }}" 
           width="400" 
           height="400"
           loading="lazy" 
           alt="{{ product.title | escape }}">
    {% endif %}
  </div>
  
  <div class="product-card-info" style="min-height: 80px;">
    <h3>{{ product.title }}</h3>
    <div class="price">{{ product.price | money }}</div>
  </div>
</div>
```

---

## 5. 🔄 Reflow Forçado (251ms total)

### Scripts causando reflow:
1. `clarity.js` (69ms) - Microsoft Clarity
2. `animations.min.js` (não especificado, mas presente)
3. Múltiplos scripts causando recálculos de layout

### ✅ Correções:

#### A. Otimizar Microsoft Clarity
```javascript
// ARQUIVO: layout/theme.liquid
// Carregar Clarity de forma não-bloqueante

<script>
  // Carregar Clarity após interação ou timeout
  let clarityLoaded = false;
  
  const loadClarity = () => {
    if (!clarityLoaded && window.clarity) {
      clarityLoaded = true;
      (function(c,l,a,r,i,t,y){
        c[a]=c[a]||function(){(c[a].q=c[a].q||[]).push(arguments)};
        t=l.createElement(r);t.async=1;t.src="https://www.clarity.ms/tag/"+i;
        y=l.getElementsByTagName(r)[0];y.parentNode.insertBefore(t,y);
      })(window, document, "clarity", "script", "SEU_CLARITY_ID");
    }
  };
  
  // Carregar após 3 segundos ou primeira interação
  setTimeout(loadClarity, 3000);
  ['scroll', 'click', 'touchstart'].forEach(event => {
    document.addEventListener(event, loadClarity, { once: true, passive: true });
  });
</script>
```

#### B. Batch DOM Operations
```javascript
// ARQUIVO: assets/animations.min.js
// Usar requestAnimationFrame para batching

// ANTES (causa reflow)
elements.forEach(el => {
  el.style.transform = 'translateX(' + x + 'px)';
  el.style.opacity = opacity;
});

// DEPOIS (batched)
requestAnimationFrame(() => {
  elements.forEach(el => {
    el.style.cssText = `
      transform: translateX(${x}px);
      opacity: ${opacity};
    `;
  });
});
```

---

## 6. 🎯 Descoberta de Solicitações de LCP

### Problema:
- A solicitação é detectável no documento inicial
- Imagem LCP não está sendo carregada com prioridade adequada
- Elemento: `img.single-image--desktop`

### ✅ Correções:

```liquid
<!-- ARQUIVO: sections/single-image.liquid -->
<!-- Otimizar carregamento da imagem LCP -->

{% comment %} Identificar se é a primeira seção (LCP) {% endcomment %}
{% assign is_first_section = true %}
{% for section in sections %}
  {% if section.id == section.id %}
    {% if forloop.index > 1 %}
      {% assign is_first_section = false %}
    {% endif %}
    {% break %}
  {% endif %}
{% endfor %}

{% if section.settings.image %}
  {% if is_first_section %}
    <!-- Preload para LCP -->
    <link rel="preload" as="image" 
          href="{{ section.settings.image | img_url: '2800x1200' }}" 
          fetchpriority="high">
  {% endif %}
  
  <img 
    src="{{ section.settings.image | img_url: '2800x1200' }}"
    width="2800" 
    height="1200"
    {% if is_first_section %}
      fetchpriority="high"
      loading="eager"
    {% else %}
      loading="lazy"
    {% endif %}
    decoding="async"
    alt="{{ section.settings.image.alt | escape }}"
    class="single-image--desktop">
{% endif %}
```

---

## 7. 🌐 Árvore de Dependência da Rede (7.540ms)

### Caminho Crítico Identificado:
1. **HTML inicial** → 191ms
2. **Harmonia Sans fonts** → 381ms + 382ms (763ms total)
3. **Client Shop JS** → 197ms
4. **Chunk Common** → 268ms
5. **Accelerated Checkout CSS** → 214ms

### ✅ Correções:

#### A. Otimizar Carregamento de Fontes
```html
<!-- ARQUIVO: layout/theme.liquid -->
<!-- Preconnect e preload para fontes críticas -->

<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link rel="preconnect" href="https://rovelab.com" crossorigin>

<!-- Preload fontes críticas -->
<link rel="preload" 
      href="https://rovelab.com/cdn/fonts/harmonia_sans/harmoniasans_n4.95f20f5.woff2" 
      as="font" 
      type="font/woff2" 
      crossorigin>
<link rel="preload" 
      href="https://rovelab.com/cdn/fonts/harmonia_sans/harmoniasans_n6.27fa71e.woff2" 
      as="font" 
      type="font/woff2" 
      crossorigin>
```

#### B. Resource Hints Estratégicos
```liquid
<!-- ARQUIVO: layout/theme.liquid -->

{% comment %} Prefetch recursos que serão usados em breve {% endcomment %}
<link rel="prefetch" href="{{ 'product-card.js' | asset_url }}">
<link rel="prefetch" href="{{ 'cart-drawer.js' | asset_url }}">

{% comment %} DNS prefetch para domínios de terceiros {% endcomment %}
<link rel="dns-prefetch" href="https://www.googletagmanager.com">
<link rel="dns-prefetch" href="https://cdn.rebuyengine.com">
<link rel="dns-prefetch" href="https://static.klaviyo.com">
```

---

## 8. ⚡ Reduzir Tempo de Execução JavaScript (3.0s economia)

### Scripts pesados identificados:
1. **Rovelab próprio** - 2.112ms
2. **Animations.min.js** - 673ms  
3. **Vendor.min.js** - 140ms
4. **Google Tag Manager** - múltiplos scripts

### ✅ Correções:

#### A. Lazy Load Scripts Não-Críticos
```javascript
// ARQUIVO: layout/theme.liquid
// Implementar lazy loading inteligente

<script>
  // Carregar scripts apenas quando necessário
  const lazyLoadScript = (src, condition) => {
    if (condition()) {
      const script = document.createElement('script');
      script.src = src;
      script.async = true;
      document.head.appendChild(script);
    }
  };

  // Carregar animações apenas em desktop
  lazyLoadScript('{{ "animations.min.js" | asset_url }}', () => 
    window.innerWidth > 768
  );

  // Carregar vendor scripts após interação
  let interacted = false;
  ['scroll', 'click', 'touchstart'].forEach(event => {
    document.addEventListener(event, () => {
      if (!interacted) {
        interacted = true;
        lazyLoadScript('{{ "vendor.min.js" | asset_url }}', () => true);
      }
    }, { once: true, passive: true });
  });
</script>
```

#### B. Code Splitting para Scripts Próprios
```javascript
// ARQUIVO: assets/theme.js
// Dividir funcionalidades em módulos

// Carregar apenas funcionalidades necessárias por página
const loadPageSpecificScripts = () => {
  const template = document.body.getAttribute('data-template');
  
  switch(template) {
    case 'product':
      import('./product-specific.js');
      break;
    case 'collection':
      import('./collection-specific.js');
      break;
    case 'index':
      import('./homepage-specific.js');
      break;
  }
};

// Carregar após DOM ready
if (document.readyState === 'loading') {
  document.addEventListener('DOMContentLoaded', loadPageSpecificScripts);
} else {
  loadPageSpecificScripts();
}
```

---

## 9. 🗑️ Reduzir JavaScript Não Usado (863 KiB)

### Bibliotecas com código não usado:
1. **Rebuy Engine** - 366,1 KiB (251,8 KiB não usado)
2. **Google Tag Manager** - 390,3 KiB (156,1 KiB não usado)
3. **Loox.io** - 211,8 KiB (150,8 KiB não usado)
4. **Klaviyo** - 127,2 KiB (66,7 KiB não usado)

### ✅ Correções:

#### A. Conditional Loading por Template
```liquid
<!-- ARQUIVO: layout/theme.liquid -->

{% comment %} Carregar Rebuy apenas em páginas de produto e carrinho {% endcomment %}
{% if template contains 'product' or template contains 'cart' %}
  <script src="https://rebuyengine.com/js/global.js?shop=rovelab-us.myshopify.com" defer></script>
{% endif %}

{% comment %} Loox apenas em páginas de produto {% endcomment %}
{% if template contains 'product' %}
  <script>
    // Carregar Loox após interação
    const loadLoox = () => {
      if (!window.looxLoaded) {
        window.looxLoaded = true;
        const script = document.createElement('script');
        script.src = 'https://loox.io/widget/...';
        script.defer = true;
        document.head.appendChild(script);
      }
    };
    
    ['scroll', 'click'].forEach(event => {
      document.addEventListener(event, loadLoox, { once: true });
    });
    
    // Fallback após 5 segundos
    setTimeout(loadLoox, 5000);
  </script>
{% endif %}
```

#### B. Otimizar Google Tag Manager
```javascript
// ARQUIVO: layout/theme.liquid
// GTM otimizado com consentimento

<script>
  // Carregar GTM apenas após consentimento de cookies
  const loadGTM = () => {
    if (window.cookieConsent?.analytics && !window.gtmLoaded) {
      window.gtmLoaded = true;
      
      (function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
      new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
      j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
      'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
      })(window,document,'script','dataLayer','GTM-N8TWDZN6');
    }
  };
  
  // Aguardar consentimento ou timeout
  window.addEventListener('cookieconsent:accepted', loadGTM);
  setTimeout(loadGTM, 3000); // Fallback
</script>
```

#### C. Tree Shaking para Módulos Próprios
```javascript
// webpack.config.js ou build process
// Configurar tree shaking adequado

module.exports = {
  mode: 'production',
  optimization: {
    usedExports: true,
    sideEffects: false,
    minimize: true,
    splitChunks: {
      chunks: 'all',
      cacheGroups: {
        vendor: {
          test: /[\\/]node_modules[\\/]/,
          name: 'vendors',
          chunks: 'all',
        },
        common: {
          name: 'common',
          minChunks: 2,
          chunks: 'all',
          enforce: true
        }
      }
    }
  }
};
```

---

## 10. 🍪 Usar Cookies de Terceiros

### Problema Identificado:
- **Snapchat Analytics** (`sc_at`) usando cookies de terceiros
- Chrome irá bloquear cookies de terceiros em breve

### ✅ Correções:

#### A. Implementar First-Party Cookie Proxy
```javascript
// ARQUIVO: layout/theme.liquid
// Migrar Snapchat para first-party

<script>
  // Implementar proxy first-party para Snapchat
  const trackSnapchatEvent = (eventType, data = {}) => {
    fetch('/api/analytics/snapchat', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        event: eventType,
        data: {
          ...data,
          url: window.location.href,
          referrer: document.referrer,
          timestamp: Date.now()
        }
      }),
      credentials: 'same-origin' // Usa cookies first-party
    }).catch(err => console.warn('Snapchat tracking failed:', err));
  };

  // Substituir chamadas diretas do Snapchat
  trackSnapchatEvent('PageView');
</script>
```

#### B. Implementar Consent Management
```liquid
<!-- ARQUIVO: layout/theme.liquid -->
<!-- Sistema de consentimento completo -->

<script>
  // Sistema de consentimento de cookies
  class CookieConsent {
    constructor() {
      this.consent = this.getConsent();
      this.init();
    }
    
    getConsent() {
      const consent = localStorage.getItem('cookie-consent');
      return consent ? JSON.parse(consent) : null;
    }
    
    setConsent(preferences) {
      this.consent = preferences;
      localStorage.setItem('cookie-consent', JSON.stringify(preferences));
      this.loadScriptsBasedOnConsent();
    }
    
    loadScriptsBasedOnConsent() {
      if (this.consent?.analytics) {
        this.loadAnalytics();
      }
      if (this.consent?.marketing) {
        this.loadMarketing();
      }
    }
    
    loadAnalytics() {
      // Carregar scripts de analytics
      if (!window.gtmLoaded) {
        // Carregar GTM
      }
    }
    
    loadMarketing() {
      // Carregar scripts de marketing
      if (!window.snapchatLoaded) {
        // Carregar Snapchat
      }
    }
  }
  
  window.cookieConsent = new CookieConsent();
</script>
```

---

## 11. ❌ Erros do Navegador no Console

### Erros Identificados:
1. **Gorgias Chat** - `Failed to load resource: 403 (Forbidden)`
2. **Shop.app** - `Failed to load resource: 403 (Forbidden)` 
3. **Rovelab** - `ReferenceError: products is not defined`
4. **SyntaxError** - `Invalid or unexpected token`
5. **TypeError** - `Cannot convert undefined or null to object`

### ✅ Correções:

#### A. Corrigir ReferenceError de Products
```liquid
<!-- ARQUIVO: Procurar por uso de variável 'products' -->
<!-- Provavelmente em sections/featured-collection.liquid ou similar -->

<!-- ANTES (erro) -->
<script>
  const productData = {{ products }};
</script>

<!-- DEPOIS (corrigido) -->
<script>
  {% assign products_json = section.settings.collection.products | json %}
  const productData = {{ products_json }};
  
  // Sempre validar dados antes de usar
  if (productData && Array.isArray(productData)) {
    // Seu código aqui
  }
</script>
```

#### B. Handle Failed Resources Gracefully
```javascript
// ARQUIVO: assets/theme.js
// Adicionar error handling global

// Gorgias Chat fallback
window.addEventListener('error', function(e) {
  if (e.target.src && e.target.src.includes('gorgias.chat')) {
    console.warn('Gorgias chat failed to load');
    e.target.remove();
    
    // Mostrar fallback de contato
    const fallbackContact = document.querySelector('.contact-fallback');
    if (fallbackContact) {
      fallbackContact.style.display = 'block';
    }
  }
  
  // Shop.app fallback
  if (e.target.src && e.target.src.includes('shop.app')) {
    console.warn('Shop.app failed to load');
    e.target.remove();
  }
}, true);

// Universal script error handling
window.addEventListener('unhandledrejection', function(e) {
  console.warn('Promise rejection:', e.reason);
  // Não deixar que erros quebrem a experiência
  e.preventDefault();
});
```

#### C. Corrigir SyntaxError em Universal Script
```liquid
<!-- ARQUIVO: Procurar por scripts que geram linha com erro -->
<!-- Provavelmente em algum template que usa dados Liquid -->

<!-- ANTES (provável erro) -->
<script>
  const metaData = {{ product.metafields.custom.data }};
  const config = {{ settings.custom_config }};
</script>

<!-- DEPOIS (corrigido) -->
<script>
  const metaData = {{ product.metafields.custom.data | default: '{}' | json }};
  const config = {{ settings.custom_config | default: '{}' | json }};
  
  // Validar dados
  try {
    if (typeof metaData === 'object' && metaData !== null) {
      // Processar metaData
    }
  } catch (error) {
    console.warn('Error processing metadata:', error);
  }
</script>
```

---

## 12. ⚠️ Issues do Chrome DevTools

### Problema:
- Cookie de terceiros do Snapchat será bloqueado pelo Chrome
- Preparação necessária para mudanças de privacidade

### ✅ Correções:

#### A. Implementar SameSite Cookie Policy
```javascript
// ARQUIVO: Configuração de cookies
// Migrar todos os cookies para SameSite

// Função helper para definir cookies seguros
const setSecureCookie = (name, value, options = {}) => {
  const defaults = {
    SameSite: 'Lax',
    Secure: true,
    Path: '/'
  };
  
  const cookieOptions = { ...defaults, ...options };
  const optionsString = Object.entries(cookieOptions)
    .map(([key, value]) => `${key}=${value}`)
    .join('; ');
  
  document.cookie = `${name}=${value}; ${optionsString}`;
};

// Migrar cookies existentes
setSecureCookie('visitor_id', generateVisitorId());
setSecureCookie('session_id', generateSessionId(), { SameSite: 'Strict' });
```

#### B. Alternativas sem Cookies para Tracking
```javascript
// ARQUIVO: assets/analytics.js
// Sistema de tracking sem cookies

class PrivacyFriendlyAnalytics {
  constructor() {
    this.sessionId = this.generateSessionId();
    this.visitorId = this.getOrCreateVisitorId();
  }
  
  generateSessionId() {
    return crypto.randomUUID();
  }
  
  getOrCreateVisitorId() {
    let visitorId = localStorage.getItem('visitor_id');
    
    if (!visitorId) {
      // Gerar fingerprint baseado em características do navegador
      const fingerprint = this.generateFingerprint();
      visitorId = btoa(fingerprint).replace(/[^a-zA-Z0-9]/g, '').substring(0, 16);
      localStorage.setItem('visitor_id', visitorId);
    }
    
    return visitorId;
  }
  
  generateFingerprint() {
    const canvas = document.createElement('canvas');
    const ctx = canvas.getContext('2d');
    ctx.textBaseline = 'top';
    ctx.font = '14px Arial';
    ctx.fillText('Browser fingerprint', 2, 2);
    
    return [
      navigator.userAgent,
      navigator.language,
      screen.width + 'x' + screen.height,
      new Date().getTimezoneOffset(),
      canvas.toDataURL()
    ].join('|');
  }
  
  track(event, data = {}) {
    fetch('/api/analytics/track', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'X-Session-ID': this.sessionId,
        'X-Visitor-ID': this.visitorId
      },
      body: JSON.stringify({
        event,
        data,
        timestamp: Date.now(),
        url: window.location.href
      })
    }).catch(err => console.warn('Analytics failed:', err));
  }
}

window.analytics = new PrivacyFriendlyAnalytics();
</script>
```

---

## 13. 📐 Imagens com Proporção Incorreta

### Problema Identificado:
- Banner principal com proporção incorreta
- **Exibido**: 1180 x 1740 (ratio 0.68)
- **Real**: 20 x 29 (ratio 0.69)
- Causa layout shift e distorção visual

### ✅ Correções:

#### A. Definir Aspect Ratio Correto
```css
/* ARQUIVO: assets/single-image.css */
/* Corrigir proporções das imagens */

.single-image img {
  aspect-ratio: 20 / 29; /* Proporção real da imagem */
  width: 100%;
  height: auto;
  object-fit: cover; /* Previne distorção */
}

/* Ajustar para diferentes breakpoints */
@media (min-width: 768px) {
  .single-image img {
    aspect-ratio: 1180 / 1740; /* Desktop */
  }
}

@media (max-width: 767px) {
  .single-image img {
    aspect-ratio: 3 / 4; /* Mobile mais adequado */
  }
}
```

#### B. Usar Picture Element com Proporções Otimizadas
```liquid
<!-- ARQUIVO: sections/single-image.liquid -->
<!-- Implementar picture element responsivo -->

<picture class="single-image">
  <!-- Mobile: proporção otimizada -->
  <source 
    media="(max-width: 767px)"
    srcset="{{ section.settings.image | img_url: '375x500' }} 375w,
            {{ section.settings.image | img_url: '750x1000' }} 750w"
    sizes="100vw">
  
  <!-- Tablet: proporção intermediária -->
  <source 
    media="(max-width: 1199px)"
    srcset="{{ section.settings.image | img_url: '768x1024' }} 768w,
            {{ section.settings.image | img_url: '1536x2048' }} 1536w"
    sizes="100vw">
  
  <!-- Desktop: proporção original -->
  <source 
    media="(min-width: 1200px)"
    srcset="{{ section.settings.image | img_url: '1180x1740' }} 1x,
            {{ section.settings.image | img_url: '2360x3480' }} 2x"
    sizes="1180px">
  
  <!-- Fallback -->
  <img 
    src="{{ section.settings.image | img_url: '1180x1740' }}"
    width="1180"
    height="1740"
    alt="{{ section.settings.image.alt | escape }}"
    loading="eager"
    fetchpriority="high"
    decoding="async">
</picture>
```

#### C. Implementar Validação de Proporções
```javascript
// ARQUIVO: assets/image-validator.js
// Script para desenvolvimento - validar proporções

{% if request.host contains 'localhost' or request.host contains '.myshopify.com' %}
<script>
  document.addEventListener('DOMContentLoaded', () => {
    const images = document.querySelectorAll('img');
    
    images.forEach(img => {
      img.addEventListener('load', function() {
        const displayed = {
          width: this.clientWidth,
          height: this.clientHeight
        };
        
        const natural = {
          width: this.naturalWidth,
          height: this.naturalHeight
        };
        
        const displayedRatio = displayed.width / displayed.height;
        const naturalRatio = natural.width / natural.height;
        const difference = Math.abs(displayedRatio - naturalRatio);
        
        // Tolerância de 5% para diferença de proporção
        if (difference > 0.05) {
          console.warn('🖼️ Proporção incorreta detectada:', {
            src: this.src.split('/').pop(),
            displayed: `${displayed.width}x${displayed.height} (${displayedRatio.toFixed(2)})`,
            natural: `${natural.width}x${natural.height} (${naturalRatio.toFixed(2)})`,
            difference: (difference * 100).toFixed(1) + '%'
          });
          
          // Destacar em desenvolvimento
          this.style.outline = '3px solid red';
          this.title = 'Proporção incorreta detectada';
        }
      });
    });
  });
</script>
{% endif %}
```

---

## 🎯 Prioridades de Implementação

### 🔴 Alta Prioridade (Impacto Imediato)
1. **Converter GIF para vídeo** - 1.8MB de economia
2. **Implementar font-display: swap** - 40ms de melhoria
3. **Corrigir erros de console** - Melhora estabilidade
4. **Defer CSS não-crítico** - 40ms de bloqueio reduzido
5. **Definir aspect-ratio para imagens** - Reduz CLS de 0.150

### 🟡 Média Prioridade
6. **Lazy load scripts não-críticos** - 3s de economia em JS
7. **Otimizar caminho crítico de rede** - 7.5s de melhoria
8. **Implementar consent management** - Preparar para mudanças do Chrome
9. **Remover JavaScript não usado** - 863 KiB de economia
10. **Corrigir proporções de imagens** - Melhora UX visual

### 🟢 Baixa Prioridade (Melhorias incrementais)
11. **Code splitting avançado**
12. **Implementar analytics sem cookies**
13. **Tree shaking otimizado**
14. **Monitoramento de erros (Sentry)**

---

## 📊 Resultados Esperados

Após implementação completa:
- **Redução de ~3MB** no peso total da página
- **Melhoria de ~8s** no caminho crítico de rede
- **CLS < 0.1** (excelente)
- **LCP < 2.5s** (bom)
- **Eliminação completa** de erros no console
- **Score Performance**: 80+ (verde)
- **Compliance** com mudanças de privacidade do Chrome

---

## 🚀 Quick Wins - Ações Imediatas

### Pode implementar AGORA (< 30 minutos):

1. **Font Display Swap**
   ```css
   /* layout/theme.liquid - adicionar no <head> */
   @font-face { font-display: swap; }
   ```

2. **Preconnect Domains**
   ```html
   <!-- layout/theme.liquid -->
   <link rel="preconnect" href="https://cdn.shopify.com">
   <link rel="preconnect" href="https://rovelab.com">
   ```

3. **Aspect Ratio CSS**
   ```css
   /* assets/single-image.css */
   .single-image img { aspect-ratio: 1180/1740; }
   ```

4. **Fix Console Errors**
   ```liquid
   <!-- Adicionar filter json em variáveis Liquid -->
   {{ product.metafields | default: '{}' | json }}
   ```

5. **Defer Non-Critical CSS**
   ```html
   <!-- layout/theme.liquid -->
   <link rel="preload" href="style.css" as="style" onload="this.rel='stylesheet'">
   ```

### Impacto Esperado das Quick Wins:
- **~1s** de melhoria no tempo de carregamento
- **Redução de 0.1 CLS** no layout shift
- **Eliminação de 5+ erros** no console
- **40ms** de economia no blocking time
- **Melhoria imediata** na experiência do usuário

---

## 🔧 Ferramentas Recomendadas

1. **Squoosh.app** - Otimização de imagens
2. **FFmpeg** - Conversão GIF → MP4/WebM  
3. **Chrome DevTools Coverage** - Identificar código não usado
4. **Lighthouse CI** - Monitoramento contínuo
5. **CookieYes/Cookiebot** - Gerenciamento de consentimento
6. **Sentry** - Monitoramento de erros JavaScript
7. **WebPageTest** - Análise detalhada de performance

---

## 📝 Notas de Implementação

- ✅ Sempre testar mudanças em ambiente de desenvolvimento primeiro
- ✅ Fazer backup antes de alterações críticas  
- ✅ Monitorar métricas após cada deploy
- ✅ Considerar A/B testing para mudanças significativas
- ✅ Implementar monitoramento contínuo de performance
- ✅ Documentar todas as mudanças para a equipe
