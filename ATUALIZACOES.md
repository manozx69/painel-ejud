# ATUALIZAÇÕES — Sistema de Templates EJUD

> Registro de TODAS as mudanças do projeto. Adicione uma entrada no topo a cada
> alteração, por menor que seja. Formato: data (AAAA-MM-DD), o que mudou e por quê.
> Contexto geral do projeto: veja **CONTEXTO.md**.

---

## 2026-07-07 — Modelos salvos globalmente (Vídeo/Aula e Curso Restrito)

- **Banco de dados em nuvem via Google Planilhas.** Agora dá para **salvar,
  carregar, editar e excluir modelos** nas abas **Vídeo / Aula** e **Curso
  Restrito**, com armazenamento **global** (todos que abrem o gerador veem os
  mesmos modelos). Antes o Restrito salvava só em `localStorage` (por navegador);
  migrado para a nuvem. A aba Vídeo ganhou a barra de modelos do zero.
- **Ponte:** um **Google Apps Script** publicado como *app da Web* recebe/responde
  JSON e usa uma planilha como banco (colunas `id | aba | nome | data | dados`).
  O `dados` guarda o JSON do formulário (snapshot). CRUD por `action`
  (`create`/`update`/`delete`/`list`).
- **Camada no front (`index.html`):** helpers `egCloudList`/`egCloudPost`/
  `egRefreshModelos`, cache `egModelos` e a constante **`EG_API`** (onde se cola a
  URL do `/exec`). Snapshots: `readRstSnapshot`/`applyRstSnapshot` e
  `readVdSnapshot`/`applyVdSnapshot` (inclui inputs `vd-*` + arrays `vdInst`,
  `vdMat`, `vdCaps`; e no Restrito passou a salvar também o **Nome do Curso**).
  Funções `egVdLoadTpl`/`egVdSaveTpl`/`egVdDelTpl` e as `egRst*Tpl` reescritas
  para a nuvem. Presets do Restrito (`RST_PRESETS`) continuam locais e read-only.
- **Editar:** salvar com um nome já existente **substitui** (update) o modelo.
- **Segurança:** endpoint aberto (sem senha) — qualquer um com o gerador pode
  gravar/excluir; leitura/uso é livre. Se `EG_API` estiver vazia, o recurso fica
  inativo sem quebrar o resto (presets seguem funcionando).
- **Setup (manual, uma vez):** planilha em branco → Extensões → Apps Script →
  colar o script → Implantar como app da Web ("Qualquer pessoa") → copiar a URL
  `/exec` → colar em `EG_API`.
- **Ativado em 2026-07-07:** `EG_API` já preenchida com a URL do app da Web
  (script hospedado em conta TJAM, planilha "EJUD - Modelos"). CRUD testado
  (create/list/delete) respondendo JSON publicamente. Atenção: a publicação
  precisou ser "Qualquer pessoa" (a opção restrita ao domínio exige login e
  quebrava o fetch do GitHub Pages).

## 2026-07-02 — Melhorias na aba Vídeo / Aula

- **Reordenação de instrutores e materiais:** adicionadas setas ↑/↓ (helper
  `rowActions`), em paridade com as abas Informações e Curso Restrito. Funções
  `egInstMove`/`egMatMove`.
- **Badge de duração:** novo campo "Duração" que gera um selo no card
  (ex.: "Duração: 45 min").
- **Capítulos / timestamps:** nova seção repetível (tempo + título) exibida
  abaixo do player. Em vídeos do YouTube, o tempo vira link direto para o trecho
  (`youtu.be/ID?t=segundos`); aceita `MM:SS` e `HH:MM:SS`. Funções `renderCaps`,
  `egAddCap`/`egRmCap`/`egCapMove`.
- **Mais fontes de vídeo:** `getEmbed` agora reconhece YouTube **Shorts** e
  **Live**, e preserva o **tempo de início** (`?t=` em segundos ou `1m30s`),
  gerando `?start=`. Helpers `ytId`/`ytStart`/`capSeconds`.
- **Ordem dos campos:** Módulo/Aula subiram para logo abaixo do Link (espelham o
  cabeçalho do card); Data virou meia-coluna para a grade fechar sem órfão.

## 2026-07-02 — Melhorias na aba Aviso

- **Botão de ação (CTA) opcional:** novos campos "Texto do Botão" e "Link do
  Botão"; quando ambos preenchidos, gera um botão no aviso (ex.: "Inscreva-se").
