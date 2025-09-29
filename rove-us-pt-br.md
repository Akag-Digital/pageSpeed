# 📊 Plano de Otimização de Performance
**Data**: 29 de setembro de 2025  
**Análise**: Google PageSpeed Insights  
**URL**: https://rovelab.com 
**URL Repositorio**: https://github.com/RoveLab/ecommerce-shopify-rove-us  
**PageSpeed Insights**: https://pagespeed.web.dev/analysis/https-rovelab-com/2v6pnjmpoh?form_factor=mobile

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
**O problema:** Algumas imagens estão em dimensões ou formatos maiores do que o necessário (**~607 KiB de economia**).  

**Arquivos identificados:**  
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

**O que pode ser feito:**  
- Criar versões menores e responsivas.  
- Converter para WebP ou AVIF.  
- Aplicar compressão otimizada em JPG.  

**Impacto esperado:** Redução significativa no peso total e melhora perceptível no carregamento.  


---

### 3. Imagens fora da tela (lazy loading)
**O problema:** Algumas imagens pequenas de pré-visualização carregam mesmo estando fora da área inicial de exibição.  

**Arquivos identificados:**  
- [ba917f7….thumbnail.000…_690x.jpg](https://rovelab.com/cdn/shop/files/preview_images/ba917f75f7b74f298c20755bea863f83.thumbnail.0000000000_690x.jpg?v=1749666405) – 7.5 KiB  
- [f447ae2….thumbnail.000…_690x.jpg](https://rovelab.com/cdn/shop/files/preview_images/f447ae2b31624113b77e55f4c8761400.thumbnail.0000000000_690x.jpg?v=1749666235) – 2.5 KiB  

**O que pode ser feito:**  
- Ativar `loading="lazy"`.  

**Impacto esperado:** Mínimo (~10 KiB), mas evita download desnecessário logo no carregamento inicial.  

---

### 4. Tamanho excessivo de payload (12,47 MB)
**O problema:** O site carrega **12,47 MB de recursos**, grande parte em imagens pesadas.  

**Arquivos identificados:**  
1. Vídeo – [ba917f75f7b74f298c20755bea863f83.HD-1080p-3.3Mbps-49220334.mp4](https://rovelab.com/cdn/shop/videos/c/vp/ba917f75f7b74f298c20755bea863f83/ba917f75f7b74f298c20755bea863f83.HD-1080p-3.3Mbps-49220334.mp4?v=0) (**3.629,5 KiB**)  
2. Vídeo – [f447ae2b31624113b77e55f4c8761400.HD-1080p-3.3Mbps-49220206.mp4](https://rovelab.com/cdn/shop/videos/c/vp/f447ae2b31624113b77e55f4c8761400/f447ae2b31624113b77e55f4c8761400.HD-1080p-3.3Mbps-49220206.mp4?v=0) (**2.935,0 KiB**)  
3. Imagem – [M_HERO_BANNER_5_2_1_1180x1740_crop_center.jpg](https://rovelab.com/cdn/shop/files/M_HERO_BANNER_5_2_1_1180x1740_crop_center.jpg?v=1758744258) (**147,2 KiB**)  



**O que pode ser feito:**  
- Avaliar compressão ou substituição dos vídeos (.mp4) por versões otimizadas/mais leves.  
- Revisar a real necessidade de carregar vídeos em **1080p** no carregamento inicial.  
- Otimizar a imagem de banner para formatos modernos (WebP/AVIF) e versões responsivas.   

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

## 📊 Tabela de Prioridade (Ações possíveis)

| Prioridade | Tarefa                                | Impacto esperado | Estimativa de tempo |
|------------|----------------------------------------|------------------|----------------------|
| 🔴 Alta    | Otimização de imagens e vídeos (Itens 2 e 4)    | Muito alto       | 3–4 horas            |
| 🔴 Alta    | Descoberta de solicitações de LCP (Item 5) | Alto             | 30 min – 1 hora          |
| 🟠 Média   | Remoção/adiamento de scripts (Item 1) | Médio            | 30 min – 1 hora      |
| 🟡 Baixa   | Lazy loading em imagens (Item 3)      | Baixo            | 30 min – 1 hora      |

---

<br>

## ⚠️ Itens relacionados a apps externos (não ajustáveis)

Esses itens foram reportados pelo PageSpeed, mas **não podem ser otimizados diretamente**, pois os arquivos vêm de apps de terceiros. A única alternativa seria **remover ou substituir os apps**, caso desejado.  


- **Cache de recursos externos**  
  - Intelligems (180 KiB – cache 5 min)  
  - Facebook Pixel (117 KiB – cache 0–20 min)  
  - Loox (214 KiB – cache 1d12h)  
  - Snapchat Analytics (24 KiB – cache 10 min)  

- **JavaScript legado**  
  - Klaviyo (17,8 KiB)  
  - Rebuy (58,7 KiB)  
  - Facebook Pixel (12,5 KiB)  
  - TikTok (17,5 KiB)  

- **JavaScript não utilizado**  
  - Google Tag Manager (715.1 KiB / economia: 359 KiB)  
  - Rebuy (378.6 KiB / economia: 263 KiB)  
  - TikTok (199.4 KiB / economia: 126 KiB)  
  - Loox (147.5 KiB / economia: 111.6 KiB)  

- **CSS não utilizado**  
  - Rebuy (19.5 KiB / economia: 17.1 KiB)  
  - app.css (13.2 KiB / economia: 10.8 KiB)  

- **Fontes externas carregadas por apps**  
  - Rebuy (`fa-light-300.woff2`) sem `font-display: swap`.  

<br>

---

<br><br>

# 📌 Conclusão
Grande parte dos pontos levantados está relacionada a **apps externos**, sem possibilidade de ajuste no Shopify.  
Os principais ganhos de performance virão das **otimizações de imagens e ajustes em scripts/carregamento inicial**, que podem ser feitos pelo time de desenvolvimento.  

