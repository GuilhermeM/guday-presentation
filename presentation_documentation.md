# Presentation System Documentation

## Overview

Sistema de apresentacoes interativas no browser que simula o Google Slides, usado para analises da GUDAY. O formato segue o branding da GUDAY e o modelo da WeConvert.

Existem **5 apresentacoes** independentes:

### 1. Competitor Analysis (Analise de Concorrentes)

**Arquivo:** `COMPETITORS/competitor-analysis.html`
**Legado:** `COMPETITORS/competitor-analysis-legado.html`
**Banco de dados:** `COMPETITORS/DB - Competitors Research - Sheet1.csv`
**Anotacoes:** `COMPETITORS/annotations.json`
**Fonte de dados:** CSV local (via `fetch`)
**Requer Live Server** — nao funciona com `file://`

### 4. User Testing Analysis (Analise de Usabilidade)

**Arquivo:** `USER-TESTING/user-testing-analysis.html`
**Fonte de dados:** Hardcoded no HTML (extraido de `User Testing - Guday.pdf`)
**Referencia:** `Tabs - User Testing Analysis.pdf`, `Supplend-User testing.pdf`
**Slides:**
1. Capa (6 participantes, 3 mobile, 3 desktop)
2. Cards: 5 segundos + Primeiras impressoes + O que destaca
3. Cards: O que falta + O que impede + Compraria?
4. Cards: Product pages + Carrinho e Checkout (2 colunas)
5. Key Takeaways (9 insights numerados)

**Edicoes:** `USER-TESTING/ut-edits.json` (exportavel, opcional)

**Tipo de slide `cards`:** Grade de 2-3 cards por slide com summary bold no topo, cada card com icone, titulo e lista de bullet points com bold. Suporta `cols: 2` para layout de 2 colunas. Icones: emoji coloridos com fundo circular.

**Features:** PDF export, fullscreen, thumbnails, navegacao por teclado, edicao de texto inline (titulos, cards, bullets, key takeaways) com persistencia localStorage + JSON export

**Slides atuais (5):**
1. Capa (6 participantes, 3 mobile, 3 desktop)
2. Cards 3 colunas: 5 segundos (👁) + Primeiras impressoes (💬) + O que destaca (⭐)
3. Cards 3 colunas: O que falta (❓) + O que impede (🚫) + Compraria? (🛒)
4. Cards 2 colunas: Product pages (📄) + Carrinho/Checkout (💳)
5. Key Takeaways: 8 insights numerados (clareza, sabores, info PDPs, frete, parcelamento, cupom, avaliacoes, cross-sell)

### 3. Stakeholders Analysis (Analise de Stakeholders)

**Arquivo:** `STAKEHOLDERS/stakeholders-analysis.html`
**Fonte de dados:** Hardcoded no HTML (extraido de `Guday - Stakeholders.pdf`)
**Edicoes:** `STAKEHOLDERS/stakeholders-edits.json` (exportavel, opcional)
**Slides:**
1. Capa
2. Resumo da Guday pelos C-Levels (frase, valores, desafios, metas)
3. Leitura da equipe de suporte (reclamacoes, duvidas, insights)
4. Key Takeaways (9 insights numerados no formato "bold: descricao")

**Features:** PDF export, fullscreen, thumbnails, navegacao por teclado, edicao de texto inline com persistencia (localStorage + JSON export)

**Tipos de slide:**
- `cover` — capa com gradiente roxo
- `info` — titulo + card com secoes (label + texto), suporta HTML (ex: `<br>` para quebra de linha)
- `list` — titulo + itens numerados com bold + descricao na mesma linha

### 2. Post-Purchase Analysis (Pesquisa Pos-Compra)

**Arquivo:** `POST-PURCHASE/post-purchase-analysis.html`
**Fonte de dados:** Google Sheets ao vivo (publica)
**Sheet ID:** `1rQ5VjGYa07zwi-utOK4dE2UmhIxVhSi1WqvcVHECgSs`
**Funciona direto no browser** — dados carregados do Google Sheets via URL publica, nao precisa de Live Server

