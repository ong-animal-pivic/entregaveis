# Pasta "entregaveis" — Documentação e relatórios do projeto PIVIC

## O que é esta pasta

Esta pasta reúne os **materiais documentais e de apresentação** produzidos ao longo do projeto de iniciação científica (PIVIC) "Sistema ONG Animal", destinados à orientadora e a possíveis avaliações/relatórios do programa. Ela é **separada dos repositórios de código** propositalmente: aqui não entra código-fonte, apenas documentos (relatórios em HTML, PDFs, apresentações, atas, etc.) que registram o progresso, as decisões técnicas e o raciocínio por trás do que foi implementado.

## Contexto do projeto (visão geral)

O **Sistema ONG Animal** é um sistema de gestão para ONGs de proteção animal, composto por:

- **Backend** (`G:\My Drive\01. Projetos\pivic\sistema-ong-animal`): API REST em **Java com Spring Boot**, usando **Spring Data JPA** (Hibernate) para persistência em **PostgreSQL**, migrações de banco via **Flyway**, e **Bean Validation** (`jakarta.validation`) para validação de dados. Usa Lombok para reduzir boilerplate.
- **Frontend** (`G:\My Drive\01. Projetos\pivic\sistema-ong-animal-frontend`): aplicação em **Angular**.

### Domínio do sistema
As entidades centrais do backend são:
- **Adotante**: pessoa cadastrada como potencial adotante de animais (dados pessoais, documento/CPF, contato, endereço, dados demográficos).
- **Animal**: animal resgatado pela ONG (nome, idade, porte, sexo, status de adoção, se é castrado, datas de resgate/saída), relacionado a uma `Raca` e, opcionalmente, a um `Adotante` (quando adotado).
- **Raca**: raça do animal, relacionada a uma `Especie`.
- **Especie**: espécie do animal (ex: cão, gato).

### Arquitetura (backend)
O backend segue camadas: `Controller` (API REST) → `Service` (regras de negócio) → `Repository` (JPA) → `Entity` (mapeamento com o banco). Decisões arquiteturais específicas e seu histórico de evolução ficam registradas nos relatórios desta pasta, não neste arquivo — consulte os relatórios individuais (ou o código-fonte) para o estado mais recente.

## O que deve entrar nesta pasta

- Relatórios de progresso/entrega em **HTML** (autocontidos, sem dependências externas, para poderem ser abertos offline e enviados por e-mail/Drive).
- Documentos de decisões arquiteturais relevantes (o "porquê" por trás de mudanças estruturais no código).
- Materiais de apresentação para reuniões de orientação.
- Outros documentos administrativos do PIVIC associados a este projeto (atas, cronogramas, etc.), quando fizer sentido centralizá-los aqui.

## O que NÃO deve entrar nesta pasta

- Código-fonte ou configuração do sistema (isso vive nos repositórios `sistema-ong-animal` e `sistema-ong-animal-frontend`).
- Segredos, credenciais ou dados de produção.

## Organização da pasta

Os documentos ficam organizados em subpastas por tipo. Todo documento novo deve ser salvo na subpasta correspondente ao seu tipo, nunca solto na raiz:

- **`relatorios-progresso/`** — todo relatório em HTML sobre o andamento do projeto: tanto os relatórios periódicos ("o que mudou recentemente") quanto os relatórios sobre uma decisão/mudança técnica específica (ex: uma migração arquitetural, uma análise do estado do backend). Não existe subcategoria separada para "decisões arquiteturais" — tudo isso é relatório de progresso, só com escopo diferente (um período de tempo vs. um tópico específico).
- **`diagramas/`** — documentos cujo conteúdo principal é um diagrama (sequência, fluxo, arquitetura, etc.), seguindo o padrão descrito em "Padrão obrigatório para diagramas" abaixo.
- **`documentacao-geral/`** — documentação de referência sobre o projeto que não é um relatório de progresso nem um diagrama (ex: stack tecnológico, guia de execução do projeto). Geralmente são "documentos vivos", atualizados conforme o projeto evolui.
- **`material-de-estudo/`** — documentos explicativos sobre conceitos de programação, arquitetura ou bibliotecas aprendidos durante o desenvolvimento, sem relação com uma alteração específica do sistema. Ver "Padrão para material de estudo" abaixo.

