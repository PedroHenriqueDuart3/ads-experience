# Protótipo mobile — Clientes com sinais de fraude | PicPay

## Objetivo

Crie um protótipo **mobile de alta fidelidade**, em português do Brasil, para uso interno da equipe de **gestão de riscos e prevenção a fraudes do PicPay**. A interface deve ajudar o analista a identificar clientes com maior concentração de transações suspeitas, consultar os sinais que justificam a prioridade e tomar uma decisão registrada.

O protótipo deve conter **duas telas navegáveis**: (1) lista de clientes priorizados e (2) detalhes do cliente. Inclua também os estados de confirmação das ações. Use dados fictícios.

## Identidade visual

- Linguagem visual inspirada no PicPay: simples, limpa, amigável, com cantos arredondados e bastante espaço em branco.
- Verde principal: `#00BC54`. Use em destaques, indicadores e no botão de ação principal, com **texto escuro** para preservar a leitura.
- Verde escuro: `#238662`. Use em textos ou elementos que precisem de contraste maior.
- Texto principal: `#121212`; superfícies: `#FFFFFF`; fundo da tela: `#F6F8F7`; bordas: `#E5EAE7`.
- Estados de risco devem ter **rótulo textual além da cor**: alto `#B42318` sobre fundo `#FEECEB`; médio `#9A6700` sobre fundo `#FFF4D6`; baixo `#238662` sobre fundo `#E8F7EE`.
- Não use o verde como indicação de fraude. O verde representa a marca e ações de continuidade; a severidade aparece nos chips de risco.
- Tipografia sem serifa, próxima à linguagem visual do PicPay; caso a fonte proprietária não esteja disponível, use Inter ou uma fonte de sistema.
- Referência de cores da marca: [Brandfetch — PicPay](https://brandfetch.com/picpay.com). Os hexadecimais são uma referência visual para o protótipo, não uma especificação oficial de marca.

## Formato e navegação

- Desenhe para largura de referência de **390 px**, com rolagem vertical e áreas de toque de pelo menos **44 × 44 px**.
- Respeite as áreas seguras do aparelho. Mantenha os botões decisórios visíveis em uma barra fixa inferior na tela de detalhes.
- Um toque no card ou no nome abre os detalhes; o botão de voltar retorna à lista preservando busca, filtros e posição da rolagem.
- Use ícones simples acompanhados por texto. Não dependa apenas de cor ou ícone para transmitir status.

## Tela 1 — Clientes priorizados

### Cabeçalho

- Identificação discreta **PicPay | Gestão de risco**.
- Título: **Clientes em atenção**.
- Subtítulo: **Priorize casos com mais transações suspeitas**.
- Indicador de atualização: **Atualizado às 10:42**.

### Controles

- Campo de busca: **Buscar por nome ou ID do cliente**.
- Filtros em chips: **Todos**, **Alto risco**, **Médio risco**, **Em análise**.
- Ordenação padrão: **Mais transações suspeitas**. Permita trocar para **Maior valor exposto** e **Mais recentes**.
- Resumo compacto acima da lista: **24 clientes em atenção · 8 de alto risco**.

### Cards da lista

Mostre 4 a 5 cards fictícios. Cada card deve trazer:

1. Nome do cliente e ID parcialmente mascarado.
2. Chip com nível de risco e status do caso, por exemplo **Alto risco · Pendente**.
3. Quantidade de transações **sinalizadas** no período, em destaque.
4. Valor total envolvido, período analisado e horário do último alerta.
5. Um motivo curto e concreto: **5 tentativas em 12 min**, **beneficiário novo**, **localização incomum**.
6. Seta ou indicação **Ver detalhes**.

Exemplo do primeiro card:

> **Mariana Oliveira** · ID **•••• 4821**  
> **Alto risco** · **Pendente**  
> **12 transações sinalizadas** nos últimos 7 dias  
> **R$ 18.450,00** envolvidos · último alerta hoje, 10:38  
> Sinais: 5 tentativas em 12 min · 3 beneficiários novos

Os demais cards devem ter números e sinais variados. Ordene pelos casos que exigem atenção primeiro. Não trate um alerta como fraude confirmada.

## Tela 2 — Detalhes do cliente

Ao tocar em um card, abra uma tela completa com:

### Identificação e panorama

- Nome, ID mascarado, status da conta e status do caso.
- Chip de risco **Alto**, **Médio** ou **Baixo**, acompanhado de texto explicativo: **Sinais identificados automaticamente; requer avaliação humana**.
- Três indicadores de leitura rápida: **transações sinalizadas**, **valor envolvido** e **último alerta**.
- Período considerado, por exemplo **Últimos 7 dias**, e data/hora da atualização.

### Motivos do alerta

Mostre os sinais de forma verificável, sem afirmar culpa:

- **5 tentativas de Pix em 12 minutos**;
- **3 beneficiários cadastrados recentemente**;
- **Acesso de dispositivo ainda não reconhecido**.

### Transações recentes sinalizadas

Lista curta com tipo, valor, data/hora, situação e sinal associado. Exemplo: **Pix · R$ 2.400,00 · hoje 10:38 · retida para análise · beneficiário novo**. Cada item pode expandir para mostrar ID da transação mascarado e detalhes úteis à investigação.

### Histórico de decisão

Exiba uma linha do tempo simples com alertas anteriores e decisões já registradas, se houver. Para um caso novo, mostre **Nenhuma decisão registrada neste caso**.

### Ações fixas no rodapé

- Botão primário: **Enviar para análise**. Abre um painel de confirmação com campo obrigatório **Motivo/observação**, e confirma com **Enviar caso**. Após confirmar, atualiza o status para **Em análise** e mostra um aviso de sucesso.
- Botão secundário de atenção: **Bloquear cliente**. Abre uma confirmação que explica o impacto do bloqueio, exige um motivo e apresenta os botões **Cancelar** e **Confirmar bloqueio**. Após confirmar, atualiza o status para **Bloqueado**, registra a ação no histórico e mostra confirmação clara.
- Desabilite ações incompatíveis com o status atual; não permita enviar novamente um caso já em análise nem bloquear novamente um cliente já bloqueado.

## Estados adicionais

- **Carregando:** esqueletos dos cards, sem saltos bruscos de layout.
- **Busca sem resultados:** mensagem **Nenhum cliente encontrado** e opção de limpar busca/filtros.
- **Lista vazia:** mensagem **Nenhum caso pendente no momento**.
- **Erro ao carregar ou confirmar:** texto claro com botão **Tentar novamente**, sem indicar que a ação foi concluída.
- **Acessibilidade:** contraste legível, foco visível, rótulos para leitor de tela, números formatados em `pt-BR` e status descritos por texto.

## Critérios de aceite do protótipo

1. A lista evidencia quem precisa de atenção, com quantidade de transações sinalizadas, severidade e motivo resumido.
2. Um toque abre os detalhes completos do cliente selecionado.
3. **Enviar para análise** e **Bloquear cliente** têm confirmação, motivo obrigatório e feedback de sucesso ou erro.
4. O estado do caso muda na interface após uma ação confirmada.
5. O visual usa o verde do PicPay como referência e mantém os riscos legíveis e distinguíveis.
6. Todos os nomes, IDs e transações são fictícios; o protótipo não expõe dados reais.
