# site-pessoal

Site pessoal em Hugo, com tema próprio (sem tema de terceiros).

## Requisitos

Hugo 0.158 ou mais novo (edição padrão basta). Testado com 0.169 e 0.170.

## Uso

Servidor local com recarga automática, incluindo rascunhos:

```
hugo server -D
```

Build de produção (gera `public/`, rascunhos ficam de fora):

```
hugo --gc --minify
```

## Línguas

Inglês na raiz do site e português em `/pt/`. Para traduzir uma página, criar o arquivo com `.pt` antes da extensão ao lado do original (`content/cv.md` e `content/cv.pt.md`; `content/projects/nome/index.md` e `content/projects/nome/index.pt.md`). Posts sem tradução aparecem nas duas versões, marcados com a língua em que foram escritos. Textos fixos da interface ficam em `i18n/`.

## Estrutura

```
hugo.toml                  configuração, menu, e-mail, GitHub
content/_index.md          texto da página inicial (inglês)
content/_index.pt.md       texto da página inicial (português)
content/cv.md, cv.pt.md    currículo nas duas línguas
content/projects/          um arquivo .md por projeto
content/notes/             um arquivo .md por nota
data/publications.yaml     lista de publicações (home e CV)
data/talks.yaml            palestras, minicursos e workshops (home e CV)
assets/css/main.css        estilo completo do site
assets/img/                imagens processadas pelo Hugo (foto de perfil)
i18n/                      textos fixos da interface por língua
layouts/                   templates
layouts/_partials/         cabeçalho, rodapé, publicação, lista, traço espectral
static/fonts/              Spectral e Public Sans (woff2, servidas localmente)
static/files/              PDFs (currículo, artigos)
```

## Tarefas comuns

Novo projeto ou nota: criar `content/projects/nome.md` ou `content/notes/nome.md` com o cabeçalho abaixo. Enquanto `draft: true`, o post só aparece com `hugo server -D`.

```
---
title: "Título"
date: 2026-11-14
draft: true
description: "Uma frase que aparece na listagem."
tags: ["rf", "sdr"]
---
```

Para posts com imagens, usar um diretório (`content/projects/nome/index.md`) e colocar as imagens ao lado do `index.md`. Uma imagem chamada `cover.jpg` (ou `.png`) nesse diretório vira a miniatura na listagem e a capa do post. Alternativa sem diretório: `cover: "img/covers/arquivo.png"` no cabeçalho, com o arquivo em `assets/img/covers/`.

Nova palestra: adicionar uma entrada no topo de `data/talks.yaml`.

Nova publicação: adicionar uma entrada no topo de `data/publications.yaml`. O BibTeX é gerado a partir dos campos. Para disponibilizar o PDF, copiar para `static/files/` e preencher `pdf: "/files/arquivo.pdf"`.

Currículo em PDF: copiar para `static/files/cv.pdf`. O link aparece na página do CV automaticamente quando o arquivo existe.

Traço espectral do cabeçalho: SVG estático em `layouts/_partials/spectrum.html` (lóbulo sinc² de BPSK sobre piso de ruído).

## Deploy

Publicado em https://allao255.github.io/ pelo workflow `.github/workflows/hugo.yml`, a cada push na branch `main` (GitHub Pages com fonte "GitHub Actions"). Para domínio próprio, ajustar `baseURL` em `hugo.toml` e criar `static/CNAME` com o domínio.
