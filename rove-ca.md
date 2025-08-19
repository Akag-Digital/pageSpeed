# 📊 Plano de Otimização de Performance
**Data**: Agosto 2025  
**Análise**: Google PageSpeed Insights  
**URL**: https://rovelab.ca  
**URL Repositorio**: https://github.com/RoveLab/ecommerce-shopify-rove-ca  
**PageSpeed Insights**: https://pagespeed.web.dev/analysis/https-rovelab-ca/eaj3j5vh4u?hl=pt&form_factor=desktop  


## 📑 Índice de Otimizações

### Performance
1. [🖼️ Otimização de Imagens](#1--otimização-de-imagens-economia-2131-kib)
2. [🔤 Otimização de Fontes](#2--otimização-de-fontes-economia-220ms)
3. [🚫 Eliminar Recursos Render-Blocking](#3--eliminar-recursos-render-blocking-economia-30ms)
4. [📐 Reduzir Layout Shifts](#4--reduzir-layout-shifts-cls-0148)
5. [🔄 Reduzir Reflow Forçado](#5--reduzir-reflow-forçado-169ms-total)
6. [🎯 Otimizar LCP](#6--otimizar-lcp-largest-contentful-paint)
7. [🌐 Otimizar Árvore de Dependências](#7--otimizar-árvore-de-dependências-de-rede)
8. [⚡ Reduzir Tempo de Execução JavaScript](#8--reduzir-tempo-de-execução-javascript-25s-economia)
9. [🗑️ Remover JavaScript Não Usado](#9-️-remover-javascript-não-usado-935-kib)

### Práticas Recomendadas
10. [🍪 Cookies de Terceiros](#10--cookies-de-terceiros)
11. [❌ Erros no Console do Navegador](#11--erros-no-console-do-navegador)
12. [⚠️ Issues do Chrome DevTools](#12-️-issues-do-chrome-devtools)
13. [📐 Imagens com Proporção Incorreta](#13--imagens-com-proporção-incorreta)

---

## 📈 Métricas Atuais
- **Performance Score**: Necessita melhoria
- **Economia Potencial**: ~2.131 KiB em imagens + 935 KiB em JavaScript
- **Tempo de Bloqueio**: ~30ms em requisições render-blocking
- **Layout Shift**: 0.148 CLS
- **Erros no Console**: 6 erros (CORS, SyntaxError, Failed Resources)
- **Cookies de Terceiros**: 1 (Snapchat - será bloqueado)
- **Imagens Distorcidas**: 1 banner principal com proporção incorreta

---

## 1. 🖼️ Otimização de Imagens (Economia: 2.131 KiB)

### Problemas Identificados:
1. **Imagem Hero Banner** (1.880,7 KiB de economia)
   - `D_BANNER_EG_2_1_86df988e-3dd5-4fd7-97da.gif`
   - Usar formato de vídeo (MP4/WebM) em vez de GIF animado

2. **Imagens de Produtos** (múltiplas economias de 10-20 KiB cada)
   - Várias imagens `SWATCH_PW` podem ser compactadas
   - Logo Shopify pode ser otimizada

### ✅ Correções Necessárias:

#### A. Converter GIF para Vídeo
```liquid
<!-- ARQUIVO: sections/goto-hero-banner.liquid -->
<!-- LINHA: ~70 (onde está a imagem) -->

<!-- ANTES -->
{%- render 'responsive-image', class: 'product-primary-image', image: featured_media, sizes: sizes -%}

<!-- DEPOIS -->
{%- if featured_media.media_type == 'video' or featured_media contains '.gif' -%}
  <video autoplay loop muted playsinline loading="lazy">
    <source src="{{ featured_media | replace: '.gif', '.mp4' | asset_url }}" type="video/mp4">
    <source src="{{ featured_media | replace: '.gif', '.webm' | asset_url }}" type="video/webm">
  </video>
{%- else -%}
  {%- render 'responsive-image', class: 'product-primary-image', image: featured_media, sizes: sizes -%}
{%- endif -%}
```

#### B. Implementar Responsive Images
```liquid
<!-- ARQUIVO: snippets/responsive-image.liquid -->
<!-- Adicionar srcset com múltiplas resoluções -->

<img 
  srcset="{{ image | img_url: '375x' }} 375w,
          {{ image | img_url: '750x' }} 750w,
          {{ image | img_url: '1125x' }} 1125w,
          {{ image | img_url: '1500x' }} 1500w"
  sizes="(max-width: 768px) 100vw, 50vw"
  loading="lazy"
  decoding="async"
  fetchpriority="{{ priority | default: 'auto' }}"
>
```

---

## 2. 🔤 Otimização de Fontes (Economia: 220ms)

### Problema:
- Fonte `fa-light-300.woff2` do Rebuy Engine causando delay

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
</style>
```

---

## 3. 🚫 Eliminar Recursos Render-Blocking (Economia: 30ms)

### Recursos Bloqueantes:
1. `single-image.css` (1.6 KiB)
2. `animations.min.js` (49.3 KiB)
3. `accelerated-checkout-backwards-compat.css` (2.8 KiB)

### ✅ Correções:

#### A. Defer JavaScript não-crítico
```liquid
<!-- ARQUIVO: sections/goto-products.liquid -->
<!-- LINHAS: 3-5 -->

<!-- ANTES -->
{{ 'preorder.js' | asset_url | script_tag }}
{{ 'popup-image-dimension.js' | asset_url | script_tag }}
{{ 'switch-image-option-group.js' | asset_url | script_tag }}

<!-- DEPOIS -->
<script src="{{ 'preorder.js' | asset_url }}" defer></script>
<script src="{{ 'popup-image-dimension.js' | asset_url }}" defer></script>
<script src="{{ 'switch-image-option-group.js' | asset_url }}" defer></script>
```

#### B. Inline Critical CSS
```liquid
<!-- ARQUIVO: layout/theme.liquid -->
<!-- Adicionar no <head> -->

<style>
  /* Critical CSS - inline apenas o necessário para above-the-fold */
  .hero-banner-goto { min-height: 100vh; }
  .product-card { display: block; }
  /* ... outros estilos críticos ... */
</style>

<!-- Carregar resto async -->
<link rel="preload" href="{{ 'single-image.css' | asset_url }}" as="style" onload="this.onload=null;this.rel='stylesheet'">
<noscript><link rel="stylesheet" href="{{ 'single-image.css' | asset_url }}"></noscript>
```

---

## 4. 📐 Reduzir Layout Shifts (CLS: 0.148)

### Elementos com Shift:
1. Product cards do M1 SOFA (0.121)
2. Imagens do banner (0.014)
3. Menu de navegação (0.009)

### ✅ Correções:

#### A. Definir aspect-ratio para imagens
```css
/* ARQUIVO: assets/goto-products.css */
/* Adicionar */

.product-featured-image img {
  aspect-ratio: 1 / 1;
  width: 100%;
  height: auto;
  object-fit: cover;
}

.hero-banner-goto__image {
  aspect-ratio: 16 / 9;
  min-height: 50vh; /* Previne collapse */
}

@media (max-width: 768px) {
  .hero-banner-goto__image {
    aspect-ratio: 4 / 5;
  }
}
```

#### B. Reservar espaço para elementos dinâmicos
```css
/* ARQUIVO: assets/global.css */

.announcement-bar {
  min-height: 40px; /* Previne shift quando carrega */
}

.product-card-swatches {
  min-height: 60px; /* Espaço reservado para swatches */
}
```

---

## 5. 🔄 Reduzir Reflow Forçado (169ms total)

### Scripts causando reflow:
1. `clarity.js` (69ms)
2. `animations.min.js` (85ms)
3. `vendor.min.js` (27ms)

### ✅ Correções:

#### A. Otimizar animações
```javascript
// ARQUIVO: assets/animations.min.js
// Use requestAnimationFrame para batching

// ANTES
element.style.transform = 'translateX(' + x + 'px)';
element.style.opacity = opacity;

// DEPOIS
requestAnimationFrame(() => {
  element.style.cssText = `
    transform: translateX(${x}px);
    opacity: ${opacity};
  `;
});
```

#### B. Debounce scroll events
```javascript
// ARQUIVO: assets/global.js ou theme.js
// Adicionar debounce para scroll

let scrollTimeout;
window.addEventListener('scroll', () => {
  if (scrollTimeout) {
    window.cancelAnimationFrame(scrollTimeout);
  }
  
  scrollTimeout = window.requestAnimationFrame(() => {
    // Seu código de scroll aqui
  });
}, { passive: true });
```

---

## 6. 🎯 Otimizar LCP (Largest Contentful Paint)

### Elemento LCP identificado:
- Imagem do banner principal (`D_BANNER_EG_2_1_86df988e`)

### ✅ Correções:

```liquid
<!-- ARQUIVO: sections/goto-hero-banner.liquid -->
<!-- Adicionar fetchpriority e preload -->

{% if section.settings.image %}
  <link rel="preload" as="image" href="{{ section.settings.image | img_url: '1920x' }}" fetchpriority="high">
{% endif %}

<img 
  src="{{ section.settings.image | img_url: '1920x' }}"
  fetchpriority="high"
  loading="eager" <!-- Não usar lazy para LCP -->
  decoding="async"
>
```

---

## 7. 🌐 Otimizar Árvore de Dependências de Rede

### Critical Path (6.840ms):
1. HTML → 328ms
2. Harmonia Sans → 699ms
3. Client Shop → 341ms
4. Chunk Common → 390ms

### ✅ Correções:

#### A. Preconnect para recursos críticos
```html
<!-- ARQUIVO: layout/theme.liquid -->
<!-- Adicionar no início do <head> -->

<link rel="preconnect" href="https://cdn.shopify.com" crossorigin>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link rel="dns-prefetch" href="https://www.googletagmanager.com">
```

#### B. Resource Hints
```liquid
<!-- ARQUIVO: layout/theme.liquid -->

{% comment %} Prefetch recursos que serão usados em breve {% endcomment %}
<link rel="prefetch" href="{{ 'product-card.js' | asset_url }}">
<link rel="prefetch" href="{{ 'cart-drawer.js' | asset_url }}">
```

---

## 8. ⚡ Reduzir Tempo de Execução JavaScript (2.5s economia)

### Scripts pesados:
1. `animations.min.js` - 891ms
2. `vendor.min.js` - 315ms
3. Google Tag Manager - 631ms

### ✅ Correções:

#### A. Lazy Load scripts não-críticos
```javascript
// ARQUIVO: assets/theme.js
// Adicionar lazy loading condicional

// Carregar GTM apenas após interação
let gtmLoaded = false;
const loadGTM = () => {
  if (!gtmLoaded) {
    const script = document.createElement('script');
    script.src = 'https://www.googletagmanager.com/gtm.js?id=GTM-5GQRVBXW';
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

#### B. Code Splitting
```javascript
// ARQUIVO: assets/landing-product-switcher.js
// Usar dynamic imports

// ANTES
import { convertSchemaToHtml } from '../assets/rich-text-renderer.js';

// DEPOIS
const loadRichTextRenderer = async () => {
  const module = await import('../assets/rich-text-renderer.js');
  return module.convertSchemaToHtml;
};
```

---

## 9. 🗑️ Remover JavaScript Não Usado (935 KiB)

### Bibliotecas com código não usado:
1. Rebuy Engine - 366 KiB (251 KiB não usado)
2. Google Tag Manager - 537 KiB (205 KiB não usado)
3. Klaviyo - 127 KiB (66 KiB não usado)

### ✅ Correções:

#### A. Conditional Loading
```liquid
<!-- ARQUIVO: layout/theme.liquid -->

{% comment %} Carregar Rebuy apenas em páginas de produto {% endcomment %}
{% if template contains 'product' %}
  <script src="https://rebuyengine.com/js/global.js" defer></script>
{% endif %}

{% comment %} Klaviyo apenas após consentimento {% endcomment %}
<script>
  if (window.cookieConsent && window.cookieConsent.marketing) {
    const klaviyoScript = document.createElement('script');
    klaviyoScript.src = 'https://static.klaviyo.com/...';
    klaviyoScript.defer = true;
    document.head.appendChild(klaviyoScript);
  }
</script>
```

#### B. Tree Shaking para módulos próprios
```javascript
// webpack.config.js ou vite.config.js
// Adicionar configuração de tree shaking

module.exports = {
  optimization: {
    usedExports: true,
    sideEffects: false,
    minimize: true,
    minimizer: [
      new TerserPlugin({
        terserOptions: {
          compress: {
            dead_code: true,
            drop_console: true,
            drop_debugger: true,
            unused: true
          }
        }
      })
    ]
  }
};
```

---

## 🎯 Prioridades de Implementação

### 🔴 Alta Prioridade (Impacto Imediato)
1. **Converter GIF para vídeo** - 1.8MB de economia
2. **Defer JavaScript** - 30ms de melhoria no blocking time
3. **Implementar font-display: swap** - 220ms de melhoria
4. **Corrigir erros de console** - Melhora estabilidade e UX
5. **Corrigir proporções de imagem** - Reduz CLS

### 🟡 Média Prioridade
6. **Otimizar imagens com srcset**
7. **Adicionar aspect-ratio CSS**
8. **Implementar lazy loading condicional**
9. **Migrar cookies de terceiros** - Preparar para mudanças do Chrome
10. **Implementar consent management** - Compliance e privacidade

### 🟢 Baixa Prioridade (Melhorias incrementais)
11. **Tree shaking**
12. **Code splitting avançado**
13. **Otimizações de animação**
14. **Implementar monitoring de erros (Sentry)**

---

## 📊 Resultados Esperados

Após implementação completa:
- **Redução de ~3MB** no peso total da página
- **Melhoria de ~2.5s** no tempo de carregamento
- **CLS < 0.1** (bom)
- **FCP < 1.8s** (bom)
- **LCP < 2.5s** (bom)
- **Score Performance**: 70+ (verde)

---

## 10. 🍪 Cookies de Terceiros

### Problema Identificado:
- **Snapchat Analytics** (`sc_at`) usando cookies de terceiros
- Chrome está migrando para bloquear cookies de terceiros

### ✅ Correções:

#### A. Migrar para First-Party Cookies
```javascript
// ARQUIVO: layout/theme.liquid
// Substituir Snapchat Pixel por proxy first-party

// Criar proxy endpoint no seu servidor
fetch('/api/analytics/snapchat', {
  method: 'POST',
  body: JSON.stringify({
    event: 'PageView',
    data: {
      url: window.location.href,
      referrer: document.referrer
    }
  }),
  credentials: 'same-origin' // Usa cookies first-party
});
```

#### B. Implementar Consent Management
```liquid
<!-- ARQUIVO: layout/theme.liquid -->
<!-- Adicionar consent management -->

<script>
  // Só carregar Snapchat após consentimento
  function loadSnapchatPixel() {
    if (window.cookieConsent?.analytics) {
      !function(e,t,n,s,u,a){...}(window,document,'script',
        'https://sc-static.net/scevent.min.js');
      
      snaptr('init', 'YOUR_PIXEL_ID');
      snaptr('track', 'PageView');
    }
  }
  
  // Aguardar consentimento do usuário
  window.addEventListener('cookieconsent:accepted', loadSnapchatPixel);
</script>
```

---

## 11. ❌ Erros no Console do Navegador

### Erros Identificados:
1. **CORS Error** - Klaviyo sessions API
2. **SyntaxError** - Token inválido na linha 3106
3. **Failed Resources** - Gorgias chat e Shopify apps

### ✅ Correções:

#### A. Corrigir CORS da Klaviyo
```liquid
<!-- ARQUIVO: layout/theme.liquid -->
<!-- Adicionar headers CORS apropriados -->

<script>
  // Usar Klaviyo via server-side proxy para evitar CORS
  async function trackKlaviyoEvent(eventData) {
    try {
      await fetch('/apps/klaviyo-proxy/track', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(eventData)
      });
    } catch (error) {
      console.warn('Klaviyo tracking failed:', error);
      // Fail silently - não bloquear UX
    }
  }
</script>
```

#### B. Corrigir SyntaxError na linha 3106
```liquid
<!-- ARQUIVO: Verificar arquivo que gera linha 3106 -->
<!-- Procurar por universal-script -->

<!-- ANTES (provável erro) -->
<script>
  const data = {{ product.metafields.custom.data }};
</script>

<!-- DEPOIS (corrigido) -->
<script>
  const data = {{ product.metafields.custom.data | json }};
  // Sempre usar filter json para dados do Liquid
</script>
```

#### C. Handle Failed Resources Gracefully
```javascript
// ARQUIVO: assets/theme.js
// Adicionar fallback para recursos que falham

// Gorgias Chat fallback
window.addEventListener('error', function(e) {
  if (e.target.src && e.target.src.includes('gorgias.chat')) {
    console.warn('Gorgias chat failed to load');
    // Remover elemento quebrado
    e.target.remove();
    // Opcionalmente, mostrar chat alternativo
    document.querySelector('.chat-widget')?.classList.add('fallback-mode');
  }
}, true);

// Shop.app fallback
if (window.Shop?.app) {
  window.Shop.app.onerror = function() {
    console.warn('Shop.app failed, using fallback');
    return true; // Previne erro de propagar
  };
}
```

---

## 12. ⚠️ Issues do Chrome DevTools

### Problema:
- Cookie de terceiros do Snapchat será bloqueado

### ✅ Correções:

#### A. Implementar SameSite Cookie Policy
```javascript
// ARQUIVO: Configuração do servidor/app
// Adicionar SameSite aos cookies

document.cookie = "analytics_id=123; SameSite=Strict; Secure";
// ou
document.cookie = "session_id=456; SameSite=Lax; Secure";
```

#### B. Migrar para alternativas sem cookies
```javascript
// ARQUIVO: assets/analytics.js
// Usar localStorage + fingerprinting em vez de cookies

const getVisitorId = () => {
  let visitorId = localStorage.getItem('visitor_id');
  
  if (!visitorId) {
    // Gerar ID único sem cookies
    visitorId = crypto.randomUUID();
    localStorage.setItem('visitor_id', visitorId);
  }
  
  return visitorId;
};

// Enviar para analytics
fetch('/api/analytics/track', {
  method: 'POST',
  headers: {
    'X-Visitor-ID': getVisitorId()
  }
});
```

---

## 13. 📐 Imagens com Proporção Incorreta

### Problema Identificado:
- Banner `M_BANNER_EOS_6_1` 
  - Exibido: 1180 x 1740 (ratio 0.68)
  - Real: 20 x 29 (ratio 0.69)
  - Pequena distorção causando layout shift

### ✅ Correções:

#### A. Definir Aspect Ratio Correto
```css
/* ARQUIVO: assets/goto-hero-banner.css */
/* Adicionar aspect-ratio específico */

.hero-banner-goto__image img {
  aspect-ratio: 20 / 29; /* Proporção real da imagem */
  width: 100%;
  height: auto;
  object-fit: cover; /* Previne distorção */
}

/* Para diferentes breakpoints */
@media (min-width: 768px) {
  .hero-banner-goto__image img {
    aspect-ratio: 1180 / 1740; /* Ajustar para desktop */
  }
}
```

#### B. Usar Picture Element para Diferentes Proporções
```liquid
<!-- ARQUIVO: sections/goto-hero-banner.liquid -->
<!-- Substituir img simples por picture element -->

<picture>
  <!-- Mobile: proporção 20/29 -->
  <source 
    media="(max-width: 767px)"
    srcset="{{ image | img_url: '375x543' }} 375w,
            {{ image | img_url: '750x1087' }} 750w"
    width="375"
    height="543">
  
  <!-- Desktop: proporção 1180/1740 -->
  <source 
    media="(min-width: 768px)"
    srcset="{{ image | img_url: '1180x1740' }} 1x,
            {{ image | img_url: '2360x3480' }} 2x"
    width="1180"
    height="1740">
  
  <!-- Fallback -->
  <img 
    src="{{ image | img_url: '1180x1740' }}"
    width="1180"
    height="1740"
    alt="{{ image.alt | escape }}"
    loading="eager"
    fetchpriority="high">
</picture>
```

#### C. Validar Todas as Imagens
```javascript
// ARQUIVO: assets/image-validator.js
// Script para validar proporções em desenvolvimento

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
        
        // Tolerância de 5% para diferença de proporção
        if (Math.abs(displayedRatio - naturalRatio) > 0.05) {
          console.warn('Proporção incorreta detectada:', {
            src: this.src,
            displayed: `${displayed.width}x${displayed.height} (${displayedRatio.toFixed(2)})`,
            natural: `${natural.width}x${natural.height} (${naturalRatio.toFixed(2)})`
          });
          
          // Adicionar borda vermelha em dev
          this.style.border = '2px solid red';
        }
      });
    });
  });
}
```

#### D. Implementar Lazy Sizes para Proporções Automáticas
```html
<!-- ARQUIVO: layout/theme.liquid -->
<!-- Adicionar lazysizes com aspectratio plugin -->

<script src="https://cdnjs.cloudflare.com/ajax/libs/lazysizes/5.3.2/lazysizes.min.js" async></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/lazysizes/5.3.2/plugins/aspectratio/ls.aspectratio.min.js" async></script>

<!-- Usar data-aspectratio -->
<img 
  class="lazyload"
  data-aspectratio="1180/1740"
  data-sizes="auto"
  data-src="{{ image | img_url: '1180x1740' }}"
  alt="">
```

---

## 🔧 Ferramentas Recomendadas

1. **Squoosh.app** - Otimização de imagens
2. **FFmpeg** - Conversão GIF → MP4/WebM
3. **Webpack Bundle Analyzer** - Análise de bundles
4. **Chrome DevTools Coverage** - Identificar código não usado
5. **Lighthouse CI** - Monitoramento contínuo
6. **CookieYes/Cookiebot** - Gerenciamento de consentimento
7. **Sentry** - Monitoramento de erros JavaScript

---

## 📝 Notas de Implementação

- Sempre testar mudanças em ambiente de staging primeiro
- Fazer backup antes de alterações críticas
- Monitorar métricas após cada deploy
- Considerar A/B testing para mudanças significativas

---

## 🚀 Quick Wins - Ações Imediatas

### Pode implementar AGORA (< 30 minutos):

1. **Font Display Swap**
   ```css
   /* layout/theme.liquid - adicionar no <head> */
   @font-face { font-display: swap; }
   ```

2. **Defer JavaScript**
   ```html
   <!-- Trocar script_tag por script defer em goto-products.liquid -->
   <script src="{{ 'arquivo.js' | asset_url }}" defer></script>
   ```

3. **Aspect Ratio CSS**
   ```css
   /* assets/goto-hero-banner.css */
   img { aspect-ratio: 1180/1740; }
   ```

4. **Fix Console Errors**
   ```liquid
   <!-- Adicionar filter json em variáveis Liquid -->
   {{ product.metafields | json }}
   ```

5. **Preconnect Domains**
   ```html
   <!-- layout/theme.liquid -->
   <link rel="preconnect" href="https://cdn.shopify.com">
   ```

### Impacto Esperado das Quick Wins:
- **~500ms** de melhoria no tempo de carregamento
- **Redução de 0.05 CLS** no layout shift
- **Eliminação de 6 erros** no console
- **Melhoria imediata** na experiência do usuário