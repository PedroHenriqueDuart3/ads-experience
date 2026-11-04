# Tela 01 — Dashboard Geral de Análise (Prevenção a Fraudes)

> Especificação para gerar a tela no protótipo (pasta do projeto) e no Figma.
> Produto: app mobile interno para **analistas de prevenção a fraudes** do PicPay.
> Desafio: ADS Experience 2026 — Desafio 02.

---

## 1. Objetivo da tela

Dar ao analista, em **um único scroll**, a visão geral da operação de risco no momento:

- quanto há para analisar e quanto está prestes a estourar o SLA;
- onde o risco está concentrado (tipo de transação, sinal de risco, horário, região);
- como a operação está performando (tempo de decisão, falsos positivos, fraude confirmada).

Tudo **filtrável** (filtros globais + filtro cruzado tocando nos gráficos) e **atualizando em tempo real** com dados mockados.

A tela responde a três perguntas, nesta ordem de leitura:

1. **"Estou em risco agora?"** → KPIs e faixa de SLA (topo).
2. **"Onde está o problema?"** → gráficos de distribuição (meio).
3. **"O que está chegando?"** → feed ao vivo (final), com atalho para analisar.

---

## 2. Especificações de plataforma (mobile)

| Item | Valor |
|---|---|
| Frame base | **390 × 844 pt** (iPhone 14/15). Testar também em 360 × 800 (Android médio) |
| Orientação | Somente retrato |
| Safe area | Topo 47 pt (status bar), base 34 pt (home indicator) |
| Margem lateral | 16 pt |
| Grid | 4 pt; espaçamentos usados: 4, 8, 12, 16, 24, 32 |
| Largura útil | 358 pt (390 − 2×16) |
| Área mínima de toque | **44 × 44 pt** (iOS) / 48 × 48 dp (Android) — vale para chips, barras de gráfico e ícones |
| Rolagem | Vertical única. Carrosséis horizontais só nos chips de filtro e nos KPIs |
| Header | Fixo (sticky) com a barra de filtros logo abaixo, que também fica fixa ao rolar |
| Navegação | Tab bar inferior fixa, 5 itens |
| Gestos | Pull-to-refresh, swipe horizontal nos KPIs, toque longo em gráfico = tooltip, toque simples = filtro cruzado |
| Altura total estimada do conteúdo | ~2.300 pt (≈ 3 telas de scroll) |

---

## 3. Paleta de cores

### 3.1 Marca (inspirada no app PicPay)

> O verde é o verde característico da marca PicPay. Confirme os valores exatos no brand guide oficial antes da entrega; os demais tons foram derivados para cumprir contraste WCAG.

| Token | Hex | Uso |
|---|---|---|
| `brand-500` | `#11C76F` | Verde PicPay — preenchimento de botão primário, aba ativa, indicador "Ao vivo", toggles |
| `brand-600` | `#0A8A4C` | Pressed do botão primário |
| `brand-700` | `#087A43` | **Texto/ícone verde sobre branco** (5,4:1). Nunca usar `brand-500` como texto em fundo claro (2,2:1 — reprova) |
| `brand-50` | `#E7F9F0` | Fundo de chip selecionado, destaque suave |
| `on-brand` | `#0B0B0B` | Texto sobre `brand-500` (8,8:1) |

### 3.2 Neutros — tema claro (padrão)

| Token | Hex | Uso |
|---|---|---|
| `bg-page` | `#F5F5F7` | Fundo da tela |
| `bg-surface` | `#FFFFFF` | Cards, gráficos, bottom sheets |
| `text-primary` | `#0B0B0B` | Títulos, valores |
| `text-secondary` | `#52514E` | Subtítulos, legendas |
| `text-muted` | `#6B6B70` | Eixos, timestamps (5,3:1) |
| `border` | `#E5E5EA` | Bordas de card (1 pt), divisórias |
| `grid` | `#ECECEF` | Linhas de grade dos gráficos (hairline 1 pt) |
| `baseline` | `#C3C2B7` | Eixo base dos gráficos |

### 3.3 Neutros — tema escuro (opcional, mesmo layout)