O `CLAUDE.md` continua na raiz da pasta `entregaveis`, pois documenta a pasta como um todo.

## Padrão obrigatório para relatórios (HTML)

Todo relatório em HTML criado nesta pasta deve seguir os pontos abaixo. O arquivo `relatorios-progresso/be-migracao-dto-2026-08-16.html` é o **modelo de referência** — em caso de dúvida sobre como implementar algum item, replicar a solução usada nele.

### Conteúdo e linguagem
- **Público-alvo é não técnico** (orientadora, avaliadores do PIVIC): explicar conceitos com analogias antes de nomear o termo técnico, nunca assumir conhecimento prévio de programação.
- **Pode citar nomenclatura técnica** (ex: DTO, API, entidade, Controller), mas sempre com explicação curta ao lado ou em um glossário — nunca aprofundar detalhes de implementação/código.
- **Nunca incluir trechos de código-fonte** (blocos `<pre><code>`, snippets, nomes de arquivos/classes específicos) — o relatório fala de conceitos e decisões, não de sintaxe.
- **Ficar no escopo da alteração**: só explicar um conceito quando ele for necessário para entender a alteração em si (ex: o que é uma máscara de campo, o que é CRUD, o que é uma migração de banco). Não explicar conceitos genéricos de arquitetura/programação que não são o foco do relatório (ex: o que é "frontend" e "backend" em geral) — usar esses termos como rótulos diretos (ex: tag "Backend"/"Frontend"), sem analogia introdutória, a menos que a alteração em questão seja especificamente sobre a diferença entre as duas camadas.
- Incluir **elementos visuais** (diagramas de fluxo, comparações antes/depois, linhas do tempo) sempre que ajudarem a explicar um mecanismo — usar SVG desenhado à mão (sem bibliotecas externas), com setas rotuladas.

### Data de criação
Todo relatório **precisa exibir a data em que foi criado**, de forma visível (ex: na linha de cabeçalho/eyebrow do documento), no formato `DD/MM/AAAA`. Usar a data real do dia da criação. Essa data **não muda** em edições posteriores do mesmo relatório — marca quando o conteúdo original foi produzido.

### Autocontido e com suporte a tema claro/escuro
- HTML, CSS e JS **inline no mesmo arquivo** — sem dependências externas (CDN, fontes remotas, imagens externas), para abrir offline.
- Paleta de cores definida via variáveis CSS (`:root`), com bloco `@media (prefers-color-scheme: dark)` e seletor `[data-theme="dark"]`/`[data-theme="light"]` para respeitar o tema do sistema/navegador do leitor.

### Edição de texto direto na interface
Incluir uma barra flutuante fixa (`#editToolbar`, canto inferior direito) com:
- Botão **"Editar texto"**: alterna `contenteditable` no container principal do conteúdo.
- Botão **"Salvar no arquivo"**: usa a File System Access API (`showSaveFilePicker` + `createWritable`) para sobrescrever o próprio arquivo HTML com as edições feitas na tela. Se a API não estiver disponível no navegador, cair no fallback de baixar uma cópia editada via `Blob`/link de download.
- A barra deve ficar oculta na impressão (`@media print { #editToolbar { display: none; } }`).

### Botão de exportar como PDF
Incluir também na barra um botão **"Salvar como PDF"**, que chama `window.print()` (sem bibliotecas externas). O CSS de impressão (`@media print`) deve:
- Forçar **fundo branco e texto escuro**, independente do tema (claro/escuro) usado na tela.
- Definir `@page { size: A4; margin: ...; }`.
- Manter cores de destaque (cards, comparações, linha do tempo) legíveis no papel, com `print-color-adjust: exact`.
- Evitar cortar seções/figuras no meio entre páginas (`break-inside: avoid`/`avoid-page`).