### 5. Analytics Analysis (Analise de Funil)

**Arquivo:** `ANALYTICS/analytics-analysis.html`
**Fonte de dados:** Google Sheets ao vivo (publica)
**Sheet ID:** `16w4WevtU6cRKbImBp6OxhLcg-cIT8w7320dx3YYdH_U`
**Aba:** Funil (gid: `212738571`)
**Edicoes:** `ANALYTICS/analytics-edits.json` (exportavel, opcional)
**Funciona direto no browser** — dados carregados do Google Sheets via URL publica, nao precisa de Live Server
**Referencia estetica:** `ANALYTICS/Analytics Analysis (1).pdf` (slides WeConvert)

**Slides atuais (3):**
1. Capa — "Onde os seus visitantes estao abandonando?" (estilo WeConvert: fundo verde/teal, logo GUDAY)
2. Funil Homepage — barras verticais: Home (86.353) → Product (34.792) → Cart (7.412) → Checkout (4.131) → Purchase (2.122). Taxa de abandono em badges vermelhos, CVR final em badge teal. Conclusoes no painel direito.
3. Funil PDP — barras verticais: Product (656.096) → Cart (28.668) → Checkout (14.697) → Purchase (7.010). Mesmo formato.

**Tipo de slide `funnel`:** Grafico de barras verticais com eixo Y, labels de sessoes, badges de abandonment rate (vermelho) e CVR final (teal). Painel de conclusoes editaveis ao lado direito. Visual inspirado no formato WeConvert (cores navy/teal/coral).

**Dados da planilha (aba Funil):**
- Secao `GA4 - Home - 90D`: funil completo da homepage (5 etapas)
- Secao `GA4 - PDP - 90D`: funil completo das PDPs (4 etapas)
- Colunas: Stage, Total, %, CVR, Bench
- Parser detecta headers `GA4 - Home` e `GA4 - PDP` para separar os dois funis

**Fluxo de dados:**
1. Fetch da aba Funil do Google Sheets via CSV export
2. Parser identifica dois blocos (Home e PDP) pelos headers
3. Calcula abandonment rates automaticamente (1 - next/current)
4. Dados ao vivo substituem fallback hardcoded
5. Se fetch falhar, usa dados hardcoded como fallback

**Features:** PDF export, fullscreen, thumbnails, navegacao por teclado, edicao de texto inline (titulos, conclusoes) com persistencia localStorage + JSON export, anotacoes com retangulos

**Paleta de cores (estilo WeConvert):**
```css
--weconvert-teal: #0ea5a0;    /* Verde teal (capa, badges CVR) */
--weconvert-dark: #0c2340;    /* Navy escuro (titulos, barras) */
--weconvert-coral: #e8524f;   /* Coral (badges abandonment) */
```

**Cores das barras (degradee navy):**
- Homepage: `#0c2340`, `#3b6fa0`, `#5a9bc7`, `#7bbce0`, `#a8d8ea`
- PDP: `#0c2340`, `#3b6fa0`, `#5a9bc7`, `#7bbce0`

### Repositorios e Deploy

| Repo | URL | Conteudo |
|------|-----|----------|
| `_GUDAY` | github.com/GuilhermeM/_GUDAY | Repo completo (privado) |
| `guday-presentation` | github.com/GuilhermeM/guday-presentation | So a pasta PRESENTATION (publico, conectado ao Vercel) |

**Push duplo (sempre fazer os dois):**
```bash
git push origin master && git subtree push --prefix PRESENTATION presentation main
```

