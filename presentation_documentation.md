# Presentation System Documentation

## Overview

Sistema de apresentacao interativa no browser que simula o Google Slides, usado para analise de concorrentes da GUDAY. O formato segue o modelo da WeConvert (MYND - Competitor analysis.pdf) adaptado ao branding da GUDAY.

**Arquivo principal:** `COMPETITORS/competitor-analysis.html`
**Banco de dados:** `COMPETITORS/DB - Competitors Research - Sheet1.csv`
**Tipo:** Single-file HTML (CSS + JS inline, zero build step)
**Abre direto no browser** — basta dar duplo clique no arquivo ou usar Live Server.

---

## Estrutura de Arquivos

```
PRESENTATION/
├── COMPETITORS/
│   ├── competitor-analysis.html          # Apresentacao principal
│   ├── DB - Competitors Research - Sheet1.csv  # Banco de dados (fonte)
│   ├── _gummy/                           # Screenshots Gummy (1-20.png)
│   ├── _essential/                       # Screenshots Essential (1-12.png)
│   ├── _desincha/                        # Screenshots Desincha (1-12.png)
│   ├── _drgood/                          # Screenshots Dr. Good (1-2.png)
│   ├── _bold/                            # Screenshots Bold (1-11.png)
│   └── _maismu/                          # Screenshots Mais Mu (1-11.png)
├── presentation_documentation.md         # Este arquivo
├── MYND - Competitor analysis.pdf        # Referencia de formato (WeConvert)
├── GUDAY_Reposicionamento_05_03_2026.pdf # Brand guidelines
└── GUDAY — BASE DE CONHECIMENTO DO ASSISTENTE CRIATIVO (2).pdf
```

**Nota:** As imagens no HTML usam caminhos relativos a partir de `COMPETITORS/` (ex: `_gummy/1.png`). Pastas de concorrentes sempre em minusculo com prefixo `_`.

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
│        │  ◄              ►    [2/51]        │
└────────┴────────────────────────────────────┘
```

- **Toolbar:** titulo do documento, botao PDF, botao Apresentar
- **Thumbnail Panel:** miniaturas dos slides (CSS `transform: scale(0.18)` do slide real), separadores por categoria, scroll vertical
- **Slide Area:** slide ativo com escalonamento responsivo, setas de navegacao, contador

### Layout interno de cada slide (formato MYND)

```
┌──────────────┬──────────────────────────────────────────┐
│  COMPETITOR  │  Categoria Title                 [PAGE]  │
│  INSIGHTS    │                                          │
│──────────────│  ┌─────────┐ ┌─────────┐ ┌─────────┐   │
│ 1. titulo    │  │  IMG 1  │ │  IMG 2  │ │  IMG 3  │   │
│ 2. titulo    │  │         │ │         │ │         │   │
│ 3. *ATIVO*   │  │         │ │         │ │         │   │
│   (descricao)│  └─────────┘ └─────────┘ └─────────┘   │
│ 4. titulo    │  url1        url2        url3           │
│              │                                          │
│              │  Source                    [GUDAY logo]   │
└──────────────┴──────────────────────────────────────────┘
```

- **Sidebar esquerdo (272px):** banner roxo "COMPETITOR INSIGHTS" + lista numerada de insights da categoria
  - Item ativo: **titulo em bold + (descricao) em bold** — mesma cor e formato
  - Itens inativos: apenas titulo em cinza claro
  - Se nao tem titulo, usa a descricao como titulo
- **Area principal:** titulo da categoria + badge da pagina (canto superior direito) + ate 3 imagens lado a lado + URL de cada concorrente embaixo do respectivo screenshot + footer com source e logo GUDAY

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
| Imagens alinhadas a esquerda, lado a lado | Seguir formato MYND fielmente (ate 3 por slide) |
| URL embaixo de cada screenshot | Cada imagem tem sua URL do concorrente correspondente |
| Sem page labels no sidebar | Poluia visualmente — info da pagina vai no badge superior direito |
| Sem linhas divisorias no sidebar | Visual mais limpo |
| Sem logo GUDAY no sidebar footer | Redundante — logo ja aparece no footer do slide |
| URL de referencia em `#333`, 10px | Discreto mas visivel, como no MYND |
| Logo GUDAY como SVG preto (16px) | No footer inferior direito de cada slide |
| Sidebar bg `#f9f8f6` | Cinza bem claro, quase branco |
| Descricao em bold no item ativo | Mesma cor e peso do titulo — sem diferenciar visualmente |

---

## Dados e Categorias

### Fonte de dados

Arquivo `COMPETITORS/DB - Competitors Research - Sheet1.csv` com colunas:
- `#` — numero sequencial
- `Categoria` — categoria do insight (Clareza, Confianca, UX, Beneficios, AOV Booster, Ofertas)
- `Bench` — URLs dos concorrentes separadas por ` / ` (1a URL = Print 1, 2a = Print 2, etc.)
- `Página` — pagina do site (Home, HP, PDP, PP, Menu, Cart, CP, Checkout)
- `Insight` — titulo do insight (pode ser vazio)
- `Descrição` — descricao detalhada (pode ser vazio)
- `Print 1` — caminho da 1a imagem (ex: `/_gummy/1.png`)
- `Print 2` — caminho da 2a imagem (opcional)
- `Print 3` — caminho da 3a imagem (opcional)

