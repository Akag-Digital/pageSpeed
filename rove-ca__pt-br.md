# 📊 Plano de Otimização de Performance
**Data**: 29 de setembro de 2025  
**Análise**: Google PageSpeed Insights  
**URL**: https://rovelab.ca  
**URL Repositorio**: https://github.com/RoveLab/ecommerce-shopify-rove-ca  
**PageSpeed Insights**: https://pagespeed.web.dev/analysis/https-rovelab-ca/t1kptem4hg?form_factor=mobile

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
**O problema:** Algumas imagens estão em dimensões ou formatos maiores do que o necessário (**~607 KiB de economia**).  

**Arquivos identificados:**  
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

**O que pode ser feito:**  
- Criar versões menores e responsivas.  
- Converter para WebP ou AVIF.  
- Aplicar compressão otimizada em JPG.  

**Impacto esperado:** Redução significativa no peso total e melhora perceptível no carregamento.  

---

### 3. Estabilidade do layout (CLS)
**O problema:** Ocorrem mudanças de layout durante o carregamento por imagens sem dimensões fixadas e pela fonte Poppins.  

**Arquivos identificados:**  
- `M_Hero_1180x1740_crop_center.jpg` – imagem sem dimensões fixadas.  
- Seção de produtos em destaque (`shopify-section-featured_collection`).  
- Fonte **Poppins (woff2)**.  

**O que pode ser feito:**  
- Definir largura/altura nas imagens.  
- Usar `font-display: swap` para fontes.  
- Reservar espaço em seções de destaque.  

**Impacto esperado:** Experiência mais estável e sem mudanças visuais bruscas.  

---

### 4. Imagens fora da tela (lazy loading)
**O problema:** Algumas imagens pequenas de pré-visualização carregam mesmo estando fora da área inicial de exibição.  

**Arquivos identificados:**  
- [75b9d7d221f04e32a19dc387d6c8c754…_690x.jpg](https://rovelab.ca/cdn/shop/files/preview_images/75b9d7d221f04e32a19dc387d6c8c754.thumbnail.0000000000_690x.jpg?v=1725888914) – 7.5 KiB  
- [0c2f9128f931419292fcbea199437511…_690x.jpg](https://rovelab.ca/cdn/shop/files/preview_images/0c2f9128f931419292fcbea199437511.thumbnail.0000000000_690x.jpg?v=1725889262) – 2.3 KiB  

**O que pode ser feito:**  
- Ativar `loading="lazy"`.  

**Impacto esperado:** Mínimo (~10 KiB), mas evita download desnecessário logo no carregamento inicial.  

---

### 5. Tamanho excessivo de payload (8,4 MB)
**O problema:** O site carrega **8,4 MB de recursos**, grande parte em imagens pesadas.  

**Arquivos identificados:**  
1. [Corduroy_Velvet_Clay_Swatch_1.jpg](https://rovelab.ca/cdn/shop/files/Corduroy_Velvet_Clay_Swatch_1.jpg?v=1753987241) – 304.7 KiB  
2. [Corduroy_Velvet_Silkstone_Swatch_1.jpg](https://rovelab.ca/cdn/shop/files/Corduroy_Velvet_Silkstone_Swatch_1.jpg?v=1753987224) – 285.7 KiB  
3. [D_Hero_3_2800x1200_crop_center.jpg](https://rovelab.ca/cdn/shop/files/D_Hero_3_2800x1200_crop_center.jpg?v=1758915004) – 265.8 KiB  
4. [D_HERO_BANNER_6_1_2800x1200_crop_center.jpg](https://rovelab.ca/cdn/shop/files/D_HERO_BANNER_6_1_83ae70aa-2bee-4e3d-96f7-f2ffad09d375_2800x1200_crop_center.jpg?v=1758585436) – 196.6 KiB  

**O que pode ser feito:**  
- Comprimir imagens.  
- Criar versões responsivas.  
- Converter para WebP/AVIF.  

**Impacto esperado:** Redução significativa do peso total da página.  

---

<br>

## 📊 Tabela de Prioridade (Ações possíveis)

| Prioridade | Tarefa                                | Impacto esperado | Estimativa de tempo |
|------------|----------------------------------------|------------------|----------------------|
| 🔴 Alta    | Otimização de imagens (Itens 2 e 5)    | Muito alto       | 4-5 horas            |
| 🔴 Alta    | Estabilidade do layout (CLS – Item 3) | Alto             | 3-4 horas            |
| 🟠 Média   | Remoção/adiamento de scripts (Item 1) | Médio            | 30 min – 1 hora      |
| 🟡 Baixa   | Lazy loading em imagens (Item 4)      | Baixo            | 1-2 horas            |

---

<br>

## ⚠️ Itens relacionados a apps externos (não ajustáveis)

Esses itens foram reportados pelo PageSpeed, mas **não podem ser otimizados diretamente**, pois os arquivos vêm de apps de terceiros. A única alternativa seria **remover ou substituir os apps**, caso desejado.  

### Cache curto de recursos externos  
- Intelligems – 180 KiB (cache 5 min)  
- Facebook Pixel – 117 KiB (cache 0–20 min)  
- Loox (reviews) – 214 KiB (cache 1 dia e 12 h)  
- 9gtb (script externo) – 58 KiB (cache 1 dia)  
- Snapchat Analytics – 24 KiB (cache 10 min)  

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
- Google Tag Manager – 715.1 KiB  
- Rebuy – 378.6 KiB  
- TikTok – 199.4 KiB  
- Loox – 147.5 KiB  

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

