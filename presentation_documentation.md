# Presentation System Documentation

## Overview

Sistema de apresentacao interativa no browser que simula o Google Slides, usado para analise de concorrentes da GUDAY. O formato segue o modelo da WeConvert (MYND - Competitor analysis.pdf) adaptado ao branding da GUDAY.

**Arquivo principal:** `COMPETITORS/competitor-analysis.html`
**Arquivo legado:** `COMPETITORS/competitor-analysis-legado.html` (backup pre-anotacoes)
**Banco de dados:** `COMPETITORS/DB - Competitors Research - Sheet1.csv`
**Anotacoes:** `COMPETITORS/annotations.json` (exportavel, opcional)
**Tipo:** Single-file HTML (CSS + JS inline, zero build step)
**Requer Live Server** — os dados sao carregados via `fetch()` do CSV, nao funciona com `file://`.

### Fluxo de dados (dinamico)

1. Ao carregar a pagina, o JS faz `fetch("DB - Competitors Research - Sheet1.csv")`
2. Parseia o CSV automaticamente (suporta campos com aspas/virgulas)
3. Agrupa por categoria com **ordem fixa**: Clareza > Confianca > Beneficios > AOV Booster > UX
4. Pagina o sidebar (max 8 itens por pagina)
5. Renderiza slides, thumbnails e navegacao
6. Carrega anotacoes (annotations.json > localStorage)
7. Navega para o slide indicado no parametro `?slide=` da URL (se houver)
8. **Para atualizar a apresentacao: edite o CSV e recarregue a pagina**

---

## Estrutura de Arquivos

```
PRESENTATION/
├── COMPETITORS/
│   ├── competitor-analysis.html              # Apresentacao principal
│   ├── competitor-analysis-legado.html       # Backup pre-anotacoes
│   ├── DB - Competitors Research - Sheet1.csv  # Banco de dados (fonte)
│   ├── annotations.json                      # Anotacoes exportadas (opcional)
│   ├── _gummy/                               # Screenshots Gummy
│   ├── _essential/                           # Screenshots Essential
│   ├── _desincha/                            # Screenshots Desincha
│   ├── _drgood/                              # Screenshots Dr. Good
│   ├── _bold/                                # Screenshots Bold
│   └── _maismu/                              # Screenshots Mais Mu
├── presentation_documentation.md             # Este arquivo
├── MYND - Competitor analysis.pdf            # Referencia de formato (WeConvert)
├── GUDAY_Reposicionamento_05_03_2026.pdf     # Brand guidelines
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
┌──────────────────────────────────────────────────────────────────┐
│  TOOLBAR (52px)                                                  │
│  [🔍+][🔍-][✕] [Editar] [↺][🗑] [JSON] [PDF] [Apresentar]     │
├────────┬─────────────────────────────────────────────────────────┤
│THUMBS  │  SLIDE AREA                                             │
│(224px) │                                                         │
│        │  ┌────────────────────────────────┐                     │
│ [1] ■  │  │     SLIDE ATIVO (960x540)     │                     │
│ [2] □  │  │     + annotation rects        │                     │
│ [3] □  │  │                               │                     │
│ ...    │  └────────────────────────────────┘                     │
│        │  ◄                ►         [2/50]                      │
└────────┴─────────────────────────────────────────────────────────┘
```

Toolbar buttons (esquerda para direita):
- **🔍 Ampliar** — ativa modo zoom (mostra +, -, ✕)
- **Editar** — ativa modo anotacao (mostra ↺ desfazer, 🗑 limpar slide)
- **JSON** — exporta anotacoes como arquivo
- **PDF** — exporta apresentacao completa
- **Apresentar** — modo fullscreen

### Layout interno de cada slide (formato MYND)

