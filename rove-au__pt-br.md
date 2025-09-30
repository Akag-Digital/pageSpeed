# 📊 Plano de Otimização de Performance
**Data**: 30 de setembro de 2025  
**Análise**: Google PageSpeed Insights  
**URL**: https://rovelab.au  
**URL Repositorio**: https://github.com/RoveLab/ecommerce-shopify-rove-au  
**PageSpeed Insights**: https://pagespeed.web.dev/analysis/https-rovelab-au/mzbpim9uep?form_factor=mobile  

<br>

---

<br>

Este documento reúne os principais pontos identificados no **PageSpeed** e divide em duas categorias:

- **Ações que podem ser feitas pelo desenvolvedor**  
- **Itens relacionados a apps externos** (sem possibilidade de intervenção direta no Shopify)  

<br>

---

<br>

## ✅ Itens que podem ser feitos (desenvolvedor)

### 1. Arquivos bloqueando o carregamento inicial
**O problema:** Alguns arquivos de CSS e JS atrasam a exibição inicial do site, aumentando o tempo para mostrar o conteúdo principal.  

**Arquivos identificados:**  
- `animations.min.js` – pode ser removido se não for essencial.  
- `multi-image.css` – pode ser otimizado para carregar de forma não-bloqueante.  
- `bundle.js` (Intelligems) – confirmar se o app ainda é usado; se não, pode ser removido.  

**Não pode ser feito:** Arquivos da Shopify (`accelerated-checkout-backwards-compat.css` e `native-geo-redirects.min.css`).  

**Impacto esperado:** Menor tempo de carregamento percebido pelo usuário.

---

### 2. Otimização de imagens
**O problema:** Algumas imagens estão em dimensões ou formatos maiores do que o necessário (**~859 KiB de economia**).  