### Convenção de nomes
Usar `kebab-case` descritivo, dentro da subpasta correspondente ao tipo de documento (ver "Organização da pasta"), e **sempre incluir a data de criação do arquivo no nome**, no formato `AAAA-MM-DD` ao final (antes da extensão). Essa data é a de **criação** do arquivo e não muda em edições/atualizações posteriores do mesmo documento — mesmo para "documentos vivos" que são reabertos e atualizados ao longo do projeto (o conteúdo interno pode ter sua própria data de "última atualização", mas o nome do arquivo preserva a data original de criação).

Em `diagramas/` e `documentacao-geral/`, o nome é só `<assunto>-AAAA-MM-DD.html`, ex: `stack-tecnologico-2026-08-16.html`. Em `relatorios-progresso/`, segue o padrão específico abaixo.

### Relatórios de progresso (`relatorios-progresso/`)
Todo relatório dessa pasta — periódico ou sobre um tópico específico — segue o formato:

`<sigla>-<indicativo-da-mudança>-AAAA-MM-DD.html`

- **`<sigla>`**: `fe`, `be`, ou `fe-be` — indica só **onde** houve alteração (frontend, backend, ou ambos). Não é uma categoria do documento, é só informativo para quem está olhando a lista de arquivos.
- **`<indicativo-da-mudança>`**: resumo curto em kebab-case do que foi tratado no relatório, para que o nome do arquivo já diga do que se trata sem precisar abrir o documento.
  - Relatório sobre um tópico específico (ex: uma migração, uma análise arquitetural): usar o nome desse tópico, ex: `migracao-dto`, `exclusao-logica`, `analise-arquitetural`.
  - Relatório periódico amplo, cobrindo várias frentes num período (ex: resumo semanal): destacar as 1-2 mudanças mais relevantes do período, mesmo que o conteúdo cubra mais coisas, ex: `idade-em-meses-e-busca`.
- **`AAAA-MM-DD`**: data de criação do relatório, sempre no final.

Exemplos: `be-migracao-dto-2026-08-16.html`, `be-analise-arquitetural-2026-08-22.html`, `fe-be-idade-em-meses-e-busca-2026-08-30.html`.

O conteúdo interno do relatório pode e deve detalhar o período e todas as frentes cobertas (ex: "29 e 30/08") no título e no texto — a simplificação para 1-2 destaques vale só para o nome do arquivo.

## Padrão obrigatório para diagramas (HTML com SVG)

Documentos cujo conteúdo principal é um **diagrama** (sequência, fluxo, arquitetura, etc.) seguem as mesmas regras gerais de "Padrão obrigatório para relatórios" acima (público não técnico, autocontido, tema claro/escuro, barra de edição, exportar PDF, convenção de nomes), **mais** as regras específicas abaixo. O arquivo `diagramas/diagrama-sequencia-backend-2026-08-16.html` é o **modelo de referência**.

### Estrutura geral do documento
- Cabeçalho (`header.hero`) com eyebrow (`PIVIC · Sistema ONG Animal · Documento vivo`), `h1` com o título e um badge "Última atualização: DD/MM/AAAA".
- Um parágrafo `.hint` logo abaixo explicando, em uma frase, como ler o diagrama (o que são as colunas, o sentido do tempo, o que fazer com o mouse).
- Uma **legenda de atores** (`.actor-key`) fixa no topo, com um ícone SVG + nome para cada "raia"/coluna do diagrama (ex: Tela, Controller, Service, Repository/Banco, Tratador de erros), reaproveitada em todos os diagramas do documento.
- Cada diagrama fica dentro de um `.diagram-block`, com um `h2` prefixado por uma tag `ok` (verde, caminho de sucesso) ou `err` (vermelha, caminho de erro) e um `figure` contendo o SVG.