O remote `presentation` ja esta configurado. O segundo comando sincroniza a pasta `PRESENTATION/` com o repo separado que o Vercel usa para deploy.

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
│   ├── competitor-analysis.html              # Apresentacao de concorrentes
│   ├── competitor-analysis-legado.html       # Backup pre-anotacoes
│   ├── DB - Competitors Research - Sheet1.csv  # Banco de dados (fonte)
│   ├── annotations.json                      # Anotacoes exportadas (opcional)
│   ├── favicon.svg                           # Favicon GUDAY
│   ├── _gummy/                               # Screenshots Gummy
│   ├── _essential/                           # Screenshots Essential
│   ├── _desincha/                            # Screenshots Desincha
│   ├── _drgood/                              # Screenshots Dr. Good
│   ├── _bold/                                # Screenshots Bold
│   └── _maismu/                              # Screenshots Mais Mu
├── USER-TESTING/
│   ├── user-testing-analysis.html            # Analise de usabilidade
│   ├── ut-edits.json                         # Edicoes de texto (opcional)
│   ├── User Testing - Guday.pdf              # Dados brutos (sessoes)
│   ├── Tabs - User Testing Analysis.pdf      # Referencia de formato
│   └── Supplend-User testing.pdf             # Referencia de formato
├── STAKEHOLDERS/
│   ├── stakeholders-analysis.html            # Analise de stakeholders
│   ├── stakeholders-edits.json               # Edicoes de texto (opcional)
│   └── Guday - Stakeholders.pdf              # Fonte de dados (entrevistas)
├── POST-PURCHASE/
│   ├── post-purchase-analysis.html           # Apresentacao pos-compra
│   ├── pp-edits.json                         # Edicoes exportadas (anotacoes + textos)
│   ├── DB - Post-Purchase - Sheet1.csv       # Dados brutos (backup)
│   ├── DB - Post-Purchase.xlsx               # Planilha original
│   └── MYND - Post Purchase Analysis.pdf     # Referencia de formato
├── ANALYTICS/
│   ├── analytics-analysis.html               # Apresentacao de funil (analytics)
│   ├── analytics-edits.json                  # Edicoes exportadas (opcional)
│   ├── Analytics Analysis.pptx               # Referencia original (WeConvert)
│   └── Analytics Analysis (1).pdf            # Referencia estetica (PDF)
├── presentation_documentation.md             # Este arquivo
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

---

# Post-Purchase Analysis

## Overview

Apresentacao de dados da pesquisa pos-compra da GUDAY. Os dados sao carregados ao vivo do Google Sheets — ao editar a planilha, basta recarregar a pagina.

**Arquivo:** `POST-PURCHASE/post-purchase-analysis.html`
**Referencia de formato:** `POST-PURCHASE/MYND - Post Purchase Analysis.pdf`

---

## Arquitetura

### Fonte de dados

Google Sheets publico:
- **Sheet ID:** `1rQ5VjGYa07zwi-utOK4dE2UmhIxVhSi1WqvcVHECgSs`
- **URL base:** `https://docs.google.com/spreadsheets/d/{SHEET_ID}/export?format=csv&gid={GID}`

### Abas do Google Sheets

| Aba | GID | Tipo | Descricao |
|-----|-----|------|-----------|
| dados brutos | 0 | raw | Respostas individuais (nao usado diretamente) |
| genero | 397488875 | bars | Distribuicao por genero |
| age | 381994185 | bars | Distribuicao por faixa etaria |
| canal | 2095508115 | bars | Como conheceu a Guday |
| interesse | 786515145 | bars | O que despertou interesse |
| fatores | 1945302614 | bars | Fatores de compra |
| dificuldade | 706090509 | bars | Dificuldades antes da Guday |
| nps | 622836116 | bars | Satisfacao geral (0-10) |
| recomendar | 1933124829 | bars | Probabilidade de recomendar (0-10) |
| objecoes | 839143174 | bars | Objecoes antes de comprar |
| naoRecom | 1185308463 | bars | Maior medo/preocupacao antes de comprar |
| comprarMais | 1350295206 | bars | O que convenceria a comprar mais |
| descricao | 2129416028 | quotes | Citacoes de clientes |

### Formato esperado das abas (bars)

```csv
Category,Percentage,Count
Feminino,86.11%,62
Masculino,13.89%,10
```

ou

```csv
Category,Count,Percentage
Nao gostar dos sabores,27,64.29%
```

O parser detecta a coluna `Percentage` pelo nome do header, independente da posicao.

