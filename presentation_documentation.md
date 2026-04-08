# Presentation System Documentation

## Overview

Sistema de apresentacao interativa no browser que simula o Google Slides, usado para analise de concorrentes da GUDAY. O formato segue o modelo da WeConvert (MYND - Competitor analysis.pdf) adaptado ao branding da GUDAY.

**Arquivo principal:** `competitor-analysis.html`
**Tipo:** Single-file HTML (CSS + JS inline, zero build step)
**Abre direto no browser** — basta dar duplo clique no arquivo.

---

## Estrutura de Arquivos

```
PRESENTATION/
├── competitor-analysis.html          # Apresentacao principal
├── presentation_documentation.md     # Este arquivo
├── Competitors - Sheet1.csv          # Dados fonte (insights + imagens)
├── GUMMY/                            # Screenshots do concorrente
│   ├── 3.png ... 20.png              # Imagens mobile (18 arquivos)
├── MYND - Competitor analysis.pdf    # Referencia de formato (WeConvert)
├── GUDAY_Reposicionamento_05_03_2026.pdf        # Brand guidelines
└── GUDAY — BASE DE CONHECIMENTO DO ASSISTENTE CRIATIVO (2).pdf
```

---

## Arquitetura do HTML

### Dependencias externas (CDN)

| Lib | Versao | Uso |
|-----|--------|-----|
| Adobe Typekit (`lcw2zgr`) | - | Fonte Degular (branding GUDAY) |
| Google Fonts Inter | 400-800 | Fallback font |
| html2canvas | 1.4.1 | Captura de slides para PDF |
| jsPDF | 2.5.1 | Geracao do PDF |

### Layout principal (3 camadas)

```
┌─────────────────────────────────────────────┐
│  TOOLBAR (52px)                             │
│  [icon + titulo]              [PDF] [Play]  │
├────────┬────────────────────────────────────┤
│THUMBS  │  SLIDE AREA                        │
│(224px) │                                    │
│        │  ┌────────────────────────┐        │
│ [1] ■  │  │     SLIDE ATIVO       │        │
│ [2] □  │  │     (960x540)         │        │
│ [3] □  │  │                       │        │
│ ...    │  └────────────────────────┘        │
│        │  ◄              ►    [2/20]        │
└────────┴────────────────────────────────────┘
```

- **Toolbar:** titulo do documento, botao PDF, botao Apresentar
- **Thumbnail Panel:** miniaturas dos slides (CSS `transform: scale(0.18)` do slide real), separadores por categoria, scroll vertical
- **Slide Area:** slide ativo com escalonamento responsivo, setas de navegacao, contador

### Layout interno de cada slide (formato MYND)

```
┌──────────────┬──────────────────────────────┐
│  COMPETITOR  │  Categoria Title    [PAGE]   │
│  INSIGHTS    │                              │
│──────────────│  ┌──────────┐                │
│ 1. insight   │  │          │                │
│ 2. insight   │  │  IMG     │                │
│ 3. *ATIVO*   │  │  (left)  │                │
│ 4. insight   │  │          │                │
│              │  └──────────┘                │
│              │  url reference               │
│              │                              │
│              │  Source          [GUDAY logo] │
└──────────────┴──────────────────────────────┘
```

- **Sidebar esquerdo (272px):** banner roxo "COMPETITOR INSIGHTS" + lista numerada de insights da categoria (ativo em bold)
- **Area principal:** titulo da categoria + badge da pagina (canto superior direito) + imagem alinhada a esquerda + URL de referencia + footer com source e logo GUDAY

---

## Design System / Branding

### Paleta de cores (CSS variables)

```css
--purple-primary: #b282eb;    /* Roxo claro principal GUDAY */
--purple-deep:    #7f56d9;    /* Roxo escuro (banner, botoes, badges) */
--purple-dark:    #1E192A;    /* Texto escuro / background cover */
--purple-muted:   #6d6daa;    /* Texto secundario */
--bg-cream:       #FAFAF2;    /* Background padrao GUDAY */
--bg-beige:       #f4efe5;    /* Background alternativo */
--bg-sidebar:     #f9f8f6;    /* Sidebar dos slides (cinza bem claro) */
--white:          #ffffff;
--text-dark:      #1E192A;
```

### Tipografia

- **Primaria:** `degular` (Adobe Typekit kit `lcw2zgr`)
- **Fallback:** `Inter` (Google Fonts) > `system-ui` > `sans-serif`
- **Titulo categoria:** 26px, weight 800
- **Sidebar items:** 12.5px
- **Page badge:** 11px, uppercase, letter-spacing 1px

### Decisoes de design (iteracoes feitas)