```
┌──────────────┬──────────────────────────────────────────┐
│  COMPETITOR  │  Categoria Title                 [PAGE]  │
│  INSIGHTS    │                                          │
│──────────────│  ┌─────────┐ ┌─────────┐ ┌─────────┐   │
│ 1. titulo    │  │  IMG 1  │ │  IMG 2  │ │  IMG 3  │   │
│ 2. titulo    │  │  [rect] │ │         │ │  [rect] │   │
│ 3. *ATIVO*   │  │         │ │         │ │         │   │
│   (descricao)│  └─────────┘ └─────────┘ └─────────┘   │
│ 4. titulo    │  url1        url2        url3           │
│              │                                          │
│              │  Source                    [GUDAY logo]   │
└──────────────┴──────────────────────────────────────────┘
```

- **Sidebar esquerdo (272px):** banner roxo "COMPETITOR INSIGHTS" + lista numerada (max 8 por pagina)
  - Item ativo: **titulo em bold + (descricao) em bold** — mesma cor e formato
  - Itens inativos: apenas titulo em cinza claro
  - Se nao tem titulo, usa a descricao como titulo
- **Area principal:** titulo da categoria + badge da pagina (canto superior direito) + ate 3 imagens lado a lado + retangulos de anotacao + URL de cada concorrente embaixo do respectivo screenshot + footer com source e logo GUDAY

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
| Retangulos de anotacao em vermelho | Borda 3px `#e00` com fundo semi-transparente |

---

## Dados e Categorias

### Fonte de dados

Arquivo `COMPETITORS/DB - Competitors Research - Sheet1.csv` com colunas:
- `ID` — identificador unico de 4 caracteres (ex: `a1b2`) — **nunca alterar**
- `#` — numero sequencial (referencia, nao usado pelo codigo)
- `Categoria` — categoria do insight (Clareza, Confianca, Beneficios, AOV Booster, UX)
- `Bench` — URLs dos concorrentes separadas por ` / ` (1a URL = Print 1, 2a = Print 2, etc.)
- `Página` — pagina do site (Home, HP, PDP, PP, Menu, Cart, CP, Checkout)
- `Insight` — titulo do insight (pode ser vazio)
- `Descrição` — descricao detalhada (pode ser vazio)
- `Print 1` — caminho da 1a imagem (ex: `/_gummy/1.png`)
- `Print 2` — caminho da 2a imagem (opcional)
- `Print 3` — caminho da 3a imagem (opcional)

### IDs unicos

- Cada linha do CSV tem um ID fixo de 4 caracteres (ex: `a1b2`, `c3d4`)
- O ID e usado como chave para anotacoes e parametro de URL
- **Nunca reutilizar um ID** — ao adicionar novas linhas, criar IDs novos
- Remover/reordenar linhas nao afeta anotacoes de outros slides

### Logica de titulo/descricao

- Se tem **Insight** e **Descricao**: titulo = Insight, descricao entre parenteses no slide ativo
- Se tem **Insight** mas nao **Descricao**: titulo = Insight, sem parenteses
- Se tem **Descricao** mas nao **Insight**: titulo = Descricao
- A descricao so aparece no slide ativo, nao nos outros itens do sidebar
- A descricao no slide ativo fica em **bold** (mesma cor e peso do titulo)

### Logica de URLs (Bench)

- URLs separadas por ` / ` no campo Bench
- Cada URL corresponde ao Print na mesma posicao (1a URL → Print 1, 2a → Print 2, etc.)
- **Se houver mais prints que URLs, a ultima URL disponivel e repetida** para os prints restantes
- Isso evita imagens sem link quando o mesmo site tem multiplos screenshots

### Ordem das categorias

Ordem fixa definida no JS (array `CAT_ORDER`):
1. Clareza
2. Confianca
3. Beneficios
4. AOV Booster
5. UX

Categorias novas que nao estao na lista aparecem no final automaticamente.

### Paginacao do sidebar

- Maximo de **8 itens por pagina** no sidebar
- Categorias com mais de 8 itens sao divididas automaticamente em multiplos slides
- Numeracao continua entre paginas (ex: pagina 2 comeca em 9, 10, 11...)
- Mesmo titulo de categoria em todas as paginas

### Validacao de imagens