| Token | Hex |
|---|---|
| `bg-page` | `#000000` |
| `bg-surface` | `#1C1C1E` |
| `text-primary` | `#F0EFEC` |
| `text-secondary` | `#C3C2B7` |
| `text-muted` | `#8E8E93` |
| `border` / `grid` | `#2C2C2E` |
| `brand-500` (texto também) | `#11C76F` (7,6:1 sobre `#1C1C1E`) |

### 3.4 Status de risco (reservados — nunca usar para outra coisa)

Sempre acompanhados de **ícone + rótulo**, nunca só cor.

| Nível | Fundo/marca | Texto sobre branco | Ícone | Rótulo |
|---|---|---|---|---|
| Alto | `#D03B3B` | `#D03B3B` (4,8:1) | `alert-octagon` | "Alto risco" |
| Médio | `#FAB219` | `#A6620A` (4,8:1) | `alert-triangle` | "Médio risco" |
| Baixo | `#0CA30C` | `#087A43` | `shield-check` | "Baixo risco" |

Badge de risco: altura 24 pt, raio 12 pt, fundo com o tom a 12% de opacidade, ícone 14 pt + texto 12 pt semibold na cor de texto da tabela.

### 3.5 Cores de gráfico por tipo de transação (categóricas)

Validadas com o script de daltonismo/contraste (todas as combinações aprovadas nos dois temas):

| Tipo | Claro | Escuro |
|---|---|---|
| Pix | `#2A78D6` (azul) | `#3987E5` |
| Pagamento em loja | `#EB6834` (laranja) | `#D95926` |
| Compra online | `#1BAF7A` (água) | `#199E70` |
| Outros (boleto, TED, recarga) | `#B5B5BA` (cinza) | `#5A5A5E` |

Regras:
- **A cor acompanha o tipo, não a posição.** Se um filtro remove "Pix", as outras cores não mudam.
- O água no tema claro fica abaixo de 3:1 → **rótulos diretos visíveis são obrigatórios** nos gráficos (ou tabela alternativa).
- Gráficos de risco usam as cores de status (3.4); gráficos por tipo usam as categóricas (3.5). Nunca misturar as duas no mesmo gráfico.

### 3.6 Escala sequencial (heatmap)

Um só tom, azul, do claro ao escuro: `#CDE2FB` → `#86B6EF` → `#3987E5` → `#256ABF` → `#184F95` → `#0D366B`. Zero alertas = `bg-page`.

---

## 4. Tipografia

Fonte do sistema: **SF Pro** (iOS) / **Roboto** (Android). No Figma, usar Inter como substituta.

| Estilo | Tamanho / altura de linha | Peso | Uso |
|---|---|---|---|
| `display` | 32 / 38 | Bold | Número principal do KPI em destaque |
| `title-1` | 22 / 28 | Bold | Título da tela |
| `title-2` | 17 / 22 | Semibold | Título de seção/card |
| `kpi-value` | 24 / 30 | Bold | Valor dos KPIs |
| `body` | 15 / 20 | Regular | Texto geral |
| `label` | 13 / 18 | Medium | Chips, legendas, rótulos |
| `caption` | 11 / 14 | Regular | Eixos, timestamps |

Números em colunas e eixos: `tabular-nums`. Valores monetários: `R$ 12.430,00` (pt-BR); acima de 100 mil, abreviar: `R$ 1,2 mi`.

---

## 5. Estrutura da tela (de cima para baixo)

```
┌──────────────────────────────────┐
│ Status bar                       │
├──────────────────────────────────┤
│ A. HEADER (sticky)               │
│ B. BARRA DE FILTROS (sticky)     │
├──────────────────────────────────┤
│ C. Faixa de SLA crítico          │
│ D. Carrossel de KPIs             │
│ E. Alertas ao longo do tempo     │
│ F. Distribuição por risco        │
│ G. Alertas por tipo              │
│ H. Top sinais de risco           │
│ I. Heatmap hora × dia            │
│ J. Regiões com mais alertas      │
│ K. Desempenho da equipe          │
│ L. Feed ao vivo                  │
├──────────────────────────────────┤
│ M. TAB BAR (fixa)                │
└──────────────────────────────────┘
```

