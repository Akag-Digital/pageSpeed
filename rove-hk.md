# 📊 Plano de Otimização de Performance
**Data**: Agosto 2025  
**Análise**: Google PageSpeed Insights  
**URL**: https://rovelab.hk  
**URL Repositorio**: https://github.com/Akag-Digital/rover-lab-hk  
**PageSpeed Insights**: https://pagespeed.web.dev/analysis/https-rovelab-hk/msvx92e3di?hl=pt&form_factor=desktop


## 📑 Índice de Otimizações

### Performance
1. [⚡ JavaScript Legado](#1--javascript-legado-economia-de-58-kib)
2. [🚫 Renderizar Solicitações de Bloqueio](#2--renderizar-solicitações-de-bloqueio-economia-de-40ms)
3. [🔄 Reflow Forçado](#3--reflow-forçado-economia-de-169ms)
4. [🎯 Descoberta de Solicitações de LCP](#4--descoberta-de-solicitações-de-lcp)
5. [🌐 Árvore de Dependência da Rede](#5--árvore-de-dependência-da-rede-6998ms-crítico)

### Acessibilidade
6. [🖼️ Elementos de Imagem sem Atributos Alt](#6--elementos-de-imagem-sem-atributos-alt)
7. [🔗 Links sem Nome Compreensível](#7--links-sem-nome-compreensível)
8. [📋 Elementos de Título Fora de Ordem](#8--elementos-de-título-fora-de-ordem-sequencial)

### Práticas Recomendadas
9. [📐 Imagens com Proporção Incorreta](#9--imagens-com-proporção-incorreta)
10. [❌ Erros do Navegador no Console](#10--erros-do-navegador-no-console)

---

## 📈 Métricas Atuais
- **Performance Score**: Necessita melhoria
- **JavaScript Legado**: 58 KiB (Klaviyo, Rebuy, Facebook)
- **Recursos Bloqueantes**: 40ms de economia potencial
- **Reflow Forçado**: 169ms total de tempo desperdiçado
- **LCP**: Imagem não otimizada detectável
- **Caminho Crítico**: 6.998ms de latência máxima
- **Erros no Console**: 2 erros principais (SyntaxError, Failed Resources)
- **Imagens sem Alt**: Múltiplas imagens de produtos
- **Links sem Nome**: 1 link problemático
- **Hierarquia de Títulos**: Ordem incorreta (H3 antes de H4)

---

## 1. ⚡ JavaScript Legado (Economia de 58 KiB)

### Problemas Identificados:
1. **Klaviyo** (17,8 KiB) - Polyfills desnecessários:
   - Array.prototype.flat
   - Array.prototype.flatMap
   - Array.prototype.sort
   - Object.fromEntries
   - Promise.allSettled
   - String.prototype.replaceAll

2. **Rebuy Engine** (28,0 KiB) - Polyfills desnecessários:
   - Array.prototype.findLastIndex
   - Array.prototype.unshift
   - Object.fromEntries
   - Object.hasOwn
   - Promise.allSettled
   - String.fromCodePoint

3. **Facebook** (12,5 KiB) - Polyfills desnecessários:
   - @babel/plugin-transform-classes
   - @babel/plugin-transform-spread
   - Array.from
   - Array.prototype.filter
   - Array.prototype.find

### ✅ Correções Necessárias:

#### A. Atualizar Target de Navegadores Modernos
```json
// ARQUIVO: package.json
// Atualizar browserslist para navegadores modernos

"browserslist": [
  "> 0.5%",
  "last 2 versions",
  "not dead",
  "not ie 11",
  "not op_mini all"
]
```

#### B. Configurar Babel para ES6+ sem Polyfills
```javascript
// ARQUIVO: babel.config.js ou .babelrc
{
  "presets": [
    ["@babel/preset-env", {
      "targets": {
        "browsers": ["> 0.5%", "last 2 versions", "not ie 11"]
      },
      "useBuiltIns": false, // Não incluir polyfills automáticos
      "modules": false
    }]
  ]
}
```

#### C. Conditional Loading para Apps Terceiros
```liquid
<!-- ARQUIVO: layout/theme.liquid -->
<!-- Carregar Klaviyo apenas quando necessário -->

{% if template contains 'product' or template contains 'cart' %}
  <script>
    // Detectar se navegador suporta recursos modernos
    if ('replaceAll' in String.prototype && 'flat' in Array.prototype) {
      // Carregar versão moderna sem polyfills
      const script = document.createElement('script');
      script.src = 'https://static.klaviyo.com/onsite/js/klaviyo.js?company_id=YOUR_ID';
      script.async = true;
      document.head.appendChild(script);
    }
  </script>
{% endif %}
```

#### D. Otimizar Rebuy Engine
```liquid
<!-- ARQUIVO: sections/product-recommendations.liquid -->
<!-- Carregar Rebuy apenas quando necessário -->

<script>
  // Lazy load Rebuy após interação do usuário
  let rebuyLoaded = false;
  
  const loadRebuy = () => {
    if (!rebuyLoaded && window.innerWidth > 768) {
      const script = document.createElement('script');
      script.src = 'https://rebuyengine.com/js/global.js';
      script.async = true;
      document.head.appendChild(script);
      rebuyLoaded = true;
    }
  };
  
  // Carregar após scroll ou após 3 segundos
  window.addEventListener('scroll', loadRebuy, { once: true, passive: true });
  setTimeout(loadRebuy, 3000);
</script>
```

---

## 2. 🚫 Renderizar Solicitações de Bloqueio (Economia de 40ms)

### Recursos Bloqueantes Identificados:
1. **rovelab.hk** (5,0 KiB - 300ms)
   - `announcement-bar.css` (2,2 KiB - 150ms)
   - `accelerated-checkout-backwards-compat.css` (2,8 KiB - 150ms)

2. **Shopify Hosting** (2,4 KiB - 230ms)
   - `advanced-switcher.css` (2,4 KiB - 230ms)

### ✅ Correções Necessárias:

#### A. Inline Critical CSS
```liquid
<!-- ARQUIVO: layout/theme.liquid -->
<!-- Mover CSS crítico para inline no <head> -->

<style>
  /* Critical CSS - Above the fold */
  .announcement-bar {
    background: var(--color-announcement-bar-bg);
    color: var(--color-announcement-bar-text);
    padding: 10px 0;
    text-align: center;
    font-size: calc(var(--font-body-scale) * 0.875rem);
  }
  
  .header {
    position: sticky;
    top: 0;
    z-index: 100;
    background: var(--color-header-bg);
  }
  
  /* Prevenir layout shift */
  .product-card {
    min-height: 400px;
  }
</style>

<!-- Carregar CSS não-crítico de forma assíncrona -->
<link rel="preload" href="{{ 'announcement-bar.css' | asset_url }}" as="style" onload="this.onload=null;this.rel='stylesheet'">
<noscript><link rel="stylesheet" href="{{ 'announcement-bar.css' | asset_url }}"></noscript>
```

#### B. Defer CSS Não-Crítico
```liquid
<!-- ARQUIVO: sections/announcement-bar.liquid -->
<!-- Remover stylesheet_tag e usar preload -->

<!-- ANTES -->
{{ 'announcement-bar.css' | asset_url | stylesheet_tag }}

<!-- DEPOIS -->
<link rel="preload" href="{{ 'announcement-bar.css' | asset_url }}" as="style" onload="this.onload=null;this.rel='stylesheet'">
<noscript><link rel="stylesheet" href="{{ 'announcement-bar.css' | asset_url }}"></noscript>
```

#### C. Otimizar Accelerated Checkout CSS
```liquid
<!-- ARQUIVO: layout/theme.liquid -->
<!-- Carregar apenas quando necessário -->

{% if template contains 'cart' or template contains 'product' %}
  <link rel="preload" href="{{ 'accelerated-checkout-backwards-compat.css' | asset_url }}" as="style" onload="this.onload=null;this.rel='stylesheet'">
{% endif %}
```

---

## 3. 🔄 Reflow Forçado (Economia de 169ms)

### Scripts Causando Reflow:
1. **Função Principal**: `assets/vendor.min.js` (5ms)
2. **Fontes**: Sem atribuição (50ms total)
3. **Múltiplos Assets**: header.js, animations.min.js, vendor.min.js, slideshow.js

### ✅ Correções Necessárias:

#### A. Otimizar Manipulação de DOM
```javascript
// ARQUIVO: assets/app.js
// Usar requestAnimationFrame para batch DOM updates

// ANTES - Causa reflow
element.style.height = newHeight + 'px';
element.style.width = newWidth + 'px';
element.style.opacity = newOpacity;

// DEPOIS - Batch updates
requestAnimationFrame(() => {
  element.style.cssText = `
    height: ${newHeight}px;
    width: ${newWidth}px;
    opacity: ${newOpacity};
  `;
});
```

#### B. Implementar Virtual Scrolling para Listas Longas
```javascript
// ARQUIVO: assets/product-recommendations.js
// Para listas de produtos longas

class VirtualScroller {
  constructor(container, items, itemHeight = 200) {
    this.container = container;
    this.items = items;
    this.itemHeight = itemHeight;
    this.visibleStart = 0;
    this.visibleEnd = Math.ceil(container.clientHeight / itemHeight) + 1;
    
    this.render();
  }
  
  render() {
    // Apenas renderizar itens visíveis
    const visibleItems = this.items.slice(this.visibleStart, this.visibleEnd);
    
    // Usar DocumentFragment para evitar reflows múltiplos
    const fragment = document.createDocumentFragment();
    
    visibleItems.forEach((item, index) => {
      const element = this.createItemElement(item);
      element.style.transform = `translateY(${(this.visibleStart + index) * this.itemHeight}px)`;
      fragment.appendChild(element);
    });
    
    this.container.innerHTML = '';
    this.container.appendChild(fragment);
  }
}
```

#### C. Debounce Resize Events
```javascript
// ARQUIVO: assets/global.js
// Evitar reflows excessivos no resize

let resizeTimeout;
window.addEventListener('resize', () => {
  if (resizeTimeout) {
    cancelAnimationFrame(resizeTimeout);
  }
  
  resizeTimeout = requestAnimationFrame(() => {
    // Batch todas as operações de resize
    document.querySelectorAll('.responsive-element').forEach(element => {
      element.style.width = window.innerWidth < 768 ? '100%' : '50%';
    });
  });
}, { passive: true });
```

---

## 4. 🎯 Descoberta de Solicitações de LCP

### Problema Identificado:
- **Imagem LCP**: `D_BANNER_DEFAULT_2_2800x1300_crop_center.jpg`
- **Fetchpriority**: Não aplicado
- **Lazy Loading**: Aplicado incorretamente ao LCP
- **Preload**: Ausente para elemento LCP

### ✅ Correções Necessárias:

#### A. Otimizar Elemento LCP
```liquid
<!-- ARQUIVO: sections/single-image.liquid -->
<!-- Identificar e otimizar imagem LCP -->

{% comment %} Preload da imagem LCP no head {% endcomment %}
{% if section.settings.image and section.index == 1 %}
  <link rel="preload" as="image" href="{{ section.settings.image | img_url: '2800x1300' }}" fetchpriority="high">
{% endif %}

<img 
  class="single-image--desktop lazyautosizes lazyloaded"
  {% if section.index == 1 %}
    fetchpriority="high"
    loading="eager"
  {% else %}
    loading="lazy"
  {% endif %}
  width="2800" 
  height="1300"
  data-sizes="auto"
  src="{{ section.settings.image | img_url: '2800x1300' }}"
  alt="{{ section.settings.image.alt | escape }}"
>
```

#### B. Preload no Head para LCP
```liquid
<!-- ARQUIVO: layout/theme.liquid -->
<!-- Adicionar preload condicional no head -->

{% for section in content_for_layout.sections %}
  {% if section.type == 'single-image' and forloop.first %}
    {% if section.settings.image %}
      <link rel="preload" as="image" href="{{ section.settings.image | img_url: '2800x1300' }}" fetchpriority="high">
    {% endif %}
  {% endif %}
{% endfor %}
```

#### C. Remover Lazy Loading do LCP
```liquid
<!-- ARQUIVO: snippets/responsive-image.liquid -->
<!-- Conditional loading baseado na posição -->

{% liquid
  assign is_lcp = false
  if priority == 'high' or position == 'hero' or section.index == 1
    assign is_lcp = true
  endif
%}

<img 
  {% if is_lcp %}
    loading="eager"
    fetchpriority="high"
  {% else %}
    loading="lazy"
    fetchpriority="auto"
  {% endif %}
  src="{{ image | img_url: size }}"
  alt="{{ image.alt | escape }}"
>
```

---

## 5. 🌐 Árvore de Dependência da Rede (6.998ms crítico)

### Caminho Crítico Identificado:
1. **HTML inicial**: 878ms, 41,87 KiB
2. **Harmonia Sans**: 1.077ms, 23,16 KiB
3. **v2/client.init-shop**: 924ms, 1,61 KiB
4. **chunk.common**: 977ms, 41,73 KiB
5. **Google Fonts**: 1.732ms, 1,27 KiB
6. **Assets diversos**: 900ms+ cada

### ✅ Correções Necessárias:

#### A. Preconnect para Domínios Críticos
```html
<!-- ARQUIVO: layout/theme.liquid -->
<!-- Adicionar no início do <head> -->

<link rel="preconnect" href="https://cdn.shopify.com" crossorigin>
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="dns-prefetch" href="https://rebuyengine.com">
<link rel="dns-prefetch" href="https://static.klaviyo.com">
<link rel="dns-prefetch" href="https://cdn.growthbook.io">
```

#### B. Resource Hints Inteligentes
```liquid
<!-- ARQUIVO: layout/theme.liquid -->
<!-- Prefetch recursos que serão usados -->

<link rel="prefetch" href="{{ 'product.js' | asset_url }}">
<link rel="prefetch" href="{{ 'cart-drawer.js' | asset_url }}">
<link rel="prefetch" href="{{ 'predictive-search.js' | asset_url }}">

{% comment %} Preload apenas recursos críticos {% endcomment %}
<link rel="preload" href="{{ 'app.css' | asset_url }}" as="style">
<link rel="preload" href="{{ 'app.js' | asset_url }}" as="script">
```

#### C. Otimizar Carregamento de Fontes
```liquid
<!-- ARQUIVO: snippets/head-variables.liquid -->
<!-- Otimizar carregamento da Harmonia Sans -->

{% unless settings.heading_font.system? and settings.body_font.system? %}
  <link rel="preconnect" href="https://fonts.shopifycdn.com" crossorigin>
  
  {% comment %} Preload apenas as fontes críticas {% endcomment %}
  <link rel="preload" href="{{ settings.heading_font | font_url }}" as="font" type="font/woff2" crossorigin>
  <link rel="preload" href="{{ settings.body_font | font_url }}" as="font" type="font/woff2" crossorigin>
{% endunless %}
```

#### D. Implementar Service Worker para Cache
```javascript
// ARQUIVO: assets/sw.js
// Service Worker para cache agressivo

const CACHE_NAME = 'rovelab-v1';
const CRITICAL_RESOURCES = [
  '/cdn/shop/files/harmonia_sans/harmoniasans_n4.95f20f5.woff2',
  '/assets/app.css',
  '/assets/app.js',
  '/assets/global.css'
];

self.addEventListener('install', event => {
  event.waitUntil(
    caches.open(CACHE_NAME)
      .then(cache => cache.addAll(CRITICAL_RESOURCES))
  );
});

self.addEventListener('fetch', event => {
  if (event.request.destination === 'font' || 
      event.request.url.includes('/assets/')) {
    event.respondWith(
      caches.match(event.request)
        .then(response => response || fetch(event.request))
    );
  }
});
```

---

## 6. 🖼️ Elementos de Imagem sem Atributos Alt

### Problemas Identificados:
Múltiplas imagens sem atributo `alt`, incluindo:
1. **Imagens de produto** no Cevoid Content
2. **Thumbnails de vídeo** 
3. **Imagens de galeria**
4. **Imagens decorativas**

### ✅ Correções Necessárias:

#### A. Adicionar Alt Texts Descritivos
```liquid
<!-- ARQUIVO: snippets/product-card.liquid -->
<!-- Corrigir imagens de produtos -->

<!-- ANTES -->
<img src="{{ product.featured_image | img_url: '400x400' }}">

<!-- DEPOIS -->
<img 
  src="{{ product.featured_image | img_url: '400x400' }}" 
  alt="{{ product.featured_image.alt | default: product.title | escape }}"
  loading="lazy"
>
```

#### B. Alt Texts para Vídeos e Thumbnails
```liquid
<!-- ARQUIVO: sections/background-video.liquid -->
<!-- Corrigir thumbnails de vídeo -->

<img 
  class="cevoid-video-thumbnail"
  src="{{ video_thumbnail_url }}"
  alt="Thumbnail do vídeo: {{ section.settings.video_title | default: 'Vídeo promocional' | escape }}"
  loading="lazy"
>
```

#### C. Imagens Decorativas
```liquid
<!-- ARQUIVO: snippets/responsive-image.liquid -->
<!-- Para imagens puramente decorativas -->

{% if decorative %}
  <img 
    src="{{ image | img_url: size }}"
    alt=""
    role="presentation"
    loading="lazy"
  >
{% else %}
  <img 
    src="{{ image | img_url: size }}"
    alt="{{ image.alt | default: fallback_alt | escape }}"
    loading="lazy"
  >
{% endif %}
```

#### D. Sistema de Alt Text Automático
```liquid
<!-- ARQUIVO: snippets/product-image.liquid -->
<!-- Sistema inteligente de alt text -->

{% liquid
  assign alt_text = image.alt
  
  if alt_text == blank
    if product
      assign alt_text = product.title | append: ' - ' | append: image.position
    elsif collection
      assign alt_text = collection.title | append: ' collection image'
    else
      assign alt_text = 'Imagem do produto'
    endif
  endif
%}

<img 
  src="{{ image | img_url: size }}"
  alt="{{ alt_text | escape }}"
  loading="{{ loading | default: 'lazy' }}"
>
```

---

## 7. 🔗 Links sem Nome Compreensível

### Problema Identificado:
- **Link problemático**: `<a href="/collections/m1-sofa-collection" class="single-image__link">`
- **Contexto**: Link em imagem sem texto descritivo

### ✅ Correções Necessárias:

#### A. Adicionar Texto Descritivo ao Link
```liquid
<!-- ARQUIVO: sections/single-image.liquid -->
<!-- Corrigir link sem nome -->

<!-- ANTES -->
<a href="/collections/m1-sofa-collection" class="single-image__link">
  <img src="...">
</a>

<!-- DEPOIS -->
<a href="/collections/m1-sofa-collection" class="single-image__link" aria-label="Ver coleção M1 Sofa">
  <img src="..." alt="Coleção M1 Sofa">
  <span class="sr-only">Ver coleção M1 Sofa</span>
</a>
```

#### B. Implementar Screen Reader Classes
```css
/* ARQUIVO: assets/global.css */
/* Adicionar classe para screen readers */

.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}

.sr-only-focusable:active,
.sr-only-focusable:focus {
  position: static;
  width: auto;
  height: auto;
  margin: 0;
  overflow: visible;
  clip: auto;
  white-space: normal;
}
```

#### C. Validar Todos os Links
```javascript
// ARQUIVO: assets/accessibility-checker.js
// Script de desenvolvimento para validar links

if (window.location.hostname === 'localhost') {
  document.addEventListener('DOMContentLoaded', () => {
    const links = document.querySelectorAll('a');
    
    links.forEach(link => {
      const hasText = link.textContent.trim().length > 0;
      const hasAriaLabel = link.getAttribute('aria-label');
      const hasAriaLabelledBy = link.getAttribute('aria-labelledby');
      const hasTitle = link.getAttribute('title');
      
      if (!hasText && !hasAriaLabel && !hasAriaLabelledBy && !hasTitle) {
        console.warn('Link sem nome acessível encontrado:', link);
        link.style.outline = '2px solid red';
      }
    });
  });
}
```

---

## 8. 📋 Elementos de Título Fora de Ordem Sequencial

### Problemas Identificados:
1. **H3**: "您能感受到的創新" (Inovação que você pode sentir)
2. **H4**: "大車" (Carro grande) - deveria ser H2 ou H3

### ✅ Correções Necessárias:

#### A. Corrigir Hierarquia de Títulos
```liquid
<!-- ARQUIVO: sections/main-product.liquid ou similar -->
<!-- Corrigir ordem dos headings -->

<!-- ANTES -->
<h3 class="h3">您能感受到的創新</h3>
<!-- ... outros conteúdos ... -->
<h4 class="body-font">大車</h4>

<!-- DEPOIS -->
<h2 class="h3">您能感受到的創新</h2>
<!-- ... outros conteúdos ... -->
<h3 class="body-font">大車</h3>
```

#### B. Implementar Sistema de Headings Semânticos
```liquid
<!-- ARQUIVO: snippets/section-header.liquid -->
<!-- Sistema de headings baseado no nível da seção -->

{% liquid
  assign heading_level = heading_level | default: 2
  assign heading_tag = 'h' | append: heading_level
%}

<{{ heading_tag }} class="{{ heading_class | default: 'section-heading' }}">
  {{ heading_text }}
</{{ heading_tag }}>

{% comment %} Para sub-headings {% endcomment %}
{% assign sub_heading_level = heading_level | plus: 1 %}
<h{{ sub_heading_level }} class="{{ sub_heading_class | default: 'sub-heading' }}">
  {{ sub_heading_text }}
</h{{ sub_heading_level }}>
```

#### C. Validador de Hierarquia de Títulos
```javascript
// ARQUIVO: assets/heading-validator.js
// Validação automática da hierarquia

if (window.location.hostname === 'localhost') {
  document.addEventListener('DOMContentLoaded', () => {
    const headings = document.querySelectorAll('h1, h2, h3, h4, h5, h6');
    let lastLevel = 0;
    
    headings.forEach((heading, index) => {
      const currentLevel = parseInt(heading.tagName.charAt(1));
      
      if (index === 0 && currentLevel !== 1) {
        console.warn('Primeira heading deveria ser H1:', heading);
      }
      
      if (currentLevel > lastLevel + 1) {
        console.warn('Hierarquia de heading pulou nível:', {
          previous: lastLevel,
          current: currentLevel,
          element: heading
        });
        heading.style.outline = '2px solid orange';
      }
      
      lastLevel = currentLevel;
    });
  });
}
```

---

## 9. 📐 Imagens com Proporção Incorreta

### Problema Identificado:
- **Banner**: `M_BANNER_DEFAULT_2_1_20x32_crop_center.jpg`
- **Proporção Exibida**: 1080 x 1740 (0.62)
- **Proporção Real**: 20 x 32 (0.63)
- **Diferença**: Pequena distorção causando layout shift

### ✅ Correções Necessárias:

#### A. Definir Aspect Ratio Correto
```css
/* ARQUIVO: assets/single-image.css */
/* Corrigir proporção da imagem */

.single-image--mobile img {
  aspect-ratio: 20 / 32; /* Proporção real da imagem */
  width: 100%;
  height: auto;
  object-fit: cover;
}

.single-image--desktop img {
  aspect-ratio: 1080 / 1740; /* Proporção de exibição desktop */
  width: 100%;
  height: auto;
  object-fit: cover;
}
```

#### B. Picture Element para Diferentes Proporções
```liquid
<!-- ARQUIVO: sections/single-image.liquid -->
<!-- Usar picture element para responsive images -->

<picture>
  <!-- Mobile: proporção 20/32 -->
  <source 
    media="(max-width: 767px)"
    srcset="{{ image | img_url: '400x640' }} 1x,
            {{ image | img_url: '800x1280' }} 2x"
    width="400"
    height="640">
  
  <!-- Desktop: proporção 1080/1740 -->
  <source 
    media="(min-width: 768px)"
    srcset="{{ image | img_url: '1080x1740' }} 1x,
            {{ image | img_url: '2160x3480' }} 2x"
    width="1080"
    height="1740">
  
  <!-- Fallback -->
  <img 
    src="{{ image | img_url: '1080x1740' }}"
    width="1080"
    height="1740"
    alt="{{ image.alt | escape }}"
    loading="{{ loading | default: 'lazy' }}">
</picture>
```

#### C. Validador de Proporções
```javascript
// ARQUIVO: assets/image-ratio-validator.js
// Detectar imagens com proporção incorreta

if (window.location.hostname === 'localhost') {
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
        
        // Tolerância de 3% para diferença de proporção
        if (Math.abs(displayedRatio - naturalRatio) > 0.03) {
          console.warn('Proporção incorreta:', {
            src: this.src,
            displayed: `${displayed.width}x${displayed.height} (${displayedRatio.toFixed(2)})`,
            natural: `${natural.width}x${natural.height} (${naturalRatio.toFixed(2)})`
          });
        }
      });
    });
  });
}
```

---

## 10. ❌ Erros do Navegador no Console

### Erros Identificados:
1. **shop.app**: Failed to load resource: 403 (Forbidden)
2. **rovelab.hk**: Failed to load resource: 404 (Not Found)
   - `timesact/shop:1:0` - 404 Not Found
   - `shop/settings:1:0` - 404 Not Found
3. **SyntaxError**: Invalid or unexpected token
4. **TypeError**: Cannot destructure property 'settings'

### ✅ Correções Necessárias:

#### A. Corrigir Recursos 404
```liquid
<!-- ARQUIVO: layout/theme.liquid -->
<!-- Verificar se arquivos existem antes de carregar -->

{% if content_for_header contains 'timesact.js' %}
  <script src="{{ 'timesact.js' | asset_url }}" defer="defer"></script>
{% endif %}

<!-- Remover referências a arquivos inexistentes -->
{% comment %} 
  Verificar e remover:
  - shop/settings:1:0
  - timesact/shop:1:0
{% endcomment %}
```

#### B. Corrigir SyntaxError no JavaScript
```javascript
// ARQUIVO: Procurar arquivo que contém erro de sintaxe
// Provavelmente em assets/timesact.js ou similar

// ANTES (provável erro)
const settings = { shop: {{ shop | json }} };

// DEPOIS (corrigido)
const settings = { 
  shop: {{ shop | json }},
  // Garantir que não há trailing commas ou caracteres inválidos
};
```

#### C. Handle de Recursos Faltantes
```javascript
// ARQUIVO: assets/error-handler.js
// Adicionar tratamento global de erros

window.addEventListener('error', function(e) {
  // Log de recursos que falharam ao carregar
  if (e.target !== window) {
    console.warn('Resource failed to load:', e.target.src || e.target.href);
    
    // Para Shop.app especificamente
    if (e.target.src && e.target.src.includes('shop.app')) {
      console.warn('Shop.app failed to load - this is expected');
      return true; // Previne propagação do erro
    }
  }
}, true);

// Handle de erros não capturados
window.addEventListener('unhandledrejection', function(event) {
  console.warn('Unhandled promise rejection:', event.reason);
  
  // Prevenir que erros de terceiros quebrem a página
  if (event.reason && event.reason.toString().includes('settings')) {
    event.preventDefault();
  }
});
```

#### D. Validação de Dependências
```liquid
<!-- ARQUIVO: layout/theme.liquid -->
<!-- Verificar dependências antes de usar -->

<script>
  // Verificar se objetos globais existem antes de usar
  if (typeof window.Shop !== 'undefined' && window.Shop.app) {
    // Usar Shop.app apenas se disponível
    window.Shop.app.init();
  }
  
  if (typeof window.timesact !== 'undefined') {
    // Usar timesact apenas se carregado
    window.timesact.init();
  }
</script>
```

---

## 🎯 Prioridades de Implementação

### 🔴 Alta Prioridade (Impacto Imediato)
1. **Corrigir erros de console** - Eliminar 404s e SyntaxErrors
2. **Adicionar alt texts** - Compliance de acessibilidade
3. **Otimizar LCP** - Fetchpriority e preload da imagem principal
4. **Inline Critical CSS** - Reduzir render blocking de 40ms
5. **Preconnect domínios** - Reduzir latência de rede

### 🟡 Média Prioridade
6. **Corrigir hierarquia de títulos** - SEO e acessibilidade
7. **Remover JavaScript legado** - 58 KiB de economia
8. **Otimizar reflows** - 169ms de economia
9. **Corrigir proporções de imagem** - Reduzir CLS
10. **Implementar lazy loading inteligente**

### 🟢 Baixa Prioridade (Melhorias incrementais)
11. **Service Worker para cache**
12. **Virtual scrolling**
13. **Validadores de desenvolvimento**
14. **Monitoramento de performance**

---

## 📊 Resultados Esperados

Após implementação completa:
- **Redução de ~98 KiB** em JavaScript legado
- **Melhoria de ~40ms** em render blocking
- **Eliminação de ~169ms** em reflows forçados
- **Redução significativa** no tempo de LCP
- **Zero erros** no console do navegador
- **100% compliance** de acessibilidade básica
- **Score Performance**: 80+ (verde)
- **Score Acessibilidade**: 95+ (verde)

---

## 🚀 Quick Wins - Ações Imediatas

### Pode implementar AGORA (< 30 minutos):

1. **Adicionar Alt Texts**
   ```liquid
   <!-- Adicionar em todas as imagens -->
   alt="{{ image.alt | default: product.title | escape }}"
   ```

2. **Preconnect Domínios**
   ```html
   <!-- layout/theme.liquid -->
   <link rel="preconnect" href="https://cdn.shopify.com" crossorigin>
   <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
   ```

3. **Fetchpriority para LCP**
   ```html
   <!-- Primeira imagem da página -->
   <img fetchpriority="high" loading="eager">
   ```

4. **Corrigir Links sem Nome**
   ```html
   <!-- Adicionar aria-label -->
   <a href="/collections/m1-sofa-collection" aria-label="Ver coleção M1 Sofa">
   ```

5. **Inline Critical CSS**
   ```html
   <!-- Mover CSS crítico para <style> no head -->
   <style>.announcement-bar { /* CSS crítico */ }</style>
   ```

### Impacto Esperado das Quick Wins:
- **~300ms** de melhoria no LCP
- **~40ms** de redução em render blocking
- **100%** compliance básica de acessibilidade
- **Eliminação de erros** críticos no console
- **Melhoria imediata** na experiência do usuário

---

## 🔧 Ferramentas Recomendadas

1. **Lighthouse CI** - Monitoramento contínuo de performance
2. **axe DevTools** - Auditoria de acessibilidade
3. **Chrome DevTools Coverage** - Identificar código não usado
4. **WebPageTest** - Análise detalhada de performance
5. **Squoosh.app** - Otimização de imagens
6. **Can I Use** - Verificar suporte de navegadores modernos

---

## 📝 Notas de Implementação

- Sempre testar mudanças em ambiente de desenvolvimento primeiro
- Usar feature flags para rollout gradual de mudanças significativas  
- Monitorar métricas Core Web Vitals após cada deploy
- Manter backup dos arquivos antes de alterações críticas
- Considerar impacto em diferentes dispositivos e conexões