- Imagens com path quebrado mostram borda vermelha tracejada + nome do arquivo
- `console.warn` e logado no DevTools para cada imagem nao encontrada
- Para validar todos os paths do CSV: rodar o script Node.js descrito na secao "Como validar"

### Concorrentes analisados

| Concorrente | Pasta | URL |
|-------------|-------|-----|
| Gummy | `_gummy/` | gummy.com.br |
| Essential Nutrition | `_essential/` | essentialnutrition.com.br |
| Desincha | `_desincha/` | desincha.com.br |
| Dr. Good | `_drgood/` | finistore.com.br |
| Bold Snacks | `_bold/` | boldsnacks.com.br |
| Mais Mu | `_maismu/` | lojamaismu.com.br |

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
| URL `?slide=ID` | Abre direto no slide com aquele ID |

### URL com parametros

- A URL atualiza automaticamente ao mudar de slide: `?slide=a1b2`
- Ao abrir com `?slide=m3n4`, navega direto para o slide com ID `m3n4`
- Capa usa `?slide=cover`
- Util para compartilhar um slide especifico com alguem

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
- Desativa zoom automaticamente ao entrar

### Export PDF

- Botao "PDF" no toolbar
- Usa `html2canvas` para capturar cada slide como imagem (scale 2x para qualidade)
- Usa `jsPDF` para montar o PDF landscape (960x540px por pagina)
- Mostra progresso no botao durante a geracao
- Download automatico como `Competitor-Analysis-GUDAY.pdf`
- Quantidade de slides e dinamica (depende do CSV)
- Inclui anotacoes (retangulos vermelhos) no PDF

### Modo Edicao (Anotacoes)

Permite desenhar retangulos vermelhos sobre os screenshots para destacar areas especificas.

**Como usar:**
1. Clicar em **"Editar"** — botao fica vermelho, aparecem botoes ↺ e 🗑
2. **Desenhar** — clicar e arrastar sobre qualquer screenshot
3. **Deletar** — hover sobre retangulo mostra ✕, clicar para remover
4. **↺ Desfazer** — remove a ultima anotacao criada
5. **🗑 Limpar slide** — remove todas anotacoes do slide atual
6. **"Salvar"** — clicar para sair do modo edicao

**Persistencia:**
- Salva automaticamente no **localStorage** a cada edicao
- Botao **"JSON"** exporta todas anotacoes como `annotations.json`
- No load, tenta carregar `annotations.json` via fetch (prioridade), senao usa localStorage
- Para compartilhar anotacoes: exportar JSON, commitar no git

**Detalhes tecnicos:**
- Coordenadas em **percentual relativo a imagem** — funciona em qualquer zoom/scale
- Chave de cada anotacao: `{ID}-{imgIndex}` (ex: `a1b2-0`)
- IDs fixos no CSV garantem que remover/reordenar linhas nunca quebra anotacoes
- Retangulos minimos de 1.5% ignorados (previne cliques acidentais)

### Modo Zoom

Independente do modo edicao — permite ampliar o slide para melhor visualizacao e edicao.

**Como usar:**
1. Clicar no icone **🔍** (lupa) no toolbar
2. Aparecem botoes **🔍+** (zoom in), **🔍-** (zoom out) e **✕** (fechar)
3. Zoom persiste ao navegar entre slides
4. **✕** fecha o zoom e volta ao scaling normal
5. Entrar em modo apresentacao desativa o zoom automaticamente

**Detalhes tecnicos:**
- Zoom inicial: 1.1x, incrementos de 0.1x (range: 0.5x a 3x)
- `transform-origin: top left` para zoom comece do canto superior esquerdo
- `resizeSlide()` respeita o zoom ativo (nao sobrescreve)

### Escalonamento responsivo

- Funcao `resizeSlide()` calcula o scale ideal baseado no tamanho da janela
- Slide base: 960x540px (16:9)
- Scale maximo: 2x
- Recalcula no resize da janela e ao entrar/sair do fullscreen
- Nao interfere quando zoom esta ativo

---

## Como adicionar novos concorrentes

### 1. Preparar imagens