Cards: fundo `bg-surface`, raio **16 pt**, padding 16 pt, borda 1 pt `border`, sem sombra (ou sombra `0 1 2 rgba(0,0,0,0.04)`). Espaço entre cards: 12 pt. Cada card de gráfico tem: título (`title-2`), subtítulo com o recorte ativo (`label`, `text-secondary`, ex.: "Hoje · Pix + Online") e menu `⋯` (44×44) com: *Ver como tabela*, *Expandir*, *Exportar*.

---

### A. Header (sticky, 56 pt + status bar)

- Esquerda: título **"Visão geral"** (`title-1`).
- Logo abaixo do título, o indicador **"● Ao vivo · atualizado há 3 s"**:
  - ponto de 8 pt `brand-500` com animação de pulso (escala 1 → 1,6, opacidade 1 → 0, 1,5 s, em loop);
  - texto `caption`, `text-muted`; o contador de segundos sobe a cada 1 s e volta a 0 a cada atualização.
  - Quando pausado: ponto cinza, sem pulso, texto "Pausado · dados de 14:32:05".
- Direita (ícones de 24 pt em área de toque de 44 pt):
  1. **Pausar/retomar** o tempo real (`pause` / `play`).
  2. **Notificações** (`bell`) com badge vermelho mostrando o nº de alertas altos novos desde a última visualização.
  3. **Avatar** do analista (32 pt) → perfil/turno.
- Ao rolar: o título encolhe para 17 pt e o header ganha a borda inferior `border`.

### B. Barra de filtros (sticky, 52 pt)

Linha horizontal rolável de chips (altura 36 pt, raio 18 pt, padding 12 pt, espaço de 8 pt):

| Ordem | Chip | Opções | Padrão |
|---|---|---|---|
| 1 | Ícone `sliders` "Filtros (n)" | Abre o bottom sheet completo | — |
| 2 | **Período** | Última 1 h · Hoje · 7 dias · 30 dias · Personalizado | Hoje |
| 3 | **Risco** | Alto · Médio · Baixo (seleção múltipla) | Todos |
| 4 | **Tipo** | Pix · Loja · Online · Outros (múltipla) | Todos |
| 5 | **Status** | Novo · Em análise · Aguardando cliente · Resolvido | Abertos (Novo + Em análise + Aguardando) |
| 6 | **Sinal** | Novo beneficiário · Localização incomum · Múltiplas tentativas · Padrão atípico · Dispositivo novo | Todos |
| 7 | **Valor** | Faixas: até R$ 500 · 500–5 mil · 5–20 mil · > 20 mil | Todos |
| 8 | **Região** | UFs/regiões | Todas |
| 9 | **Analista** | Eu · Minha equipe · Todos | Minha equipe |

Estados do chip:
- Padrão: fundo `bg-surface`, borda `border`, texto `text-primary`, chevron `▾`.
- Ativo: fundo `brand-50`, borda `brand-700`, texto `brand-700` semibold, mostrando o valor ("Risco: Alto, Médio").
- Pressed: escurece 4%.

Quando houver filtro ativo, aparece um chip final **"Limpar"** (texto `brand-700`, sem borda).

**Filtro cruzado** (vindo do toque em gráfico): aparece como chip ativo com ícone `link` (ex.: "🔗 14h–15h") e pode ser removido pelo `×`.

**Bottom sheet "Filtros"** (90% da altura, raio 24 pt no topo, alça de 36×4 pt):
- Seções em acordeão, uma por dimensão; seleção por checkbox; período com presets em lista (check de 16 pt no item selecionado) e "Personalizado" com calendário.
- Rodapé fixo: botão secundário "Limpar tudo" + botão primário **"Ver 128 alertas"**. A contagem é recalculada em tempo real conforme o usuário marca as opções.
- Botão primário: altura 52 pt, raio 26 pt, fundo `brand-500`, texto `on-brand` 16 pt semibold.

**Visões salvas:** no topo do sheet, chips "Meus alertas altos", "Pix noturno", "+ Salvar visão atual".

Todos os filtros são **globais**: valem para todos os cards da tela.

### C. Faixa de SLA crítico (aparece só se houver SLA vencendo)