- **3º tipo "Atenção" (amarelo):** o Aviso agora tem 3 níveis de cor
  (Informativo/verde, Atenção/amarelo, Urgente/vermelho). `genAviso` reescrito com
  mapa de estilos por tipo (ícone, rótulo e cores).
- **Quebras de linha na Mensagem:** `av-msg` agora converte `\n` em `<br>`
  (antes os parágrafos eram colapsados num bloco só).
- **Presets rápidos:** botões "Inscrições abertas", "Prazo final" e "Aula
  cancelada" que preenchem o formulário (`egAvisoPreset`).
- Ordem dos campos mantida (já estava coerente com o layout gerado); CTA acrescido
  ao fim do grid.

## 2026-07-02 — Reordenação de campos da aba Banner do Curso

- Reagrupados os campos do formulário do Banner para seguir o fluxo visual do
  banner gerado: (1) Identidade — Nome, Subtítulo, Modalidade, Carga Horária,
  **Tema/Sistema e Ícone** (que estavam soltos no meio, agora junto do hero);
  (2) Datas/horário — Temporalidade, Período/Data, Detalhe, Horário;
  (3) Local e Certificação; (4) Conteúdo — Objetivo e Público.
- "Horário (opcional)" deixou de ser campo largo (wide) e passou a meia-coluna,
  pareando com o "Detalhe" e eliminando a célula órfã na grade.
- Rótulo "Detalhe (opcional)" renomeado para "Período/Data — Detalhe (opcional)",
  ficando consistente com "Local — Detalhe" e "Certificador — Detalhe"; placeholder
  ajustado para refletir o uso real (ex.: "Horário Livre, I Ciclo").
- Sem mudança de comportamento/IDs — apenas ordem, largura e rótulo.

## 2026-07-02 — Correção de bugs de média prioridade

- **Escape de entrada do usuário (anti-corrupção/injeção de HTML).** Os helpers
  `v()` de todos os geradores agora retornam o valor já escapado via `esc()`, e o
  `esc()` foi aplicado aos valores vindos de arrays (botões do banner, instrutores
  e materiais do vídeo, blocos de Informações e de Curso Restrito). Assim,
  caracteres como `< > & "` em títulos, mensagens e links deixam de quebrar o HTML
  gerado. Adicionado helper `rawVal()` e usado no nome do curso do Restrito, que
  precisa ficar cru por alimentar `encodeURIComponent` do e-mail.
- **Parser do Google Meet aceita link sem protocolo.** Antes só capturava o link
  se viesse com `https://`; agora reconhece `meet.google.com/xxx-xxxx-xxx` e
  normaliza para `https://`.

## 2026-07-02 — Correção de bugs de alta prioridade

- **Cópia para o Moodle com fallback e tratamento de erro** (`egCopy`). Antes,
  `navigator.clipboard.writeText` não tinha `.catch()` nem alternativa: se a API
  falhasse (contexto não seguro, aba sem foco, navegador antigo) o botão não dava
  feedback e nada era copiado. Agora há fallback via `<textarea>` +
  `document.execCommand("copy")` e feedback de falha ("copie manualmente").
- **Tooltip do instrutor (aba Vídeo) reescrito em CSS puro.** Antes dependia de um
  `<script>` embutido no HTML copiado — que o editor do Moodle normalmente remove
  ao salvar, quebrando o tooltip. Agora o balão é mostrado via `:hover`/`:focus`
  (sem JS), funcionando mesmo com scripts removidos. Removidos o `<script>`
  injetado e os listeners de preview.
- **Adicionado helper `esc()`** (escapa `& < > "`) e aplicado ao conteúdo do balão
  do tooltip.
- Pendente: o `<script>` de auto-ocultação da aba **Curso Restrito** também pode
  ser removido pelo Moodle (aguardando confirmação de comportamento do AVA).

## 2026-07-02

- **Criada a documentação de contexto do projeto.** Adicionados `CONTEXTO.md`
  (explica o que é o sistema e sua arquitetura) e `ATUALIZACOES.md` (este
  changelog). Objetivo: dar contexto rápido no início de cada sessão e manter
  histórico das mudanças.
- Configurada a leitura automática de `CONTEXTO.md` e `ATUALIZACOES.md` ao abrir
  o terminal (hook de SessionStart), para retomar o trabalho já com o contexto.

<!--
Modelo de entrada para copiar:

## AAAA-MM-DD

- **Título curto da mudança.** Descrição do que foi alterado em `index.html`
  (aba/função afetada) e o motivo.
-->