### Desenho do SVG (raias / lifelines)
- Uma coluna por camada/ator, cada uma com: um círculo com ícone no topo (`.lane-icon`), o nome centralizado abaixo (`.lane-name`) e uma linha vertical fina (`opacity: 0.35`) descendo por toda a altura do diagrama — essa é a "lifeline".
- Ícones são definidos **uma única vez** como `<symbol>` dentro de um `<svg>` oculto (`width="0" height="0" style="position:absolute"`) no topo do `<body>`, e reutilizados via `<use href="#icon-x">` — nunca duplicar o path do ícone em cada raia.
- O tempo corre de cima para baixo. Cada interação entre raias é uma **seta horizontal numerada** (`1.`, `2.`, `3.`...) com um texto curto acima descrevendo a ação em linguagem simples ("Controller solicita ao Service que aplique as regras").
- Setas de caminho de sucesso: linha sólida, cor `currentColor`, `marker-end` com uma seta preenchida (`<marker>` definido uma vez por SVG).
- Setas/curvas de desvio para erro: `stroke="var(--danger)"`, `stroke-dasharray="4 3"` (tracejada), com o texto explicando a condição (ex: "se essa raça não existe → erro") e referenciando o outro diagrama que detalha esse caminho de erro.
- Passos internos de uma camada que **não envolvem chamar outra camada** (validação, conversão DTO↔entidade, checagem de regra) são desenhados como uma caixa pontilhada (`.note-box`, `stroke-dasharray` implícito por `opacity: 0.7`) com 1-2 linhas de texto dentro — não uma seta.

### Interatividade (tooltip on hover)
- Cada seta/caixa (`.step`/`.note`) tem uma **camada de hit invisível** mais grossa (`stroke-width="14"` ou `fill="transparent"`) só para facilitar o hover/toque, sobreposta a uma **camada de interação separada no final do SVG** (`.hitlayer`, com `data-tip="tipN"` apontando para o balão correspondente) — isso garante que o hover funcione mesmo quando outro elemento visual estaria por cima na mesma região.
- Ao passar o mouse (ou focar via teclado, `tabindex="0"`) sobre um passo, um balão (`g.tip`, escondido por padrão via `opacity: 0`) aparece com: um rótulo curto em uppercase (`.tip-label`, ex: "Por que essa checagem existe") e 1-3 linhas de detalhe (`.tip-detail`) explicando o "porquê", não só repetindo o rótulo da seta.
- Ao mesmo tempo, o elemento de origem (seta ou caixa) ganha destaque visual (`.hl`, borda mais grossa na cor de acento ou de erro) via JS, controlado pelo par `id="srcN"` / `id="tipN"`.
- Balões de desvio para erro usam a variante `.tip-danger` (cores de destaque em vermelho).
- Cada diagrama tem um botão "⛶ Tela cheia" (`data-fs`) no canto superior direito da `figure`, que usa a Fullscreen API do navegador — sem bibliotecas externas.

### Numeração e nomenclatura das interações
- Interações que cruzam raias (chamadas reais entre camadas) são numeradas sequencialmente dentro de cada diagrama (1, 2, 3...) e descritas com o verbo de quem inicia a ação ("Controller solicita...", "Service devolve...").
- Passos internos (notas/caixas) **não** entram na numeração — são detalhes de apoio de uma raia específica.
- Uma legenda final (`figcaption`) resume a convenção visual usada no diagrama (ex: "Setas sólidas: caminho feliz. Setas tracejadas: pontos onde o fluxo pode desviar para o caminho de erro.").

### Impressão específica de diagramas
Além das regras gerais de impressão do relatório-modelo, diagramas devem:
- Usar `@page { size: A4 landscape; ... }` (paisagem, não retrato) — diagramas são largos.
- Cada `.diagram-block` ocupa uma página inteira (`page-break-after: always`), com o SVG centralizado verticalmente.
- Esconder na impressão: barra de edição, botão de tela cheia, o `.hint` introdutório, o cabeçalho (`header.hero`), a legenda de atores (`.actor-key`) e a camada de tooltip/hit (`.tip`, `.hitlayer`) — só o `h2` (com a tag ok/err) e o SVG do diagrama aparecem.

