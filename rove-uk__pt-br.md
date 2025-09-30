# 📊 Plano de Otimização de Performance
**Data**: 30 de setembro de 2025  
**Análise**: Google PageSpeed Insights  
**URL**: https://rovelab.uk
**URL Repositorio**: https://github.com/RoveLab/ecommerce-shopify-rove-uk 
**PageSpeed Insights**: https://pagespeed.web.dev/analysis/https-rovelab-uk/zqofduni06?form_factor=mobile

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


**Não pode ser feito:** Arquivos da Shopify (`accelerated-checkout-backwards-compat.css` e `advanced-switcher.css`).  

**Impacto esperado:** Menor tempo de carregamento percebido pelo usuário.  

---

### 2. Otimização de imagens
**O problema:** Algumas imagens estão em dimensões ou formatos maiores do que o necessário (**~300 KiB de economia**).  

**Arquivos identificados:**  
1. **SWATCH_PW_Krypton_Green** – 284,3 KiB → [link](https://rovelab.uk/cdn/shop/files/SWATCH_PW_Krypton_Green_1ee08ff4-7977-45e5-803b-65a42f458d69.jpg?v=1752525705)  
2. **SWATCH_PW_Silicon_Sand** – 261,0 KiB → [link](https://rovelab.uk/cdn/shop/files/SWATCH_PW_Silicon_Sand_5cb5a981-eca7-4610-bb8a-82efacbc99eb.jpg?v=1752525705)  
3. **M_HERO_BANNER_9_1_1180x1740_crop_center** – 104,6 KiB → [link](https://rovelab.uk/cdn/shop/files/M_HERO_BANNER_9_1_1180x1740_crop_center.jpg?v=1758661597)  
4. **SWATCH_PW_Helium_Cloud** – 234,1 KiB → [link](https://rovelab.uk/cdn/shop/files/SWATCH_PW_Helium_Cloud_924e0025-b7d2-4d80-8a6b-39d3105db749.jpg?v=1752525705)  
5. **m1-2-seat-sofa-loveseat-krypton-green-front-view** – 29,3 KiB → [link](https://rovelab.uk/cdn/shop/files/m1-2-seat-sofa-loveseat-krypton-green-front-view_f67ce228-c5a4-4e77-b46e-fd6d151a865a_770x_crop_center.webp?v=1749242136)  
6. **SWATCH_PW_Hydrongen_Blue** – 183,7 KiB → [link](https://rovelab.uk/cdn/shop/files/SWATCH_PW_Hydrongen_Blue_d45301aa-1f6d-4af1-ba51-1df7444f63d5.jpg?v=1752525705)  
7. **RoveLab_Logo_v.1_512_x_72_px** – 4,6 KiB → [link](https://cdn.shopify.com/s/files/1/0724/4041/4365/files/RoveLab_Logo_v.1_512_x_72_px_2_1309da4c-9d52-4b20-9def-47be80f23b51.png?v=1754584349)  



**O que pode ser feito:**  
- Criar versões menores e responsivas.  
- Converter para WebP ou AVIF.  
- Aplicar compressão otimizada em JPG.  

**Impacto esperado:** Redução significativa no peso total e melhora perceptível no carregamento.  


---

### 3. Imagens fora da tela (lazy loading)
**O problema:** Algumas imagens pequenas de pré-visualização carregam mesmo estando fora da área inicial de exibição.  

**Arquivos identificados:**  
1. [c4e6644749ee4011a625aadf02a823ba.thumbnail.0000000000_690x.jpg](https://rovelab.uk/cdn/shop/files/preview_images/c4e6644749ee4011a625aadf02a823ba.thumbnail.0000000000_690x.jpg?v=1753473261) – 7,5 KiB  
2. [3d72ffe4441f44fcad05279720f76837.thumbnail.0000000000_690x.jpg](https://rovelab.uk/cdn/shop/files/preview_images/3d72ffe4441f44fcad05279720f76837.thumbnail.0000000000_690x.jpg?v=1756934874) – 2,5 KiB


**O que pode ser feito:**  
- Ativar `loading="lazy"`.  

**Impacto esperado:** Possível economia de 10 KiB.

---

### 4. Tamanho excessivo de payload (13,24 MB)
**O problema:** O site carrega **13,24 MB de recursos**, grande parte em imagens e vídeos pesados.  

**Arquivos identificados:**  
1. **c4e6644….HD-1080p-7.2Mbps-52035919.mp4**  
   [Link](https://rovelab.uk/cdn/shop/videos/c/vp/c4e6644749ee4011a625aadf02a823ba/c4e6644749ee4011a625aadf02a823ba.HD-1080p-7.2Mbps-52035919.mp4?v=0) – 9.044,9 KiB  
2. **3d72ffe….HD-1080p-3.3Mbps-56755690.mp4**  
   [Link](https://rovelab.uk/cdn/shop/videos/c/vp/3d72ffe4441f44fcad05279720f76837/3d72ffe4441f44fcad05279720f76837.HD-1080p-3.3Mbps-56755690.mp4?v=0) – 2.935,5 KiB  
3. **SWATCH_PW_Krypton_Green.jpg**  
   [Link](https://rovelab.uk/cdn/shop/files/SWATCH_PW_Krypton_Green_1ee08ff4-7977-45e5-803b-65a42f458d69.jpg?v=1752525705) – 285,8 KiB  
4. **SWATCH_PW_Silicon_Sand.jpg**  
   [Link](https://rovelab.uk/cdn/shop/files/SWATCH_PW_Silicon_Sand_5cb5a981-eca7-4610-bb8a-82efacbc99eb.jpg?v=1752525705) – 262,4 KiB  
5. **SWATCH_PW_Helium_Cloud.jpg**  
   [Link](https://rovelab.uk/cdn/shop/files/SWATCH_PW_Helium_Cloud_924e0025-b7d2-4d80-8a6b-39d3105db749.jpg?v=1752525705) – 235,6 KiB  
6. **SWATCH_PW_Hydrongen_Blue.jpg**  
   [Link](https://rovelab.uk/cdn/shop/files/SWATCH_PW_Hydrongen_Blue_d45301aa-1f6d-4af1-ba51-1df7444f63d5.jpg?v=1752525705) – 185,1 KiB  
7. **m1-2-seat-sofa-loveseat-krypton-green-angled-view.webp**  
   [Link](https://cdn.shopify.com/s/files/1/0724/4041/4365/files/m1-2-seat-sofa-loveseat-krypton-green-angled-view_e67e01b1-ec07-4ba1-955b-e3e80892ccd7.webp?v=1749242136) – 298,8 KiB  


**O que pode ser feito:**  
- Comprimir imagens e vídeos.  
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

### 6. Estabilidade do layout (CLS)  
**O problema:** O relatório identificou mudanças de layout durante o carregamento, causadas principalmente por imagens sem dimensões reservadas, pela barra de anúncio em marquee e pelo carregamento tardio da fonte **Poppins**.  

**Elementos/arquivos identificados:**  
- **Announcement bar** (`.announcement-bar--marquee`) – sem altura fixa definida, gerando deslocamento ao iniciar a animação.  
- **Hero image** (`M_HERO_BANNER_9_1_1180x1740_crop_center.jpg`) – imagem listada como *unsized image element*.  
- **Seção de produtos em destaque** (`shopify-section-featured_collection`) – cards carregam sem espaço pré-reservado.  
- **Redirects Icon** – imagem com `width/height` no HTML, mas sobrescrita pelo CSS.  
- **Fonte Poppins (woff2)** – sem `font-display: swap`, causando troca de fonte após carregamento.  

**O que pode ser feito:**  
- Definir largura, altura ou `aspect-ratio` em imagens (hero, ícones e produtos).  
- Adicionar `font-display: swap` às fontes externas.  
- Reservar altura fixa para o announcement bar.  
- Definir altura mínima para os cards da seção de produtos em destaque.  
- Ajustar CSS para não sobrescrever dimensões declaradas em imagens.  

**Impacto esperado:** Experiência mais estável, sem mudanças bruscas no layout durante o carregamento, resultando em menor pontuação de **CLS** e melhor usabilidade.  


---

<br>

## 📊 Tabela de Prioridade (Ações possíveis)

| Prioridade | Tarefa                                | Impacto esperado | Estimativa de tempo |
|------------|----------------------------------------|------------------|----------------------|
| 🔴 Alta    | Otimização de imagens (Itens 2 e 4)    | Muito alto       | 3–4 horas            |
| 🔴 Alta    | Descoberta de solicitações de LCP (Item 5) | Alto       | 30 min – 1 hora      |
| 🔴 Alta    | Estabilidade do layout (CLS – Item 6) | Alto             | 4–5 horas            |
| 🟠 Média   | Remoção/adiamento de scripts (Item 1) | Médio            | 30 min – 1 hora      |
| 🟡 Baixa   | Lazy loading em imagens (Item 3)      | Baixo            | 30 min – 1 hora      |

---

<br>

## ⚠️ Itens relacionados a apps externos (não ajustáveis)

Esses itens foram reportados pelo PageSpeed, mas **não podem ser otimizados diretamente**, pois os arquivos vêm de apps de terceiros. A única alternativa seria **remover ou substituir os apps**, caso desejado.  

### Cache curto de recursos externos  
- Intelligems – 91 KiB (cache 5 min)
- Facebook Pixel – 117 KiB (cache 0–20 min) 
- Widde - 87 KiB (cache 1 h) 
- Loox (reviews) – 214 KiB (cache 1 dia e 12 h)  
- 9gtb (script externo) – 58 KiB (cache 1 dia)  
- Clarity – 32 KiB (cache 1 dia)  
- Klaviyo – 3 KiB (cache 1 s)  

---

### Fontes externas sem `font-display: swap`  
- Rebuy (`fa-light-300.woff2`) – atraso de ~30 ms  

---

### JavaScript legado (79 KiB)  
- Klaviyo – 17,8 KiB  
- Rebuy – 48,4 KiB
- Facebook Pixel – 12,5 KiB

---

### JavaScript não utilizado (782 KiB)  
- Google Tag Manager – 407,6 KiB	
- Rebuy – 367,0 KiB	
- Converge - 41,8 KiB	
- Widde - 85,9 KiB
- Facebook - 82,7 KiB	
- Loox – 171,2 KiB
- Intelligems - 87,3 KiB	

---

### CSS não utilizado (27 KiB)  
- Rebuy – 19.5 KiB  
- app.css (tema da loja) – 13.2 KiB  

<br>

---

<br><br>

# 📌 Conclusão
Grande parte dos pontos levantados está relacionada a **apps externos**, sem possibilidade de ajuste no Shopify.  
Os principais ganhos de performance virão das **otimizações de imagens, melhorias de layout (CLS) e ajustes em scripts/carregamento inicial**, que podem ser feitos pelo time de desenvolvimento.  