- Card de largura total, 64 pt, fundo `#D03B3B` a 10%, borda esquerda de 4 pt `#D03B3B`.
- Ícone `timer` + texto **"7 alertas vencem nos próximos 15 min"** (`body` semibold) + "R$ 84,3 mil em jogo" (`label`, `text-secondary`).
- À direita: botão de texto **"Analisar"** → abre a fila já filtrada por SLA.
- Quando um alerta novo entra nessa faixa: o número pisca uma vez (escala 1 → 1,15 → 1, 300 ms) + vibração leve (haptic `light`).

### D. Carrossel de KPIs

Rolagem horizontal com *snap*. Cards de **160 × 112 pt** (cabem 2,2 na tela, sinalizando que tem mais). Indicador de páginas com pontos abaixo.

| # | KPI | Exemplo | Comparação | Mini-gráfico |
|---|---|---|---|---|
| 1 | Alertas abertos | **342** | ▲ 12% vs. ontem no mesmo horário | sparkline da última 1 h |
| 2 | Alto risco abertos | **48** | ▲ 5 na última hora | sparkline |
| 3 | Valor em risco | **R$ 1,8 mi** | soma dos abertos | — |
| 4 | Tempo médio de decisão | **3 min 42 s** | ▼ 18% (bom) | sparkline |
| 5 | SLA cumprido | **94,2%** | meta 95% → barra de progresso | barra |
| 6 | Fraude confirmada | **6,8%** dos alertas resolvidos | ▲ 0,4 pp | — |
| 7 | Falsos positivos | **61%** | ▼ 3 pp (bom) | — |
| 8 | Valor bloqueado (fraude evitada) | **R$ 412 mil** | hoje | — |

Anatomia do card de KPI:
- Rótulo (`label`, `text-secondary`) no topo.
- Valor (`kpi-value`, `text-primary`).
- Variação: seta + % em `caption`. Verde `brand-700` quando a mudança é boa e vermelho `#D03B3B` quando é ruim, **sempre com seta** (nunca só cor). "Bom" depende do KPI: mais alertas = ruim, menos tempo de decisão = bom.
- Sparkline: 1,5 pt, `text-muted`, sem eixos; último ponto com marcador de 6 pt.
- Toque no card → filtra a tela por aquele recorte (ex.: KPI 2 aplica Risco = Alto).
- Atualização ao vivo: o número **conta** até o novo valor (300 ms, ease-out) e a variação pisca o fundo do card com `brand-50` (ou vermelho a 8%, se piorou) por 600 ms.

### E. Alertas ao longo do tempo (gráfico de linha/área)

- Card de 358 × 260 pt. Título "Alertas recebidos", subtítulo com o período.
- Seletor segmentado no topo do card: **1 h · 24 h · 7 d** (altura 32 pt).
- **Área empilhada por nível de risco** (Alto embaixo, Médio, Baixo em cima) com as cores de status: preenchimento a 24%, linha superior de 2 pt na cor cheia, 2 pt de espaço da cor da superfície entre as camadas.
- Eixo X: horários (`caption`, 4–6 marcações). Eixo Y à esquerda, 3–4 linhas de grade hairline. **Um único eixo Y.**
- Linha tracejada horizontal "média dos últimos 7 dias" em `text-muted`, com rótulo direto à direita.
- Rótulos diretos no fim de cada camada ("Alto 48") + legenda acima do gráfico com bolinha, ícone e nome.
- **Interação:**
  - Arrastar o dedo = crosshair vertical + tooltip flutuante com horário, contagem por nível e total.
  - Tocar e soltar num ponto = filtro cruzado daquele intervalo (ex.: "14h–15h").
  - Pinça horizontal = zoom no tempo (opcional).
- **Tempo real:** a cada atualização, o último ponto cresce e a linha desliza 1 posição para a esquerda (transição de 400 ms). Não redesenhar o gráfico inteiro.

### F. Distribuição por nível de risco

- **Barra horizontal segmentada** de largura total (altura 16 pt, raio 4 pt nas pontas, 2 pt de espaço entre os segmentos) — não usar donut.
- Abaixo, 3 linhas com badge de risco, contagem, % e valor total:
  ```
  [⬣ Alto]   48   14%   R$ 1,1 mi
  [▲ Médio]  126  37%   R$ 520 mil
  [✓ Baixo]  168  49%   R$ 180 mil
  ```