## Padrão para material de estudo

Documentos que ficam em `material-de-estudo/`: conteúdo explicativo sobre um conceito de programação, arquitetura ou biblioteca aprendido durante o desenvolvimento — não é sobre uma alteração específica do sistema, é material de aprendizado para consulta futura. Cada documento cobre **um conceito por arquivo**.

- Seguem as mesmas regras gerais de "Padrão obrigatório para relatórios" acima: público não técnico, autocontido, tema claro/escuro, barra de edição, exportar PDF, elementos visuais quando ajudam a explicar (incluindo diagramas SVG no padrão de "Padrão obrigatório para diagramas" quando o mecanismo do conceito se beneficiar de um).
- **Diferença de escopo em relação aos relatórios de mudança**: como não estão amarrados a uma alteração específica do sistema, podem aprofundar mais o conceito em si (contexto, motivação, alternativas, quando usar) em vez de ficar restritos ao mínimo necessário para entender uma mudança pontual.
- **Convenção de nome**: prefixo `conceito-` + assunto em kebab-case + data de criação, ex: `conceito-injecao-de-dependencia-2026-09-05.html`.

### Uso de código-fonte (exceção à regra dos relatórios)
Diferente dos relatórios, o material de estudo **pode incluir pequenos trechos de código** para ilustrar a mecânica do conceito, desde que:
- Sejam **genéricos e didáticos** — pseudo-código ou código bem simplificado, com nomes de classes/variáveis de exemplo (ex: `ServicoDeEmail`, `ControladorDePedido`), nunca o nome real de uma classe, arquivo, tabela ou variável do projeto.
- Sirvam só para mostrar a sintaxe/estrutura do conceito em si, não para documentar como o projeto especificamente implementou algo.
- Continuem sempre acompanhados de explicação em linguagem simples ao lado — o trecho de código complementa a explicação, não a substitui.

Essa exceção vale **somente** dentro de `material-de-estudo/`; a proibição de código-fonte nos relatórios (seção "Conteúdo e linguagem" acima) continua valendo normalmente.

### Estrutura de conteúdo obrigatória
Todo documento de material de estudo segue esta sequência de seções, para manter consistência entre os documentos:

1. **O que é** — analogia do dia a dia seguida da definição do conceito (mesma regra geral de introduzir com analogia antes do termo técnico).
2. **Que problema resolve** — como seria a situação sem esse conceito (o "antes"), para deixar clara a motivação de ele existir.
3. **Como funciona** — o mecanismo por trás do conceito; usar diagrama SVG (padrão de "Padrão obrigatório para diagramas") quando ajudar a visualizar o fluxo, e snippet de código genérico (ver acima) quando ajudar a ilustrar a sintaxe.
4. **Onde isso apareceu no projeto** — em que parte do desenvolvimento (backend/frontend, que tipo de mudança) esse conceito foi usado na prática, sem citar código real — só o contexto.
5. **Armadilhas comuns / boas práticas** — erros frequentes ao aplicar o conceito e recomendações de uso.
6. **Termos relacionados** — mini-glossário com outros termos técnicos citados no texto que mereçam uma definição curta à parte.

### Nota de contexto de origem
Logo abaixo do cabeçalho/eyebrow do documento, incluir uma nota curta (1 frase) indicando qual mudança ou período do projeto motivou o estudo daquele conceito, referenciando o relatório correspondente pelo nome do arquivo (sem precisar ser um link clicável, já que os documentos são autocontidos e podem circular avulsos). Exemplo: "Este conceito foi estudado a partir da migração para DTOs — ver relatório `be-migracao-dto-2026-08-16`."
