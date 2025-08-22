# 📊 Plano de Otimização de Performance
**Data**: Agosto 2025  
**Análise**: Google PageSpeed Insights  
**URL**: https://rovelab.com  
**URL Repositorio**: https://github.com/RoveLab/ecommerce-shopify-rove-us  
**PageSpeed Insights**: https://pagespeed.web.dev/analysis/https-rovelab-com/fahnrilula?hl=pt&form_factor=desktop 

## 📑 Índice de Otimizações

### Performance
1. [🖼️ Otimização de Imagens]
3. [🚫 Eliminar Recursos Render-Blocking]
4. [📐 Reduzir Layout Shifts]
5. [🔄 Reduzir Reflow Forçado]
6. [🎯 Otimizar LCP]
7. [🌐 Otimizar Árvore de Dependências]
8. [⚡ Reduzir Tempo de Execução JavaScript]

## 1. 🖼️ Otimização de Imagens

### Problemas Identificados:
1. **Imagem Hero Banner** (1.880,7 KiB de economia)
   - `D_BANNER_EG_2_1_86df988e-3dd5-4fd7-97da.gif`
   - Usar formato de vídeo (MP4/WebM) em vez de GIF animado

2. **Imagens de Produtos** (múltiplas economias de 10-20 KiB cada)
   - Várias imagens `SWATCH_PW` podem ser compactadas
   - Logo Shopify pode ser otimizada

### ✅ Correções Necessárias:

#### Converter GIF para Vídeo
- Usar vídeo em vez de um GIF grande em HTML é mais eficiente, pois vídeos são comprimidos de forma muito melhor, resultando em arquivos menores e mais rápidos de carregar.

---

## 2. 🚫 Eliminar Recursos Render-Blocking

### Recursos Bloqueantes:
1. `single-image.css` (1.6 KiB)
2. `animations.min.js` (49.3 KiB)
3. `accelerated-checkout-backwards-compat.css` (2.8 KiB)

### ✅ Correções:

#### A. Defer JavaScript não-crítico
```liquid
<!-- ARQUIVO: sections/goto-products.liquid -->

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

<!-- Carregar resto async -->
<link rel="preload" href="{{ 'single-image.css' | asset_url }}" as="style" onload="this.onload=null;this.rel='stylesheet'">
<noscript><link rel="stylesheet" href="{{ 'single-image.css' | asset_url }}"></noscript>
```

---

## 3. 📐 Reduzir Layout Shifts

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

## 4. 🔄 Reduzir Reflow Forçado (169ms total)

### Scripts causando reflow:
1. `clarity.js` (69ms)
2. `animations.min.js` (85ms)
3. `vendor.min.js` (27ms)

### ✅ Correções:

#### A. Remover:
 - Removendo o arquivo animations.min.js. É aplicado apenas no componente slider. Nesse momento não está sendo usado no tema.

---

## 5. 🎯 Otimizar LCP (Largest Contentful Paint)

### Elemento LCP identificado:
- Imagem do banner principal (`D_BANNER_EG_2_1_86df988e`)

### ✅ Correções:

- Use imagens responsivas, otimizadas e em formatos modernos como WebP ou AVIF.
- Defina width e height para reservar espaço e evitar deslocamentos de layout.
- Priorize o carregamento com fetchpriority="high" e loading="eager".
- Reduza scripts bloqueantes e use CDN para entregar o conteúdo mais rápido ao usuário.
---

## 6. 🌐 Otimizar Árvore de Dependências de Rede

### Critical Path (6.840ms):
1. HTML → 328ms
2. Harmonia Sans → 699ms
3. Client Shop → 341ms
4. Chunk Common → 390ms

### ✅ Correções:

#### Preconnect para recursos críticos
```html
<!-- ARQUIVO: layout/theme.liquid -->
<!-- Adicionar no início do <head> -->

<link rel="preconnect" href="https://cdn.shopify.com" crossorigin>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link rel="dns-prefetch" href="https://www.googletagmanager.com">
```

---

## 7. ⚡ Reduzir Tempo de Execução JavaScript

### Scripts pesados:
1. `animations.min.js` - 891ms
2. `vendor.min.js` - 315ms
3. Google Tag Manager - 631ms

### ✅ Correções:

#### Code Splitting
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

## 📊 Resultados Esperados

Após implementação completa:
- **Redução de ~3MB** no peso total da página
- **Melhoria de ~2.5s** no tempo de carregamento
- **CLS < 0.1** (bom)
- **FCP < 1.8s** (bom)
- **LCP < 2.5s** (bom)
- **Score Performance**: 65+ (verde)

---


| Prioridade | Ordem | Tarefa                                 | Estimativa |
|------------|-------|----------------------------------------|------------|
| HIGH       | 1     | 🖼️ Otimização de Imagens               | Rove       |
| HIGH       | 2     | 🚫 Eliminar Recursos Render-Blocking   | 1h         |
| LOW        | 3     | 📐 Reduzir Layout Shifts (CLS)         | 1h         |
| LOW        | 4     | 🔄 Reduzir Reflow Forçado              | 1h         |
| HIGH       | 5     | 🎯 Otimizar LCP                        | 3h         |
| LOW        | 6     | 🌐 Otimizar Árvore de Dependências     | 2h         |
| HIGH       | 7     | ⚡ Reduzir Tempo de Execução JavaScript | 1h         |