| Decisao | Motivo |
|---------|--------|
| Imagem sem sombra | Evitar estetica "IA-generated" |
| Imagem sem border-radius | Deixar retangular com borda preta 1px para clareza |
| Imagem alinhada a esquerda | Seguir formato MYND fielmente |
| Sem page labels no sidebar | Poluia visualmente — info da pagina vai no badge superior direito |
| Sem linhas divisorias no sidebar | Visual mais limpo |
| Sem logo GUDAY no sidebar footer | Redundante — logo ja aparece no footer do slide |
| URL de referencia em preto, 9px | Discreto mas visivel, como no MYND |
| Logo GUDAY como SVG preto (16px) | No footer inferior direito de cada slide |
| Sidebar bg `#f9f8f6` | Cinza bem claro, quase branco |

---

## Dados e Categorias

### Fonte de dados

Arquivo `Competitors - Sheet1.csv` com colunas:
- `#` — numero do slide
- `Print` — caminho da imagem (ex: `/gummy/3.png`)
- `Categoria` — categoria do insight
- `Página` — pagina do site (Home, PDP, Menu, Cart, CP)
- `Insight` — texto do insight

### Agrupamento por categoria (ordem na apresentacao)

| # | Categoria | Qtd slides | Paginas |
|---|-----------|-----------|---------|
| 1 | Prova Social | 8 | Home, PDP |
| 2 | Encontrabilidade | 5 | Home, Menu, CP |
| 3 | Clareza | 3 | PDP, CP |
| 4 | Ofertas | 1 | Home |
| 5 | Acessibilidade | 1 | Cart |
| 6 | AOV Booster | 1 | Cart |
| **Total** | | **19 + 1 capa = 20** | |

**Os slides sao agrupados por categoria, independente da pagina de origem.** Dentro de cada categoria, a ordem segue a sequencia original do CSV.

### Mapeamento completo slides → imagens

| Slide | Categoria | Insight | Imagem | Pagina |
|-------|-----------|---------|--------|--------|
| 1 | Capa | - | - | - |
| 2 | Prova Social | UGC depoimentos no topo | GUMMY/3.png | Home |
| 3 | Prova Social | Farmacias parceiras | GUMMY/7.png | Home |
| 4 | Prova Social | UGC videos no topo | GUMMY/10.png | PDP |
| 5 | Prova Social | Descricao dos videos (reviews) | GUMMY/11.png | PDP |
| 6 | Prova Social | Press logos no topo | GUMMY/12.png | PDP |
| 7 | Prova Social | Antes e depois | GUMMY/13.png | PDP |
| 8 | Prova Social | Dados estatisticos | GUMMY/15.png | PDP |
| 9 | Prova Social | Reviews com videos | GUMMY/16.png | PDP |
| 10 | Encontrabilidade | Banners de categorias | GUMMY/4.png | Home |
| 11 | Encontrabilidade | Mais vendidos com sub-filters | GUMMY/5.png | Home |
| 12 | Encontrabilidade | Menu com cards visuais | GUMMY/8.png | Menu |
| 13 | Encontrabilidade | Menu com cards visuais | GUMMY/9.png | Menu |
| 14 | Encontrabilidade | Botoes com sub-filters | GUMMY/20.png | CP |
| 15 | Clareza | Mini FAQ no topo | GUMMY/12.png | PDP |
| 16 | Clareza | Beneficios claros em 1 secao | GUMMY/14.png | PDP |
| 17 | Clareza | Header nas collections | GUMMY/19.png | CP |
| 18 | Ofertas | Banner de grupo de WhatsApp VIP | GUMMY/6.png | Home |
| 19 | Acessibilidade | Modal para selecao de opcoes | GUMMY/17.png | Cart |
| 20 | AOV Booster | Recomendacao de up-sell no carrinho | GUMMY/18.png | Cart |

**Nota:** As imagens 12.png e 15.png/16.png aparecem em mais de um slide (insights diferentes).

---

## Navegacao

### Metodos de navegacao

| Metodo | Acao |
|--------|------|
| Click no thumbnail | Vai para o slide clicado |
| Seta direita / baixo / espaco | Proximo slide |
| Seta esquerda / cima | Slide anterior |
| Home | Primeiro slide |
| End | Ultimo slide |
| Escape | Sair do fullscreen |
| Botoes ◄ ► na tela | Proximo/anterior |

### Transicoes

- **Tipo:** fade + translateX (40px)
- **Duracao:** 0.45s
- **Easing:** `cubic-bezier(.4, 0, .2, 1)`
- **Classes CSS:** `.entering-right`, `.entering-left`, `.exiting-right`, `.exiting-left`, `.active`

---

## Funcionalidades

### Modo Apresentacao (Fullscreen)