- Toque em um segmento ou linha → filtro cruzado "Risco: Alto". Os outros segmentos ficam com 30% de opacidade.

### G. Alertas por tipo de transação

- Barras horizontais ordenadas por quantidade (Pix, Loja, Online, Outros), com as cores categóricas da seção 3.5.
- Barra: altura 20 pt, raio 4 pt na ponta, 12 pt entre barras. Rótulo do tipo à esquerda (`label`), valor à direita da barra ("142 · R$ 980 mil").
- Alternância no topo do card: **Quantidade | Valor (R$)**.
- Barra fina de fundo `grid` atrás de cada barra, até 100%, para dar escala.
- Toque → filtro cruzado por tipo.

### H. Top sinais de risco

- Lista ranqueada (top 5) em formato de barras horizontais de uma só cor (`#2A78D6`), ordenadas por ocorrência:
  1. Novo beneficiário — 96
  2. Múltiplas tentativas — 71
  3. Localização incomum — 58
  4. Dispositivo novo — 44
  5. Padrão atípico de valor — 39
- Cada linha mostra também a **taxa de fraude confirmada** do sinal ("23% confirmadas") em `caption`. É o dado que mostra ao analista qual sinal realmente importa.
- Ícone por sinal (`user-plus`, `repeat`, `map-pin`, `smartphone`, `trending-up`).
- Toque → filtro cruzado por sinal. Link "Ver todos (12)" no rodapé.

### I. Heatmap hora × dia da semana

- Grade de 7 colunas (Seg–Dom) × 24 linhas recolhidas em **8 faixas de 3 h** (00–03, 03–06, …), para caber no mobile.
- Células de 40 × 24 pt, 2 pt de espaço, raio 4 pt, escala sequencial azul (3.6).
- Legenda de escala abaixo ("menos ▭▭▭▭▭ mais").
- A célula "agora" tem anel de 2 pt `text-primary`.
- Tocar numa célula → tooltip ("Qua, 21h–00h: 87 alertas · 12 altos") + opção "Filtrar este horário".
- Insight automático acima da grade em `label`: *"Pico de alertas altos entre 21h e 00h, principalmente em Pix."*

### J. Regiões com mais alertas

- Ranking em tabela compacta (não usar mapa: fica ilegível em 358 pt):
  | UF | Alertas | Alto | Variação |
  |---|---|---|---|
  | SP | 98 | 14 | ▲ 8% |
  | RJ | 54 | 11 | ▲ 22% |
  | MG | 31 | 4 | ▼ 3% |
- Linhas de 44 pt; a variação usa seta + cor.
- Toque → filtro cruzado por UF.

### K. Desempenho da equipe (turno atual)

- 3 mini-indicadores lado a lado: **Resolvidos hoje 214** · **Em análise agora 19** · **Analistas online 8**.
- Ranking curto "Tempo médio por analista" (barras de uma cor, o analista logado destacado com `brand-500` e os demais em `#B5B5BA`). A ideia é dar contexto ao analista, não criar competição: nomes abreviados.

### L. Feed ao vivo

- Título "Chegando agora" + ponto pulsante verde.
- Lista com os **5 últimos alertas** que passam nos filtros. Item de 72 pt:
  ```
  [ícone tipo]  Pix · R$ 8.450,00            [⬣ Alto]
                Novo beneficiário · SP          há 12 s
  ```
  - Ícone do tipo em círculo de 40 pt com fundo da cor categórica a 12%.
  - Valor em `body` semibold; sinal + UF em `label` `text-secondary`; tempo relativo em `caption`.
- **Animação de entrada:** o novo item desliza do topo (translateY −16 → 0, opacidade 0 → 1, 250 ms), o fundo fica `brand-50` por 1,2 s e o último item da lista sai. Alertas altos entram com borda esquerda vermelha de 3 pt.
- Swipe para a esquerda num item → ações rápidas "Assumir" (verde) / "Escalar" (cinza).
- Toque → detalhe do alerta (Tela 03).
- Rodapé: botão primário de largura total **"Analisar próximo alerta"** (52 pt, `brand-500`), que leva à fila priorizada.

