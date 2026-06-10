# CLAUDE.md — site-industrial-ponzoni

## Visão geral

Mapa interativo de lotes do **Industrial Ponzoni** (Nova Prata - RS). Arquivo HTML standalone sem bundler ou dependências npm — abre direto no navegador ou qualquer servidor estático.

## Arquivo principal

`mapa-lotes-ponzoni-industrial.html` — único arquivo editável. Contém todo o CSS, SVG, dados dos lotes e JavaScript inline.

## Dependência externa

- **Panzoom 4.5.1** via CDN: `https://cdn.jsdelivr.net/npm/@panzoom/panzoom@4.5.1/dist/panzoom.min.js`
- **Status dos lotes** via Google Sheets (CSV público): variável `GS_URL` no script

## Estrutura interna do HTML

| Seção | Descrição |
|---|---|
| `<style>` | Todo o CSS, incluindo media queries para mobile (≤700px) |
| `#header` | Barra superior: logo, busca de lotes, contadores de status |
| `#legend` | Legenda fixa (canto superior esquerdo) |
| `#map-container` | Container do mapa com panzoom |
| `#svg-root` | SVG principal — imagem de fundo + polígonos dos lotes |
| `#info-panel` | Painel de detalhes do lote (desktop: lateral direita; mobile: bottom sheet) |
| `#social-box` | QR Code do Instagram (oculto no mobile) |
| `#footer` | Rodapé com links de Instagram e WhatsApp |
| `<script>` | Toda a lógica: carrega Sheets, inicializa Panzoom, busca, foco, painel |

## Lotes e quadras

44 lotes distribuídos em 5 quadras (A, B, C, D, E). Cada lote é um `<polygon>` SVG com atributos:
- `data-id` — identificador (ex: `C09`)
- `data-status` — `Livre` | `Vendido` | `Reservado` | `Nao Disponivel`
- `data-area` — área em m²
- `data-quadra` — letra da quadra
- `data-valor` — valor formatado (ex: `R$ 370.084,36`)

## Status dos lotes

Os status são atualizados dinamicamente a partir do Google Sheets (`GS_URL`). O HTML contém os valores padrão (fallback caso o Sheets falhe).

## Enquadramento do mapa

O `resetView()` foca na região dos lotes ao abrir, independente do tamanho de tela:

```js
var EW=3192, EH=1858;          // dimensões do SVG completo
var LOTS={x:890,y:110,w:1560,h:1600}; // bounding box dos lotes no SVG
```

O Panzoom usa `transform-origin: 50% 50%` — a função `centerOn(px, py, s, anim)` leva isso em conta ao calcular o pan.

## Contato / WhatsApp

Número configurado na variável `WA_NUM` no script:
```js
var WA_NUM = '5554996098638';
```

## Hospedagem

GitHub Pages — repositório `gustavo1209-ship-it/site-industrial-ponzoni`, branch `main`. O HTML é incorporado no site Webflow via `<iframe>`.

## Fluxo para atualizar status de lotes

1. Editar a planilha do Google Sheets (o mapa recarrega os dados automaticamente).
2. Para mudanças estruturais (novos lotes, layout), editar o HTML e fazer push para `main`.

## Testar localmente

```bash
python -m http.server 8123
# abrir http://localhost:8123/mapa-lotes-ponzoni-industrial.html
```