**Arquivos identificados:**  
1. [`SWATCH_PW_Copper_Tan.jpg`](https://rovelab.au/cdn/shop/files/SWATCH_PW_Copper_Tan.jpg?v=1736181995) – 358,9 KiB  
2. [`M_HERO_BANNER_7_1_07a2db7a-f369-4776-b989-f1c724f87ad2_1180x1740_crop_center.jpg`](https://rovelab.au/cdn/shop/files/M_HERO_BANNER_7_1_07a2db7a-f369-4776-b989-f1c724f87ad2_1180x1740_crop_center.jpg?v=1758549532) – 202,2 KiB 
3. [`SWATCH_PW_Krypton_Green.jpg`](https://rovelab.au/cdn/shop/files/SWATCH_PW_Krypton_Green.jpg?v=1736181995) – 284,3 KiB 
4. [`SWATCH_PW_Platinum_Grey.jpg`](https://rovelab.au/cdn/shop/files/SWATCH_PW_Platinum_Grey.jpg?v=1736181994) – 261,2 KiB
5. [`SWATCH_PW_Silicon_Sand.jpg`](https://rovelab.au/cdn/shop/files/SWATCH_PW_Silicon_Sand.jpg?v=1736181995) – 261,0 KiB
6. [`SWATCH_PW_Helium_Cloud.jpg`](https://rovelab.au/cdn/shop/files/SWATCH_PW_Helium_Cloud.jpg?v=1736181995) – 234,1 KiB
7. [`M1_2-Seat-Sofa-Loveseat_PW_Copper_Tan_1_770x_crop_center.jpg`](https://rovelab.au/cdn/shop/files/M1_2-Seat-Sofa-Loveseat_PW_Copper_Tan_1_770x_crop_center.jpg?v=1746622571) – 25,1 KiB
8. [`m1-modular-sectional-sofa-krypton-green-front-view_770x_crop_center.webp`]() – 22,6 KiB 
9. [`SWATCH_PW_Hydrongen_Blue_3.jpg`](https://rovelab.au/cdn/shop/files/SWATCH_PW_Hydrongen_Blue_3.jpg?v=1749077562) – 183,7 KiB
10. [`M1_3-Seat-Sofa_PW_Helium_Cloud_1_770x_crop_center.jpg`](https://rovelab.au/cdn/shop/files/M1_3-Seat-Sofa_PW_Helium_Cloud_1_770x_crop_center.jpg?v=1745536953) – 14,7 KiB

**O que pode ser feito:**  
- Criar versões menores e responsivas.  
- Converter para WebP ou AVIF.  
- Aplicar compressão otimizada em JPG.  

**Impacto esperado:** Redução significativa no peso total e melhora perceptível no carregamento.  


---

### 3. Imagens fora da tela (lazy loading)
**O problema:** Algumas imagens pequenas de pré-visualização carregam mesmo estando fora da área inicial de exibição.  

**Arquivos identificados:**  
- [2499e01a7aba4bbbb47761687a94..._690x.jpg](https://rovelab.au/cdn/shop/files/preview_images/2499e01a7aba4bbbb47761687a942e2c.thumbnail.0000000000_690x.jpg?v=1744201916) – 7.5 KiB  
- [fdb4117785ab4b4aa5439e1c63eeb5..._690x.jpg](https://rovelab.au/cdn/shop/files/preview_images/fdb4117785ab4b4aa5439e1c63eeb5f8.thumbnail.0000000000_690x.jpg?v=1744201992) – 2.3 KiB  

**O que pode ser feito:**  
- Ativar `loading="lazy"`.  

**Impacto esperado:** Mínimo (~10 KiB), mas evita download desnecessário logo no carregamento inicial.  

---

### 4. Tamanho excessivo de payload (22,92 MB)
**O problema:** O site carrega **22,92 MB de recursos**, grande parte em imagens e vídeos pesados.  

**Arquivos identificados:**  
1. [2499e01a7aba4bbbb47761687a942e2c.HD-1080p-7.2Mbps-45643103.mp4](https://rovelab.au/cdn/shop/videos/c/vp/2499e01a7aba4bbbb47761687a942e2c/2499e01a7aba4bbbb47761687a942e2c.HD-1080p-7.2Mbps-45643103.mp4?v=0) — 9.044,3 KiB  
2. [fdb4117785ab4b4aa5439e1c63eeb5f8.HD-1080p-7.2Mbps-45643177.mp4](https://rovelab.au/cdn/shop/videos/c/vp/fdb4117785ab4b4aa5439e1c63eeb5f8/fdb4117785ab4b4aa5439e1c63eeb5f8.HD-1080p-7.2Mbps-45643177.mp4?v=0) — 6.380,4 KiB  
3. [SWATCH_PW_Copper_Tan.jpg](https://rovelab.au/cdn/shop/files/SWATCH_PW_Copper_Tan.jpg?v=1736181995) — 360,3 KiB  
4. [SWATCH_PW_Krypton_Green.jpg](https://rovelab.au/cdn/shop/files/SWATCH_PW_Krypton_Green.jpg?v=1736181995) — 285,8 KiB  
5. [D_BANNER_SPRING_SALE_AU_1_2800x1200_crop_center.jpg](https://rovelab.au/cdn/shop/files/D_BANNER_SPRING_SALE_AU_1_2800x1200_crop_center.jpg?v=1758549533) — 272,6 KiB  
6. [SWATCH_PW_Platinum_Grey.jpg](https://rovelab.au/cdn/shop/files/SWATCH_PW_Platinum_Grey.jpg?v=1736181994) — 262,7 KiB  
7. [SWATCH_PW_Silicon_Sand.jpg](https://rovelab.au/cdn/shop/files/SWATCH_PW_Silicon_Sand.jpg?v=1736181995) — 262,5 KiB  
8. [SWATCH_PW_Helium_Cloud.jpg](https://rovelab.au/cdn/shop/files/SWATCH_PW_Helium_Cloud.jpg?v=1736181995) — 235,6 KiB  
9. [M_HERO_BANNER_7_1_07a2db7a-f369-4776-b989-f1c724f87ad2_1180x1740_crop_center.jpg](https://rovelab.au/cdn/shop/files/M_HERO_BANNER_7_1_07a2db7a-f369-4776-b989-f1c724f87ad2_1180x1740_crop_center.jpg?v=1758549532) — 203,7 KiB  


**O que pode ser feito:**  
- Comprimir imagens.  
- Criar versões responsivas.  
- Converter para WebP/AVIF.  

**Impacto esperado:** Redução significativa do peso total da página.  

---

### 5. Descoberta de solicitações de LCP

**O problema:**  
A imagem principal de LCP (Largest Contentful Paint) está configurada corretamente com `fetchpriority="high"` e `loading="eager"`. No entanto, o relatório sinaliza que a detecção poderia ser otimizada para garantir que o navegador identifique essa imagem como crítica de forma ainda mais imediata. É possível melhorar isso adicionando *preload* no bloco de imagem no Liquid.

**Elemento identificado:**

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

## 📊 Tabela de Prioridade (Ações possíveis)

| Prioridade | Tarefa                                | Impacto esperado | Estimativa de tempo |
|------------|----------------------------------------|------------------|----------------------|
| 🔴 Alta    | Otimização de imagens (Itens 2 e 4)    | Muito alto       | 4-5 horas            |
| 🔴 Alta    | Descoberta de solicitações de LCP (Item 5) | Alto         | 30 min – 1 hora      |
| 🟠 Média   | Remoção/adiamento de scripts (Item 1) | Médio            | 30 min – 1 hora      |
| 🟡 Baixa   | Lazy loading em imagens (Item 3)      | Baixo            | 1-2 horas            |

---

<br>

## ⚠️ Itens relacionados a apps externos (não ajustáveis)

Esses itens foram reportados pelo PageSpeed, mas **não podem ser otimizados diretamente**, pois os arquivos vêm de apps de terceiros. A única alternativa seria **remover ou substituir os apps**, caso desejado.  

### Cache curto de recursos externos  
- Intelligems – 180 KiB (cache 5 min)  
- Facebook Pixel – 117 KiB (cache 0–20 min) 
- Widde - 87 KiB (cache 1 hora)
- Loox (reviews) – 214 KiB (cache 1 dia e 12 h)  
- 9gtb (script externo) – 58 KiB (cache 1 dia)  


---

### Fontes externas sem `font-display: swap`  
- Rebuy (`fa-light-300.woff2`) – atraso de ~110 ms  

---

### JavaScript legado (77 KiB)  
- Klaviyo – 17.8 KiB  
- Rebuy – 58.7 KiB  
- Facebook Pixel – 12.5 KiB  
- TikTok – 17.5 KiB  

---

### JavaScript não utilizado (1,43 MB)  
- Google Tag Manager – 533,8 KiB 
- Rebuy – 367,0 KiB
- TikTok – 199,1 KiB
- Loox – 171,2 KiB

---

### CSS não utilizado (28 KiB)  
- Rebuy – 19.5 KiB  
- app.css (tema da loja) – 13.2 KiB  

<br>

---

<br><br>

# 📌 Conclusão
Grande parte dos pontos levantados está relacionada a **apps externos**, sem possibilidade de ajuste no Shopify.  
Os principais ganhos de performance virão das **otimizações de imagens, melhorias de layout (CLS) e ajustes em scripts/carregamento inicial**, que podem ser feitos pelo time de desenvolvimento.  