- Botao "Apresentar" no toolbar
- Esconde toolbar e painel de thumbnails
- Background preto
- Slide escalonado para preencher a tela
- Setas de navegacao aparecem no hover
- ESC para sair

### Export PDF

- Botao "PDF" no toolbar
- Usa `html2canvas` para capturar cada slide como imagem (scale 2x para qualidade)
- Usa `jsPDF` para montar o PDF landscape (960x540px por pagina)
- Mostra progresso no botao durante a geracao (1/20, 2/20...)
- Download automatico como `Competitor-Analysis-Gummy.pdf`

### Escalonamento responsivo

- Funcao `resizeSlide()` calcula o scale ideal baseado no tamanho da janela
- Slide base: 960x540px (16:9)
- Scale maximo: 2x
- Recalcula no resize da janela e ao entrar/sair do fullscreen

---

## Como adicionar novos concorrentes

### 1. Preparar imagens

Criar pasta dentro de `PRESENTATION/` com o nome do concorrente (ex: `COMPETITOR_NAME/`) e colocar os screenshots numerados.

### 2. Atualizar o CSV

Adicionar linhas em `Competitors - Sheet1.csv` com as colunas:
```
#,Print,Categoria,Pagina,Insight
1,/competitor_name/1.png,Prova Social,Home,Descricao do insight
```

### 3. Atualizar o JavaScript

No array `categories` dentro do `<script>`, adicionar/modificar as entradas:

```javascript
const categories = [
    {
        name: "Nome da Categoria",
        slides: [
            { insight: "Texto do insight", img: "PASTA/arquivo.png", page: "Home" },
            // ... mais slides
        ]
    },
    // ... mais categorias
];
```

### 4. Atualizar metadados

- Titulo no toolbar: `.toolbar-title`
- Nome do concorrente na capa: `cover-competitor-name`
- Data: `cover-date`
- URL de referencia: `img-source-url` no `contentHTML()`
- Nome do arquivo PDF: `pdf.save("...")` na funcao `downloadPDF()`

---

## Estrutura CSS (secoes)

| Secao | Descricao |
|-------|-----------|
| `:root` | CSS variables (cores, dimensoes) |
| TOOLBAR | Barra superior com titulo e botoes |
| MAIN LAYOUT | Container flex (thumbnails + slide area) |
| THUMBNAIL PANEL | Painel esquerdo com miniaturas |
| SLIDE AREA | Area principal do slide |
| SLIDES | Posicionamento e transicoes |
| COVER SLIDE | Slide de capa (gradiente roxo) |
| CONTENT SLIDE | Slides de conteudo (sidebar + main) |
| NAV ARROWS | Botoes de navegacao circular |
| SLIDE COUNTER | Contador de slides no rodape |
| FULLSCREEN MODE | Overrides para modo apresentacao |
| CATEGORY SEPARATOR | Labels de categoria no painel de thumbnails |
| ANIMATIONS | `spin` (loading PDF), `fadeInUp` (cover) |

---

## Estrutura JavaScript (secoes)

| Secao | Descricao |
|-------|-----------|
| DATA | Array `categories` com todos os dados dos slides |
| Flatten | Gera `allSlides` (capa + slides agrupados) |
| RENDER HELPERS | `coverHTML()`, `contentHTML()`, `slideHTML()` |
| BUILD DOM | Cria slides e thumbnails no DOM |
| NAVIGATION | `goTo()`, `next()`, `prev()`, `updateUI()` |
| KEYBOARD | Event listener para teclas de navegacao |
| FULLSCREEN | `toggleFullscreen()`, `enterFS()`, `exitFS()` |
| PDF DOWNLOAD | `downloadPDF()` com html2canvas + jsPDF |
| RESPONSIVE SCALING | `resizeSlide()` para adaptar ao viewport |

---

## Logo GUDAY

SVG inline preto no footer de cada slide. Fonte original (branco):
```
https://guday.com.br/cdn/shop/files/logo-white_0e9f1935-0dc9-4be7-93db-04c6a914f011.svg
```
Convertido para `fill="#000"` no codigo.

---

## Referencia de formato (MYND / WeConvert)

O layout dos slides segue o padrao da apresentacao `MYND - Competitor analysis.pdf` (27 paginas) criada pela WeConvert:

- **Sidebar esquerdo** com banner escuro "COMPETITOR INSIGHTS" + lista numerada progressiva (item ativo em bold)
- **Area principal** com titulo da categoria + screenshots mobile + URL de referencia
- **Footer** com logo da empresa + "Source: Expert heuristic analysis"
- Slides agrupados por categoria (5 categorias no MYND, 6 na nossa)
- Cada sub-ponto da categoria tem seu proprio slide dedicado

A diferenca principal e o branding: MYND usa navy/teal, nos usamos roxo GUDAY (`#7f56d9`).
