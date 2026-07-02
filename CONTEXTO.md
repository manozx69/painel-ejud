# CONTEXTO — Sistema de Templates EJUD

> Leia este arquivo no início de cada sessão para entender o projeto.
> Para o histórico de mudanças, veja **ATUALIZACOES.md**.

## O que é

Ferramenta web **single-file** (`index.html`) que ajuda a **montar e padronizar
templates HTML** que serão colados no **AVA EJUD Moodle** (Ambiente Virtual de
Aprendizagem da Escola Judicial). O usuário preenche um formulário, visualiza o
preview ao vivo e copia o HTML pronto para colar dentro de um recurso/rótulo do
Moodle.

- **Órgão:** EJUD — Escola Judicial (TJAM).
- **Objetivo:** garantir identidade visual e estrutura consistentes em todos os
  cursos, sem que cada pessoa precise escrever HTML na mão.
- **Saída:** blocos de HTML autocontidos (com `<style>` embutido e escopo por
  `id` único) prontos para o editor do Moodle.

## Como funciona (arquitetura)

- **Um único arquivo:** todo o sistema vive em `index.html` — HTML, CSS e JS
  (vanilla, sem build, sem dependências além de Google Fonts). Basta abrir no
  navegador.
- **Container raiz:** `#ejud-gerador`. Todo o CSS é escopado a esse id.
- **Abas (tabs):** navegação via `egTab(id)`. Cada aba = um tipo de template.
- **Geração:** cada template tem uma função `genXxx(forCopy)` que monta o HTML.
  - `forCopy = false` → gera o preview ao vivo.
  - `forCopy = true`  → gera o HTML final com `id` único (`Date.now()`) para copiar.
- **`wrap(id, html, css)`:** envolve o HTML gerado em `<div id="...">` + `<style>`
  escopado, garantindo que o CSS não vaze no Moodle.
- **Preview reativo:** `G.addEventListener("input"/"change", ...)` re-renderiza o
  preview a cada alteração no formulário.

## Templates disponíveis (abas)

| Aba (`data-tab`) | Rótulo na UI        | Uso |
|------------------|---------------------|-----|
| `banner`         | Banner do Curso     | Cabeçalho/hero do curso com modalidade, data/período, botões de ação |
| `aviso`          | Aviso               | Comunicado/alerta (ex.: inscrições abertas) |
| `meet`           | Google Meet         | Card de aula/reunião online; faz parse do convite do Meet |
| `video`          | Vídeo / Aula        | Player de YouTube embutido + instrutores + materiais |
| `info`           | Informações         | Bloco de informações gerais |
| `encerramento`   | Dados Obrigatórios  | Dados obrigatórios / encerramento |
| `certificado`    | Certificado         | Card de certificado |
| `avaliacao`      | Avaliação           | Card de avaliação |
| `restrito`       | Curso Restrito      | Aviso de curso de acesso restrito |

## Convenções

- **Idioma:** português (pt-BR). Interface e conteúdo padrão em português.
- **Identidade visual:** fontes `Cormorant Garamond` (títulos) e `Archivo`
  (texto); verde institucional `#0d8f3e` / `#0a2e1b`; cinza `#3a4555`.
- **Ícones:** SVG inline gerados por `svg(name, size, color, sw)`; há um seletor
  de ícones curados (`CURATED`).
- **Escopo de CSS:** SEMPRE escopar por `id` para não afetar o resto do Moodle.
- **Sem dependências de build:** editar = abrir `index.html` e alterar direto.

## Fluxo de trabalho ao mexer no projeto

1. Ler `CONTEXTO.md` (este arquivo) e `ATUALIZACOES.md`.
2. Fazer a alteração em `index.html`.
3. **Registrar a mudança em `ATUALIZACOES.md`** (data, o que mudou, por quê).
4. Commitar.