### Formato esperado da aba (quotes)

```csv
Como voce descreveria?
"É uma forma muito gostosa de suplementacao..."
Parece um docinho e nao um suplemento.
```

Primeira linha = header (ignorada). Demais linhas = citacoes. Linhas vazias sao filtradas. Maximo 6 citacoes exibidas.

---

## Tipos de slides

### 1. Cover (capa)

Gradiente roxo GUDAY com titulo, subtitulo e metadados (n respostas, periodo).

### 2. Content (conclusoes + grafico de barras)

```
┌──────────────────────────────────────────────────────────────┐
│  TITULO (insight principal, bold)                            │
├────────────────────────────┬─────────────────────────────────┤
│  CONCLUSOES (50%)          │  GRAFICO BARRAS HORIZ. (50%)    │
│  • Bullet 1                │  ████████████████ 69.44%        │
│  • Bullet 2                │  ████ 15.28%                    │
│  • Bullet 3                │  ███ 9.72%                      │
├────────────────────────────┴─────────────────────────────────┤
│  Source: Post-purchase survey          [GUDAY logo]          │
└──────────────────────────────────────────────────────────────┘
```

- Barras ordenadas do maior para o menor
- Maximo 7 barras exibidas (restante cortado)
- Cor das barras: roxo `#7f56d9`
- Labels truncados com ellipsis (max 130px)

### 3. Dual-chart (dois graficos lado a lado)

```
┌──────────────────────────────────────────────────────────────┐
│  TITULO                                                      │
├────────────────────────────┬─────────────────────────────────┤
│  GRAFICO ESQUERDO          │  GRAFICO DIREITO                │
│  (pizza ou barras)         │  (barras)                       │
├────────────────────────────┴─────────────────────────────────┤
│  Source                                    [GUDAY logo]      │
└──────────────────────────────────────────────────────────────┘
```

- Propriedade `usePie: true` no slide para usar pizza na esquerda
- Usado para: genero (pizza) + idade (barras), NPS + recomendar (barras + barras)

### 4. Quotes (citacoes de clientes)

```
┌──────────────────────────────────────────────────────────────┐
│  TITULO                                                      │
│  Pergunta da pesquisa                                        │
│  ┌──────────────┐  ┌──────────────┐                         │
│  │ " Citacao 1  │  │ " Citacao 2  │                         │
│  └──────────────┘  └──────────────┘                         │
│  ┌──────────────┐  ┌──────────────┐                         │
│  │ " Citacao 3  │  │ " Citacao 4  │                         │
│  └──────────────┘  └──────────────┘                         │
├──────────────────────────────────────────────────────────────┤
│  Source                                    [GUDAY logo]      │
└──────────────────────────────────────────────────────────────┘
```

- Grid 2 colunas
- Cards com fundo `#f7f6f4` e aspas roxas
- Maximo 6 citacoes, filtradas por tamanho minimo (>3 chars)

---

## Slides atuais (11)

| # | Tipo | Titulo resumido | Dados |
|---|------|----------------|-------|
| 1 | cover | Post-Purchase Analysis | - |
| 2 | dual-chart (pizza+barras) | 86% mulheres, 35-44 dominante | genero + age |
| 3 | content | Instagram 69%, indicacao 15% | canal |
| 4 | content | Praticidade 28%, sabor 18% | interesse |
| 5 | content | Praticidade 40%, gostoso 32% | fatores |
| 6 | content | Gosto ruim 17%, formato 14% | dificuldade |
| 7 | dual-chart (barras+barras) | NPS 72, 84% nota 8+ | nps + recomendar |
| 8 | content | 43% sem objecao, 38% preco | objecoes |
| 9 | content | 64% medo do sabor | naoRecom |
| 10 | content | 24% satisfeitos, 35% querem promo | comprarMais |
| 11 | quotes | Pratica, gostosa, impossivel de esquecer | descricao |

---

## Fluxo de dados