### Logica de titulo/descricao

- Se tem **Insight** e **Descricao**: titulo = Insight, descricao entre parenteses no slide ativo
- Se tem **Insight** mas nao **Descricao**: titulo = Insight, sem parenteses
- Se tem **Descricao** mas nao **Insight**: titulo = Descricao
- A descricao so aparece no slide ativo, nao nos outros itens do sidebar

### Concorrentes analisados

| Concorrente | Pasta | URL | Qtd imagens |
|-------------|-------|-----|-------------|
| Gummy | `_gummy/` | gummy.com.br | 19 |
| Essential Nutrition | `_essential/` | essentialnutrition.com.br | 12 |
| Desincha | `_desincha/` | desincha.com.br | 12 |
| Dr. Good | `_drgood/` | finistore.com.br | 2 |
| Bold Snacks | `_bold/` | boldsnacks.com.br | 11 |
| Mais Mu | `_maismu/` | lojamaismu.com.br | 11 |

### Agrupamento por categoria (ordem na apresentacao)

| # | Categoria | Qtd slides |
|---|-----------|-----------|
| 1 | Clareza | 14 |
| 2 | Confianca | 7 |
| 3 | UX | 12 |
| 4 | Beneficios | 11 |
| 5 | AOV Booster | 5 |
| 6 | Ofertas | 1 |
| **Total** | | **50 + 1 capa = 51** |

**Os slides sao agrupados por categoria, independente da pagina de origem.**

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
- Mostra progresso no botao durante a geracao (1/51, 2/51...)
- Download automatico como `Competitor-Analysis-GUDAY.pdf`

### Escalonamento responsivo

- Funcao `resizeSlide()` calcula o scale ideal baseado no tamanho da janela
- Slide base: 960x540px (16:9)
- Scale maximo: 2x
- Recalcula no resize da janela e ao entrar/sair do fullscreen

---

## Como adicionar novos concorrentes

### 1. Preparar imagens

Criar pasta dentro de `COMPETITORS/` com prefixo `_` e nome minusculo (ex: `_novocompetitor/`) e colocar os screenshots numerados (1.png, 2.png, ...).

### 2. Atualizar o CSV

Adicionar linhas em `DB - Competitors Research - Sheet1.csv` com as colunas:
```
#,Categoria,Bench,Pagina,Insight,Descricao,Print 1,Print 2,Print 3
51,Clareza,novosite.com.br,Home,Titulo do insight,Descricao detalhada,/_novocompetitor/1.png,,
```

### 3. Atualizar o JavaScript

No array `categories` dentro do `<script>`, adicionar as entradas na categoria correspondente:

```javascript
{
    title: "Titulo do insight",
    desc: "Descricao detalhada",
    imgs: ["_novocompetitor/1.png"],
    urls: ["novosite.com.br"],
    page: "Home"
}
```

### 4. Atualizar capa (se novo concorrente)

- Adicionar nome na capa: `cover-competitor-name`
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
| DATA | Array `categories` com todos os dados dos slides (titulo, desc, imgs[], urls[], page) |
| Flatten | Gera `allSlides` (capa + slides agrupados por categoria) |
| RENDER HELPERS | `getTitle()`, `coverHTML()`, `contentHTML()`, `slideHTML()` |
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
- **Area principal** com titulo da categoria + ate 3 screenshots mobile lado a lado + URL de referencia embaixo de cada screenshot
- **Footer** com logo da empresa + "Source: Expert heuristic analysis"
- Slides agrupados por categoria (5 categorias no MYND, 6 na nossa)
- Cada sub-ponto da categoria tem seu proprio slide dedicado

A diferenca principal e o branding: MYND usa navy/teal, nos usamos roxo GUDAY (`#7f56d9`).

---

## Notas tecnicas sobre imagens

- As imagens dos screenshots mobile sao renderizadas com `object-fit: contain` e `object-position: top left` para manter proporcao e alinhar a esquerda
- A imagem usa `flex: 1` + `min-height: 0` dentro do `.phone-mockup` para encolher e sempre deixar espaco para a URL de referencia embaixo
- Borda preta de 1px (`border: 1px solid #000`) para delimitar claramente o screenshot
- Sem border-radius, sem sombra — visual limpo e profissional
- Ate 3 imagens lado a lado usando flex row com `gap: 12px` e `align-items: stretch`
- Resolucao base do slide e 960x540 (16:9). Tentativas de aumentar para 1440x810, 1920x1080, 1920x820 e 1600x600 foram revertidas pois degradavam a qualidade visual das imagens (scaling up do browser causa blur). A resolucao 960x540 com scale down e a que produz melhor resultado