Criar pasta dentro de `COMPETITORS/` com prefixo `_` e nome minusculo (ex: `_novocompetitor/`) e colocar os screenshots numerados (1.png, 2.png, ...).

### 2. Atualizar o CSV

Adicionar linhas em `DB - Competitors Research - Sheet1.csv`:
```
ID,#,Categoria,Bench,Pagina,Insight,Descricao,Print 1,Print 2,Print 3
x1y2,51,Clareza,novosite.com.br,Home,Titulo do insight,descricao detalhada,/_novocompetitor/1.png,,
```

**Regras:**
- Criar um **ID unico de 4 caracteres** (letras + numeros) — nunca reutilizar
- **Nao precisa editar o JavaScript** — dados sao carregados dinamicamente do CSV
- Categoria deve ser uma das existentes ou uma nova (aparecera no final)

### 3. Atualizar capa (se novo concorrente)

- Adicionar nome na capa: editar `cover-competitor-name` no HTML

### 4. Validar

Recarregar a pagina no Live Server e verificar. Para validacao em batch:
```bash
cd PRESENTATION/COMPETITORS
node -e "
const fs = require('fs');
const text = fs.readFileSync('DB - Competitors Research - Sheet1.csv', 'utf-8');
const lines = text.split('\n').filter(l => l.trim());
for (let i = 1; i < lines.length; i++) {
    const cols = [];
    let cur = '', inQ = false;
    for (const ch of lines[i]) {
        if (ch === '\"') { inQ = !inQ; }
        else if (ch === ',' && !inQ) { cols.push(cur.trim()); cur = ''; }
        else { cur += ch; }
    }
    cols.push(cur.trim());
    for (const ci of [7,8,9]) {
        const p = (cols[ci] || '').replace(/^\//, '');
        if (p && !fs.existsSync(p))
            console.log('MISSING: ' + p + ' (ID: ' + cols[0] + ')');
    }
}
console.log('Done.');
"
```

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
| ANNOTATIONS / EDIT MODE | Retangulos vermelhos, cursor crosshair, botoes de edicao |
| ANIMATIONS | `spin` (loading PDF), `fadeInUp` (cover) |

---

## Estrutura JavaScript (secoes)

| Secao | Descricao |
|-------|-----------|
| CSV PARSER | `parseCSV()` e `csvToCategories()` — carrega e parseia o CSV dinamicamente |
| FETCH & PARSE | `fetch()` do CSV no load, com fallback de erro |
| CAT_ORDER | Ordem fixa das categorias |
| Flatten | Gera `allSlides` (capa + slides agrupados por categoria, paginados a cada 8) |
| RENDER HELPERS | `getTitle()`, `coverHTML()`, `contentHTML()`, `slideHTML()` |
| BUILD DOM | Cria slides e thumbnails no DOM |
| NAVIGATION | `goTo()`, `next()`, `prev()`, `updateUI()` + URL params |
| KEYBOARD | Event listener para teclas de navegacao |
| FULLSCREEN | `toggleFullscreen()`, `enterFS()`, `exitFS()` |
| ZOOM | `toggleZoomMode()`, `zoomIn()`, `zoomOut()`, `exitZoomMode()`, `applyEditZoom()` |
| PDF DOWNLOAD | `downloadPDF()` com html2canvas + jsPDF |
| RESPONSIVE SCALING | `resizeSlide()` para adaptar ao viewport (respeita zoom) |
| URL PARAM | Leitura de `?slide=ID` no load para deep linking |
| ANNOTATIONS | `loadAnnotations()`, `renderAllAnnotations()`, `addRectDOM()`, `deleteAnnotation()` |
| EDIT MODE | `toggleEditMode()`, drawing via mousedown/mousemove/mouseup |
| UNDO | `undoAnnotation()`, `clearSlideAnnotations()` com historico |
| EXPORT | `exportAnnotations()` — download de annotations.json |

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
- Slides agrupados por categoria (5 categorias no MYND, 5 na nossa)
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
- Imagens com path invalido mostram borda vermelha tracejada + alt text com nome do arquivo + console.warn
