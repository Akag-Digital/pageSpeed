# 📊 Plano de Otimização de Performance
**Data**: 30 de setembro de 2025  
**Análise**: Google PageSpeed Insights  
**URL**: https://rovelab.hk
**URL Repositorio**: https://github.com/RoveLab/ecommerce-shopify-rove-hk 
**PageSpeed Insights**: https://pagespeed.web.dev/analysis/https-rovelab-hk/5abt6qfmsl?form_factor=mobile

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
- `announcement-bar.css` – pode ser otimizado para carregar de forma não-bloqueante.  


**Não pode ser feito:** Arquivos da Shopify (`accelerated-checkout-backwards-compat.css` e `advanced-switcher.css`).  

**Impacto esperado:** Menor tempo de carregamento percebido pelo usuário.  

---

### 2. Otimização de imagens
**O problema:** Algumas imagens estão em dimensões ou formatos maiores do que o necessário (**~300 KiB de economia**).  

**Arquivos identificados:**  
1. [`luxury-living-room-mobile_1180x1300_crop_center.jpg`](https://rovelab.hk/cdn/shop/files/luxury-living-room-mobile_1180x1300_crop_center.jpg?v=1730148988) – 151,1 KiB 
2. [`M_BANNER_DEFAULT_2_1_1080x1740_crop_center.jpg`](https://rovelab.hk/cdn/shop/files/M_BANNER_DEFAULT_2_1_1080x1740_crop_center.jpg?v=1738154508) – 104,6 KiB
3. [`c760184679e94704b04db72e70988ccd.thumbnail.0000000000_690x.jpg`](https://rovelab.hk/cdn/shop/files/preview_images/c760184679e94704b04db72e70988ccd.thumbnail.0000000000_690x.jpg?v=1730149102) – 7,5 KiB 

**Aquivos identificados no aplicativo Cevoid Gallery (verificar se é possível alterar as imagens):**
1. [`39GIXyFihp2i5CUzhaUsZ/thumbnail.png`](https://cevoidcontent.com/images/39GIXyFihp2i5CUzhaUsZ/thumbnail.png?class=400) – 37,7 KiB
2. [`c80YZm3P5xTIb3CUwJufB/thumbnail.png`](https://cevoidcontent.com/images/c80YZm3P5xTIb3CUwJufB/thumbnail.png?class=400) – 31,1 KiB
3. [`oAngAg3pTJjVcUhIqbpSN/original.jpeg`](https://cevoidcontent.com/images/oAngAg3pTJjVcUhIqbpSN/original.jpeg?class=400) – 21,4 KiB
4. [`RyX9FEfUIgaC24GL4-wbK/original.jpeg`](https://cevoidcontent.com/images/RyX9FEfUIgaC24GL4-wbK/original.jpeg?class=400) – 21,0 KiB
5. [`sPByAmb782-6q24cpyXRE/original.jpeg`](https://cevoidcontent.com/images/sPByAmb782-6q24cpyXRE/original.jpeg?class=400) – 20,9 KiB


**O que pode ser feito:**  
- Criar versões menores e responsivas.  
- Converter para WebP ou AVIF.  
- Aplicar compressão otimizada em JPG.  

**Impacto esperado:** Redução significativa no peso total e melhora perceptível no carregamento.  


---

### 3. Imagens fora da tela (lazy loading)
**O problema:** Algumas imagens pequenas de pré-visualização carregam mesmo estando fora da área inicial de exibição.  

**Arquivos identificados:**  
1. [preview_images/c760184679e94704b04db72e70988ccd.thumbnail.0000000000_690x.jpg](https://rovelab.hk/cdn/shop/files/preview_images/c760184679e94704b04db72e70988ccd.thumbnail.0000000000_690x.jpg?v=1730149102) – 7,5 KiB  
2. [M1_2-Seat-Sofa-Loveseat_PW_Krypton_Green_2_375x_crop_center.jpg](https://rovelab.hk/cdn/shop/files/M1_2-Seat-Sofa-Loveseat_PW_Krypton_Green_2_375x_crop_center.jpg?v=1729896261) – 7,0 KiB  
3. [M1_3-Seat-Sofa_PW_Platinum_Grey_1_64508b7d-4b51-4c00-a40a-2b9b64fb52ad_375x_crop_center.jpg](https://rovelab.hk/cdn/shop/files/M1_3-Seat-Sofa_PW_Platinum_Grey_1_64508b7d-4b51-4c00-a40a-2b9b64fb52ad_375x_crop_center.jpg?v=1732049989) – 5,2 KiB  
4. [M1_Sectional_PW_Carbon_Black_3_375x_crop_center.jpg](https://rovelab.hk/cdn/shop/files/M1_Sectional_PW_Carbon_Black_3_375x_crop_center.jpg?v=1729896544) – 4,0 KiB  
5. [bazaar_1_20x4_crop_center.svg](https://rovelab.hk/cdn/shop/files/bazaar_1_20x4_crop_center.svg?v=1730149389) – 4,3 KiB (3,8 KiB em outra variação)  
6. [preview_images/cd3fef1140ee48f89fb6d1c2dfd0377d.thumbnail.0000000000_690x.jpg](https://rovelab.hk/cdn/shop/files/preview_images/cd3fef1140ee48f89fb6d1c2dfd0377d.thumbnail.0000000000_690x.jpg?v=1730149170) – 2,3 KiB  


**O que pode ser feito:**  
- Ativar `loading="lazy"`.  

**Impacto esperado:** Possível economia de 30 KiB.  

---

### 4. Tamanho excessivo de payload (15,7 MB)
**O problema:** O site carrega **15,7 MB de recursos**, grande parte em imagens e vídeos pesados.  

**Arquivos identificados:**  
1. [c760184679e94704b04db72e7098...3904.mp4](https://rovelab.hk/cdn/shop/videos/c/vp/c760184679e94704b04db72e70988ccd/c760184679e94704b04db72e70988ccd.HD-1080p-7.2Mbps-37243904.mp4?v=0) – 9.047,8 KiB  
2. [cd3fef1140ee48f89fb6d1c2dfd03...944.mp4](https://rovelab.hk/cdn/shop/videos/c/vp/cd3fef1140ee48f89fb6d1c2dfd0377d/cd3fef1140ee48f89fb6d1c2dfd0377d.HD-1080p-7.2Mbps-37243944.mp4?v=0) – 6.395,3 KiB  
3. [luxury-living-room-mobile...center.jpg](https://rovelab.hk/cdn/shop/files/luxury-living-room-mobile_1180x1300_crop_center.jpg?v=1730148988) – 152,5 KiB  
4. [M_BANNER_DEFAULT_2_1...center.jpg](https://rovelab.hk/cdn/shop/files/M_BANNER_DEFAULT_2_1_1080x1740_crop_center.jpg?v=1738154508) – 106,0 KiB  

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
  class="single-image--mobile lazyautosizes ls-is-cached lazyloaded" 
  width="1080" 
  height="1740" 
  data-sizes="auto" 
  src="https://rovelab.hk/cdn/shop/files/M_BANNER_DEFAULT_2_1_1080x1740_crop_cent…" 
  data-srcset="//rovelab.hk/cdn/shop/files/M_BANNER_DEFAULT_2_1_1080x1740_crop_center.jpg…" 
  fetchpriority="high" 
  alt="" 
  style="object-position: 50.0% 50.0%;" 
  sizes="412px" 
  srcset="//rovelab.hk/cdn/shop/files/M_BANNER_DEFAULT_2_1_1080x1740_crop_center.jpg…"
>

```
---

<br>

## 📊 Tabela de Prioridade (Ações possíveis)

| Prioridade | Tarefa                                | Impacto esperado | Estimativa de tempo |
|------------|----------------------------------------|------------------|----------------------|
| 🔴 Alta    | Otimização de imagens (Itens 2 e 4)    | Muito alto       | 4-5 horas            |
| 🔴 Alta    | Descoberta de solicitações de LCP (Item 5) | Alto       | 30 min – 1 hora      |
| 🟠 Média   | Remoção/adiamento de scripts (Item 1) | Médio            | 30 min – 1 hora      |
| 🟡 Baixa   | Lazy loading em imagens (Item 3)      | Baixo            | 1-2 horas            |

---

<br>

## ⚠️ Itens relacionados a apps externos (não ajustáveis)

Esses itens foram reportados pelo PageSpeed, mas **não podem ser otimizados diretamente**, pois os arquivos vêm de apps de terceiros. A única alternativa seria **remover ou substituir os apps**, caso desejado.  

### Cache curto de recursos externos  
- Facebook Pixel – 117 KiB (cache 0–20 min)  
- Loox (reviews) – 41 KiB (cache 1 dia e 12 h)  
- Clarity – 32 KiB (cache 1 dia)  
- Klaviyo – 3 KiB (cache 1 s)  

---

### JavaScript legado (77 KiB)  
- Klaviyo – 17,8 KiB  
- Rebuy – 27,3 KiB
- Facebook Pixel – 12,5 KiB

---

### JavaScript não utilizado (1,43 MB)  
- Google Tag Manager – 275,2 KiB
- Rebuy – 213,8 KiB
- Converge - 189,8 KiB
- Klaviyo - 49,8 KiB
- Facebook - 82,7 KiB	
- Loox – 40,5 KiB

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

