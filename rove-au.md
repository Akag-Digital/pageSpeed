# 📊 Plano de Otimização de Performance
**Data**: Agosto 2025  
**Análise**: Google PageSpeed Insights  
**URL**: https://rovelab.au  
**URL Repositório**: https://github.com/RoveLab/ecommerce-shopify-rove-au  
**PageSpeed Insights**: https://pagespeed.web.dev/analysis/https-rovelab-au/q8rorqm0nx?hl=pt&form_factor=desktop


## 📑 Índice de Otimizações

### Performance
1. [🔤 Otimização de Fontes](#1--otimização-de-fontes-economia-140ms)
2. [🚫 Eliminar Recursos Render-Blocking](#2--eliminar-recursos-render-blocking-economia-70ms)
3. [🔄 Reduzir Reflow Forçado](#3--reduzir-reflow-forçado-169ms-total)
4. [🎯 Otimizar LCP](#4--otimizar-lcp-largest-contentful-paint)
5. [🌐 Otimizar Árvore de Dependências](#5--otimizar-árvore-de-dependências-de-rede)
6. [⚡ Reduzir Tempo de Execução JavaScript](#6--reduzir-tempo-de-execução-javascript-15s-economia)
7. [🗑️ Remover JavaScript Não Usado](#7-️-remover-javascript-não-usado-843-kib)

### Práticas Recomendadas
8. [📐 Imagens com Proporção Incorreta](#8--imagens-com-proporção-incorreta)
9. [❌ Erros no Console do Navegador](#9--erros-no-console-do-navegador)

---

## 📈 Métricas Atuais
- **Performance Score**: Necessita melhoria
- **Economia Potencial**: ~843 KiB em JavaScript não usado
- **Tempo de Bloqueio**: ~70ms em requisições render-blocking
- **Reflow Forçado**: 169ms total
- **Erros no Console**: 3 erros (CORS, Failed Resources, SyntaxError)
- **Imagens Distorcidas**: 1 banner principal com proporção incorreta
- **Fontes**: 140ms de delay no carregamento

---

## 1. 🔤 Otimização de Fontes (Economia: 140ms)

### Problema:
- Fonte `fa-light-300.woff2` do Rebuy Engine causando delay de 140ms
- Carregamento sem `font-display: swap`

### ✅ Correção:
```html
<!-- ARQUIVO: layout/theme.liquid -->
<!-- Adicionar no <head> -->

<link rel="preconnect" href="https://rebuyengine.com">
<link rel="dns-prefetch" href="https://rebuyengine.com">

<!-- Adicionar font-display: swap -->
<style>
  @font-face {
    font-family: 'FA Light';
    src: url('https://rebuyengine.com/webfonts/fa-light-300.woff2') format('woff2');
    font-display: swap; /* Critical for performance */
    font-weight: 300;
  }
  
  /* Aplicar swap para todas as fontes */
  * {
    font-display: swap;
  }
</style>
```

---

## 2. 🚫 Eliminar Recursos Render-Blocking (Economia: 70ms)

### Recursos Bloqueantes:
1. `announcement-bar.css` (4.9 KiB)
2. `accelerated-checkout-backwards-compat.css` (2.8 KiB) 
3. `native-geo-redirects.min.css` (3.5 KiB)

### ✅ Correções:

#### A. Defer CSS não-crítico
```liquid
<!-- ARQUIVO: layout/theme.liquid -->
<!-- Carregar CSS não-crítico de forma assíncrona -->

<!-- ANTES -->
{{ 'announcement-bar.css' | asset_url | stylesheet_tag }}
{{ 'accelerated-checkout-backwards-compat.css' | asset_url | stylesheet_tag }}

<!-- DEPOIS -->
<link rel="preload" href="{{ 'announcement-bar.css' | asset_url }}" as="style" onload="this.onload=null;this.rel='stylesheet'">
<noscript><link rel="stylesheet" href="{{ 'announcement-bar.css' | asset_url }}"></noscript>

<link rel="preload" href="{{ 'accelerated-checkout-backwards-compat.css' | asset_url }}" as="style" onload="this.onload=null;this.rel='stylesheet'">
<noscript><link rel="stylesheet" href="{{ 'accelerated-checkout-backwards-compat.css' | asset_url }}"></noscript>
```

#### B. Inline Critical CSS
```liquid
<!-- ARQUIVO: layout/theme.liquid -->
<!-- Adicionar no <head> -->

<style>
  /* Critical CSS - inline apenas o necessário para above-the-fold */
  .announcement-bar { 
    min-height: 40px; 
    background: var(--announcement-bg, #000);
    color: var(--announcement-text, #fff);
  }
  .header { 
    position: relative;
    z-index: 100;
  }
  .product-card { 
    display: block; 
    min-height: 300px;
  }
  /* ... outros estilos críticos ... */
</style>
```

---

## 3. 🔄 Reduzir Reflow Forçado (169ms total)

### Scripts causando reflow:
1. `animations.min.js` (26ms)
2. `vendor.min.js` (8ms)
3. Scripts de terceiros (135ms total)

### ✅ Correções:

#### A. Otimizar animações com CSS transforms
```css
/* ARQUIVO: assets/animations.css */
/* Usar transforms em vez de mudanças de layout */

/* ANTES */
.slide-in {
  left: -100px;
  transition: left 0.3s ease;
}
.slide-in.active {
  left: 0;
}

/* DEPOIS */
.slide-in {
  transform: translateX(-100px);
  transition: transform 0.3s ease;
  will-change: transform;
}
.slide-in.active {
  transform: translateX(0);
}
```

#### B. Debounce scroll events
```javascript
// ARQUIVO: assets/app.js
// Adicionar debounce para scroll

let scrollTimeout;
let isScrolling = false;

window.addEventListener('scroll', () => {
  if (!isScrolling) {
    window.requestAnimationFrame(() => {
      // Seu código de scroll aqui
      handleScroll();
      isScrolling = false;
    });
    isScrolling = true;
  }
}, { passive: true });

function handleScroll() {
  // Lógica de scroll otimizada
}
```

#### C. Batch DOM operations
```javascript
// ARQUIVO: assets/product.js
// Agrupar mudanças no DOM

// ANTES
elements.forEach(el => {
  el.style.opacity = '0.5';
  el.style.transform = 'scale(0.9)';
  el.classList.add('loading');
});

// DEPOIS
// Usar DocumentFragment para mudanças em lote
const fragment = document.createDocumentFragment();
elements.forEach(el => {
  el.style.cssText = 'opacity: 0.5; transform: scale(0.9);';
  el.classList.add('loading');
});
```

---

## 4. 🎯 Otimizar LCP (Largest Contentful Paint)

### Elemento LCP identificado:
- Imagem do banner principal com proporção incorreta
- Carregamento sem prioridade adequada

### ✅ Correções:

```liquid
<!-- ARQUIVO: sections/slideshow.liquid ou hero section -->
<!-- Adicionar fetchpriority e preload para LCP -->

{% if section.settings.image %}
  <link rel="preload" as="image" href="{{ section.settings.image | image_url: width: 1920 }}" fetchpriority="high">
{% endif %}

<img 
  src="{{ section.settings.image | image_url: width: 1920 }}"
  srcset="{{ section.settings.image | image_url: width: 375 }} 375w,
          {{ section.settings.image | image_url: width: 750 }} 750w,
          {{ section.settings.image | image_url: width: 1125 }} 1125w,
          {{ section.settings.image | image_url: width: 1920 }} 1920w"
  sizes="100vw"
  fetchpriority="high"
  loading="eager" <!-- Não usar lazy para LCP -->
  decoding="async"
  alt="{{ section.settings.image.alt | escape }}"
>
```

---

## 5. 🌐 Otimizar Árvore de Dependências de Rede

### Critical Path (6.495ms):
1. HTML → 282ms
2. Harmonia Sans → 691ms  
3. Client Shop → 304ms
4. Chunk Common → 348ms

### ✅ Correções:

#### A. Preconnect para recursos críticos
```html
<!-- ARQUIVO: layout/theme.liquid -->
<!-- Adicionar no início do <head> -->

<link rel="preconnect" href="https://cdn.shopify.com" crossorigin>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link rel="dns-prefetch" href="https://www.googletagmanager.com">
<link rel="dns-prefetch" href="https://rebuyengine.com">
```

#### B. Resource Hints para recursos secundários
```liquid
<!-- ARQUIVO: layout/theme.liquid -->

{% comment %} Prefetch recursos que serão usados em breve {% endcomment %}
<link rel="prefetch" href="{{ 'product.js' | asset_url }}">
<link rel="prefetch" href="{{ 'cart.js' | asset_url }}">
<link rel="prefetch" href="{{ 'collection-tabs.js' | asset_url }}">
```

---

## 6. ⚡ Reduzir Tempo de Execução JavaScript (1.5s economia)

### Scripts pesados:
1. `animations.min.js` - 663ms
2. `vendor.min.js` - 64ms  
3. Google Tag Manager - 371ms
4. Terceiros - 400ms+

### ✅ Correções:

#### A. Lazy Load scripts não-críticos
```javascript
// ARQUIVO: layout/theme.liquid
// Adicionar lazy loading condicional

// Carregar GTM apenas após interação
let gtmLoaded = false;
const loadGTM = () => {
  if (!gtmLoaded) {
    const script = document.createElement('script');
    script.src = 'https://www.googletagmanager.com/gtm.js?id=GTM-WR9NJ9V6';
    script.async = true;
    document.head.appendChild(script);
    gtmLoaded = true;
  }
};

// Trigger após 3 segundos ou primeira interação
setTimeout(loadGTM, 3000);
['scroll', 'click', 'touchstart'].forEach(event => {
  window.addEventListener(event, loadGTM, { once: true, passive: true });
});
```

#### B. Otimizar animations.min.js
```javascript
// ARQUIVO: assets/animations.js
// Usar Intersection Observer em vez de scroll events

const observerOptions = {
  root: null,
  rootMargin: '50px',
  threshold: 0.1
};

const animationObserver = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.classList.add('animate-in');
      animationObserver.unobserve(entry.target);
    }
  });
}, observerOptions);

// Observar elementos apenas quando necessário
document.querySelectorAll('[data-animate]').forEach(el => {
  animationObserver.observe(el);
});
```

#### C. Code Splitting para features específicas
```liquid
<!-- ARQUIVO: templates/product.liquid -->
<!-- Carregar scripts apenas quando necessário -->

{% if template.name == 'product' %}
  <script src="{{ 'product.js' | asset_url }}" defer></script>
  <script src="{{ 'product-recommendations.js' | asset_url }}" defer></script>
{% endif %}

{% if template.name == 'collection' %}
  <script src="{{ 'collection-tabs.js' | asset_url }}" defer></script>
  <script src="{{ 'facets.js' | asset_url }}" defer></script>
{% endif %}
```

---

## 7. 🗑️ Remover JavaScript Não Usado (843 KiB)

### Bibliotecas com código não usado:
1. Rebuy Engine - 366 KiB (251 KiB não usado)
2. Google Tag Manager - 515 KiB (205 KiB não usado)  
3. Loox.io - 171 KiB (129 KiB não usado)
4. TikTok Pixel - 80 KiB (40 KiB não usado)

### ✅ Correções:

#### A. Conditional Loading por template
```liquid
<!-- ARQUIVO: layout/theme.liquid -->

{% comment %} Carregar Rebuy apenas em páginas de produto {% endcomment %}
{% if template.name == 'product' or template.name == 'cart' %}
  <script src="https://rebuyengine.com/js/global.js" defer></script>
{% endif %}

{% comment %} Loox apenas em produtos com reviews {% endcomment %}
{% if template.name == 'product' and product.metafields.loox.num_reviews > 0 %}
  <script src="https://loox.io/widget/{{ shop.id }}" defer></script>
{% endif %}

{% comment %} TikTok apenas após consentimento {% endcomment %}
<script>
  if (window.cookieConsent?.marketing) {
    const tiktokScript = document.createElement('script');
    tiktokScript.src = 'https://analytics.tiktok.com/...';
    tiktokScript.defer = true;
    document.head.appendChild(tiktokScript);
  }
</script>
```

#### B. Implementar Module Loading
```javascript
// ARQUIVO: assets/app.js
// Usar dynamic imports para features opcionais

// Carregar módulos apenas quando necessário
const loadModule = async (moduleName) => {
  try {
    const module = await import(`./modules/${moduleName}.js`);
    return module.default;
  } catch (error) {
    console.warn(`Failed to load module: ${moduleName}`, error);
    return null;
  }
};

// Exemplo de uso
document.addEventListener('DOMContentLoaded', async () => {
  // Carregar módulo de produto apenas se existir
  if (document.querySelector('.product-form')) {
    const ProductModule = await loadModule('product');
    if (ProductModule) new ProductModule();
  }
  
  // Carregar carrinho apenas se houver itens
  if (window.cart?.item_count > 0) {
    const CartModule = await loadModule('cart-drawer');
    if (CartModule) new CartModule();
  }
});
```

---

## 8. 📐 Imagens com Proporção Incorreta

### Problema Identificado:
- Banner principal
  - Exibido: 1180 x 1440 (ratio 0.82)
  - Real: 20 x 24 (ratio 0.83)
  - Pequena distorção causando layout shift

### ✅ Correções:

#### A. Definir Aspect Ratio Correto
```css
/* ARQUIVO: assets/single-image.css */
/* Adicionar aspect-ratio específico */

.single-image img,
.hero-banner img {
  aspect-ratio: 20 / 24; /* Proporção real da imagem */
  width: 100%;
  height: auto;
  object-fit: cover; /* Previne distorção */
}

/* Para diferentes breakpoints */
@media (min-width: 768px) {
  .single-image img,
  .hero-banner img {
    aspect-ratio: 1180 / 1440; /* Ajustar para desktop */
  }
}

@media (max-width: 767px) {
  .single-image img,
  .hero-banner img {
    aspect-ratio: 4 / 5; /* Proporção mobile otimizada */
  }
}
```

#### B. Usar Picture Element para Diferentes Proporções
```liquid
<!-- ARQUIVO: sections/single-image.liquid -->
<!-- Substituir img simples por picture element -->

<picture>
  <!-- Mobile: proporção 4/5 -->
  <source 
    media="(max-width: 767px)"
    srcset="{{ image | image_url: width: 375, height: 469 }} 375w,
            {{ image | image_url: width: 750, height: 938 }} 750w"
    width="375"
    height="469">
  
  <!-- Desktop: proporção 1180/1440 -->
  <source 
    media="(min-width: 768px)"
    srcset="{{ image | image_url: width: 1180, height: 1440 }} 1x,
            {{ image | image_url: width: 2360, height: 2880 }} 2x"
    width="1180"
    height="1440">
  
  <!-- Fallback -->
  <img 
    src="{{ image | image_url: width: 1180, height: 1440 }}"
    width="1180"
    height="1440"
    alt="{{ image.alt | escape }}"
    loading="{{ loading | default: 'lazy' }}"
    decoding="async">
</picture>
```

#### C. Implementar Image Placeholder
```css
/* ARQUIVO: assets/global.css */
/* Prevenir layout shift durante carregamento */

.image-placeholder {
  background: linear-gradient(90deg, #f0f0f0 25%, transparent 25%),
              linear-gradient(90deg, #f0f0f0 50%, #e0e0e0 50%),
              linear-gradient(90deg, transparent 75%, #f0f0f0 75%);
  background-size: 20px 20px;
  animation: loading-shimmer 1.5s infinite;
}

@keyframes loading-shimmer {
  0% { background-position: -200px 0; }
  100% { background-position: 200px 0; }
}

/* Aplicar aos contêineres de imagem */
.single-image,
.hero-banner {
  position: relative;
  overflow: hidden;
}

.single-image::before,
.hero-banner::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: #f5f5f5;
  z-index: -1;
}
```

---

## 9. ❌ Erros no Console do Navegador

### Erros Identificados:
1. **Failed to load resource** - Growthbook SDK (net::ERR_TIMED_OUT)
2. **Failed to load resource** - Gorgias chat (403 Forbidden) 
3. **SyntaxError** - Invalid token (linha não especificada)

### ✅ Correções:

#### A. Handle Failed Resources Gracefully
```javascript
// ARQUIVO: assets/app.js
// Adicionar error handling para recursos que falham

// Growthbook SDK fallback
window.addEventListener('error', function(e) {
  if (e.target?.src?.includes('growthbook.io')) {
    console.warn('Growthbook SDK failed to load, using fallback');
    // Implementar fallback para feature flags
    window.growthbook = {
      isOn: () => false,
      getFeatureValue: () => null
    };
  }
  
  if (e.target?.src?.includes('gorgias.chat')) {
    console.warn('Gorgias chat failed to load');
    // Remover elemento quebrado
    e.target?.remove();
    // Mostrar chat alternativo se disponível
    document.querySelector('.chat-fallback')?.classList.remove('hidden');
  }
}, true);
```

#### B. Implement Timeout for Third-party Scripts
```javascript
// ARQUIVO: layout/theme.liquid
// Adicionar timeout para scripts de terceiros

const loadScriptWithTimeout = (src, timeout = 5000) => {
  return new Promise((resolve, reject) => {
    const script = document.createElement('script');
    script.src = src;
    script.async = true;
    
    const timeoutId = setTimeout(() => {
      script.remove();
      reject(new Error(`Script timeout: ${src}`));
    }, timeout);
    
    script.onload = () => {
      clearTimeout(timeoutId);
      resolve();
    };
    
    script.onerror = () => {
      clearTimeout(timeoutId);
      reject(new Error(`Script error: ${src}`));
    };
    
    document.head.appendChild(script);
  });
};

// Uso para scripts problemáticos
loadScriptWithTimeout('https://cdn.growthbook.io/lib/growthbook.js')
  .catch(err => console.warn('Growthbook failed:', err));
```

#### C. Fix Syntax Errors
```liquid
<!-- ARQUIVO: Procurar por dados JSON não escapados -->
<!-- Verificar todos os templates que passam dados Liquid para JavaScript -->

<!-- ANTES (provável erro) -->
<script>
  const productData = {{ product | json }};
  const settings = {{ section.settings }};
</script>

<!-- DEPOIS (corrigido) -->
<script>
  const productData = {{ product | json }};
  const settings = {{ section.settings | json }};
  
  // Validar dados antes de usar
  if (typeof productData === 'object' && productData !== null) {
    // Usar productData
  }
</script>
```

---

## 🎯 Prioridades de Implementação

### 🔴 Alta Prioridade (Impacto Imediato)
1. **Implementar font-display: swap** - 140ms de melhoria
2. **Defer CSS não-crítico** - 70ms de melhoria no blocking time
3. **Corrigir erros de console** - Melhora estabilidade e UX
4. **Corrigir proporções de imagem** - Reduz CLS
5. **Otimizar LCP com fetchpriority** - Melhora percepção de velocidade

### 🟡 Média Prioridade
6. **Lazy load scripts de terceiros** - Reduz JavaScript inicial
7. **Implementar aspect-ratio CSS** - Previne layout shifts
8. **Adicionar preconnect para recursos críticos** - Melhora network timing
9. **Otimizar animações com transforms** - Reduz reflow forçado

### 🟢 Baixa Prioridade (Melhorias incrementais)
10. **Code splitting avançado**
11. **Implementar Intersection Observer**
12. **Tree shaking para módulos próprios**
13. **Implementar monitoring de erros**

---

## 📊 Resultados Esperados

Após implementação completa:
- **Redução de ~843 KiB** em JavaScript não usado
- **Melhoria de ~280ms** no tempo de carregamento (140ms fontes + 70ms blocking + 70ms reflow)
- **CLS < 0.1** (bom) - correção de proporções
- **FCP < 1.8s** (bom) - otimização de fontes e CSS
- **LCP < 2.5s** (bom) - priorização de imagem hero
- **Score Performance**: 70+ (verde)

---

## 🔧 Ferramentas Recomendadas

1. **Squoosh.app** - Otimização de imagens
2. **Webpack Bundle Analyzer** - Análise de bundles JavaScript
3. **Chrome DevTools Coverage** - Identificar código não usado
4. **Lighthouse CI** - Monitoramento contínuo de performance
5. **WebPageTest** - Análise detalhada de waterfall
6. **CookieYes** - Gerenciamento de consentimento para scripts
7. **Sentry** - Monitoramento de erros JavaScript

---

## 🚀 Quick Wins - Ações Imediatas

### Pode implementar AGORA (< 30 minutos):

1. **Font Display Swap**
   ```css
   /* layout/theme.liquid - adicionar no <head> */
   <style>
     * { font-display: swap; }
   </style>
   ```

2. **Preconnect Domains**
   ```html
   <!-- layout/theme.liquid -->
   <link rel="preconnect" href="https://cdn.shopify.com">
   <link rel="preconnect" href="https://rebuyengine.com">
   ```

3. **Aspect Ratio CSS**
   ```css
   /* assets/single-image.css */
   .single-image img { aspect-ratio: 1180/1440; }
   ```

4. **Defer Non-Critical CSS**
   ```html
   <link rel="preload" href="{{ 'announcement-bar.css' | asset_url }}" as="style" onload="this.rel='stylesheet'">
   ```

5. **Error Handling**
   ```javascript
   // assets/app.js
   window.addEventListener('error', function(e) {
     if (e.target?.src?.includes('growthbook')) {
       console.warn('Third-party script failed');
       e.target?.remove();
     }
   }, true);
   ```

### Impacto Esperado das Quick Wins:
- **~210ms** de melhoria no tempo de carregamento
- **Eliminação de 3 erros** no console
- **Redução de layout shift** em imagens
- **Melhoria imediata** na experiência do usuário

---

## 📝 Notas de Implementação

- Sempre testar mudanças em ambiente de desenvolvimento primeiro
- Fazer backup antes de alterações críticas no tema
- Monitorar métricas Core Web Vitals após cada deploy
- Considerar A/B testing para mudanças significativas de UX
- Usar Shopify Theme Inspector para debug de performance
- Implementar mudanças gradualmente para identificar impactos