1. No load, faz `fetch` de cada aba do Google Sheets via URL publica (em paralelo)
2. Parser detecta coluna `Percentage` pelo header (funciona com `Category,Percentage,Count` ou `Category,Count,Percentage`)
3. Dados ao vivo substituem os fallbacks hardcoded
4. Se o fetch falhar (offline, CORS), usa dados hardcoded como fallback
5. Barras sao ordenadas do maior ao menor e limitadas a 7
6. Citacoes sao filtradas (vazias removidas) e limitadas a 6

**Para atualizar:** edite a planilha no Google Sheets → recarregue a apresentacao

---

## Como adicionar novos slides

### 1. Criar aba no Google Sheets

Adicionar nova aba com formato `Category,Percentage,Count` (ou `Category,Count,Percentage`).

### 2. Descobrir o GID da aba

Abrir a aba no browser e copiar o `gid=` da URL. Ou:
```bash
curl -sL "https://docs.google.com/spreadsheets/d/SHEET_ID/htmlview" | grep -oE 'gid=[0-9]+' | sort -u
```

### 3. Adicionar no HTML

No array `SHEETS`, adicionar a nova chave:
```javascript
novaAba: { gid: "123456789" }
```

No array `slideDefs`, adicionar o slide:
```javascript
{
    type: "content",
    sheetKey: "novaAba",
    heading: "Titulo do insight principal",
    chartTitle: "Pergunta da pesquisa?",
    conclusions: [
        "Conclusao 1.",
        "Conclusao 2."
    ],
    fallbackBars: [
        { label: "Opcao A", value: 50 },
        { label: "Opcao B", value: 30 }
    ]
}
```

---

## Decisoes de design (Post-Purchase)

| Decisao | Motivo |
|---------|--------|
| Barras roxas `#7f56d9` | Consistencia com branding GUDAY |
| Max 7 barras por grafico | Evitar poluicao visual |
| Barras ordenadas maior→menor | Facilitar leitura |
| Labels truncados 130px | Evitar desalinhamento |
| Pizza so para genero | Melhor para dados binarios (2 opcoes) |
| Citacoes max 6, grid 2 colunas | Caber no slide sem scroll |
| Sem em-dashes nos textos | Evitar tom de IA, mais humanizado |
| Conclusoes sem dados redundantes | Nao repetir o que o grafico ja mostra |
| Dados ao vivo do Google Sheets | Atualizar sem editar codigo |
| Fallback hardcoded | Funcionar offline |

---

## Modo Edicao (Post-Purchase)

Mesmas features do competitor-analysis, com adicao de edicao de texto.

### Como usar

1. Clicar em **"Editar"** — botao fica vermelho, aparecem ferramentas
2. Dois modos de edicao (botoes "Retangulo" e "Texto"):
   - **Retangulo**: clicar e arrastar sobre graficos para destacar areas em vermelho
   - **Texto**: clicar em titulos, conclusoes ou perguntas para editar inline
3. **↺ Desfazer** — remove ultima acao
4. **🗑 Limpar** — remove todas edicoes do slide atual
5. **"Salvar"** — sai do modo edicao
6. **"JSON"** — exporta todas edicoes como `pp-edits.json`

### Persistencia

- **localStorage** salva automaticamente a cada edicao
- **`pp-edits.json`** e carregado via fetch no startup (prioridade sobre localStorage)
- Para compartilhar/deploy: exportar JSON, salvar na pasta `POST-PURCHASE/`, commitar no git
- No Vercel, o JSON e servido estaticamente e carregado no load

### Formato do pp-edits.json

```json
{
  "annotations": {
    "1": [{ "x": 51.88, "y": 23.22, "w": 49.81, "h": 11.24 }]
  },
  "texts": {
    "8|.conclusion-item:nth-child(1)": "Texto editado do primeiro bullet do slide 8"
  }
}
```

- **annotations**: chave = indice do slide, valor = array de retangulos em percentual
- **texts**: chave = `slideIdx|selectorCSS`, valor = texto editado
- Teclado desativado durante edicao de texto (espaco, setas nao navegam)