### M. Tab bar (fixa, 56 pt + home indicator)

| Ícone | Rótulo | Estado |
|---|---|---|
| `layout-dashboard` | **Visão geral** | ativo (`brand-700` no claro, `brand-500` no escuro) |
| `list-checks` | Fila | badge com nº de alertas altos |
| `folder-search` | Casos | — |
| `bar-chart-3` | Relatórios | — |
| `user` | Perfil | — |

Inativos em `text-muted`. Ícones de 24 pt, rótulo de 11 pt.

---

## 6. Dados mockados em tempo real

### 6.1 Motor de simulação

- **Intervalo:** a cada **5 s** (configurável: 3 / 5 / 10 s no Perfil), o motor gera de **0 a 3 alertas novos** e resolve de **0 a 2 alertas abertos**.
- Os KPIs, gráficos e o feed são **recalculados a partir da lista de alertas filtrada**, nunca de números soltos. Assim os filtros e os gráficos sempre batem entre si.
- **Pausar** congela o motor; ao retomar, aplica as mudanças acumuladas de uma vez, com um toast "+9 alertas enquanto pausado".
- **Pull-to-refresh** força um ciclo imediato.
- Base inicial: **~400 alertas** gerados para as últimas 24 h, com mais volume à noite (21h–00h) e em Pix.

### 6.2 Modelo de um alerta

```json
{
  "id": "ALT-2026-118734",
  "createdAt": "2026-11-04T14:32:05-03:00",
  "type": "pix",                     // pix | loja | online | outros
  "amount": 8450.00,
  "riskLevel": "alto",               // alto | medio | baixo
  "riskScore": 87,                   // 0–100
  "signals": ["novo_beneficiario", "valor_atipico"],
  "status": "novo",                  // novo | em_analise | aguardando_cliente | resolvido
  "resolution": null,                // fraude_confirmada | legitima | null
  "slaDeadline": "2026-11-04T14:47:05-03:00",
  "customer": { "id": "C-55821", "name": "M. Souza", "uf": "SP", "accountAgeDays": 12 },
  "analystId": null,
  "decisionSeconds": null
}
```

### 6.3 Regras de geração (para os dados parecerem reais)

| Campo | Distribuição |
|---|---|
| `type` | Pix 45% · Loja 25% · Online 22% · Outros 8% |
| `riskLevel` | Alto 15% · Médio 35% · Baixo 50% |
| `amount` | Log-normal: mediana R$ 380; alto risco puxa para valores 3× maiores; teto R$ 50 mil |
| `signals` | 1–3 por alerta; "novo beneficiário" mais comum em Pix; "localização incomum" em loja; "múltiplas tentativas" em online |
| `uf` | Proporcional à população (SP 22%, MG 10%, RJ 8%, BA 7%…) |
| SLA | Alto 15 min · Médio 1 h · Baixo 4 h a partir do `createdAt` |
| Resolução | Fraude confirmada: 40% dos altos, 8% dos médios, 1% dos baixos |
| `decisionSeconds` | Normal com média de 220 s e desvio de 90 s |
| Picos | Multiplicador de volume ×1,8 entre 21h e 00h; um "surto" aleatório a cada ~2 min (5 alertas altos do mesmo beneficiário, para mostrar o agrupamento em caso) |

### 6.4 Regras visuais da atualização

- Nunca recarregar a tela inteira nem mostrar skeleton depois da primeira carga; só animar o que mudou.
- Números: contagem animada de 300 ms. Gráficos: transição de 400 ms. Itens novos no feed: 250 ms.
- Respeitar **"Reduzir movimento"** do sistema: sem pulso nem contagem animada, troca direta de valores.
- Se o usuário estiver interagindo (tooltip aberto, arrastando um gráfico), **segurar a atualização** daquele card até soltar.

---

## 7. Comportamento dos filtros

