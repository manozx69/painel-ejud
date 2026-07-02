# ATUALIZAÇÕES — Sistema de Templates EJUD

> Registro de TODAS as mudanças do projeto. Adicione uma entrada no topo a cada
> alteração, por menor que seja. Formato: data (AAAA-MM-DD), o que mudou e por quê.
> Contexto geral do projeto: veja **CONTEXTO.md**.

---

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