1. **Globais:** todo filtro da barra B afeta todos os cards (C a L). O subtítulo de cada card reflete o recorte.
2. **Filtro cruzado:** tocar em uma marca de gráfico (segmento, barra, célula, linha) adiciona um filtro. Vários filtros cruzados se somam (E lógico). Feedback: haptic `selection` + chip novo na barra B.
3. **Contagem viva:** o botão do bottom sheet mostra quantos alertas restam antes de aplicar.
4. **Sem resultados:** se um filtro zerar a lista, cada card mostra um estado vazio (ver 8), nunca um gráfico vazio com eixos.
5. **Persistência:** filtros e período ficam salvos ao sair e voltar para a tela; o "Limpar" volta ao padrão.
6. **Cores estáveis:** aplicar ou remover um filtro nunca muda a cor de um tipo ou nível.

---

## 8. Estados da tela

| Estado | Comportamento |
|---|---|
| **Carregando (1ª vez)** | Skeleton dos cards com shimmer (cinza `#ECECEF` → `#F5F5F7`, 1,2 s). Header e filtros já interativos |
| **Atualizando** | Sem skeleton; só o contador "atualizado há Xs" e as animações |
| **Vazio por filtro** | Ilustração simples + "Nenhum alerta com esses filtros" + botão "Limpar filtros" |
| **Sem alertas abertos** | Card de comemoração: ícone `shield-check` verde + "Tudo em dia! Nenhum alerta aberto." |
| **Offline** | Faixa amarela no topo: "Sem conexão · exibindo dados de 14:32". Indicador "Ao vivo" vira cinza |
| **Erro** | Card com "Não foi possível atualizar" + "Tentar novamente"; os dados anteriores continuam na tela |
| **Muitos alertas altos** (> 60 abertos) | Faixa C muda para vermelho sólido com texto branco: "Volume crítico — considere acionar o plantão" |

---

## 9. Acessibilidade

- Contraste mínimo de 4,5:1 para texto e 3:1 para ícones e marcas de gráfico (tokens já calculados na seção 3).
- Risco e variação **nunca só por cor**: sempre ícone + rótulo/seta.
- Todo gráfico tem **"Ver como tabela"** no menu `⋯`, com os mesmos dados.
- Leitores de tela: cada gráfico com resumo textual (ex.: *"Alertas recebidos hoje: 342, pico às 22h com 41 alertas, tendência de alta"*). O feed ao vivo anuncia apenas alertas **altos** (`aria-live="polite"`), para não sobrecarregar.
- Suporte a fonte dinâmica até 130%: cards de KPI quebram em 2 linhas e o carrossel continua funcionando.
- Áreas de toque mínimas de 44 pt, inclusive barras finas de gráfico (área invisível ampliada).

---

## 10. Navegação a partir desta tela

| Origem | Destino |
|---|---|
| Faixa SLA → "Analisar" | Tela 02 — Fila (filtro: SLA < 15 min) |
| KPI tocado | Filtra o próprio dashboard |
| Item do feed | Tela 03 — Detalhe do alerta |
| "Analisar próximo alerta" | Tela 03 com o alerta de maior score |
| Sino | Central de notificações |
| `⋯` → Expandir | Gráfico em tela cheia (com opção de girar para paisagem) |

---

## 11. Checklist para gerar no Figma

- [ ] Frame `01 — Dashboard Geral` 390 × 844, com uma versão "scroll completo" de ~390 × 2300.
- [ ] Variáveis de cor criadas a partir da seção 3 (modos **Light** e **Dark**).
- [ ] Estilos de texto da seção 4.
- [ ] Componentes com variantes: `Chip` (padrão/ativo/cruzado), `RiskBadge` (alto/médio/baixo), `KpiCard` (melhorou/piorou/neutro), `FeedItem` (normal/novo/alto), `TabBar`.
- [ ] Auto layout em todos os cards (padding 16, gap 12).
- [ ] Variações de estado: Carregando, Vazio por filtro, Offline, Bottom sheet de filtros aberto, Tooltip aberto no gráfico E.
- [ ] Protótipo: chip → bottom sheet; toque em barra → chip cruzado aparece; item do feed → Tela 03.
- [ ] Na pasta do projeto: versão HTML/CSS/JS clicável com o motor de mock da seção 6 rodando de verdade.
