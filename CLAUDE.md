# CLAUDE.md

Contexto para o Claude Code neste repositório.

## Projeto

Site pessoal de Daniel Allão (estudante de Engenharia Elétrica, EESC-USP; pesquisa em segurança de GNSS, spoofing e jamming, RF fingerprinting). Público: orientadores, bancas, recrutadores da área de defesa e aeroespacial, inclusive no exterior. Função do site: apresentar quem ele é, publicações, currículo e write-ups técnicos de projetos.

Gerador: Hugo (versão mínima 0.158; edição padrão basta, não depende da extended; CI usa 0.170.0). Tema próprio em `layouts/`, sem tema de terceiros e sem módulos Hugo.

## Línguas

Site bilíngue: inglês na raiz (`/`), português em `/pt/`. Configuração em `[languages]` no `hugo.toml`, incluindo os menus de cada língua.

- Tradução por nome de arquivo: `cv.md` (inglês) e `cv.pt.md` (português); `_index.md` e `_index.pt.md`. Os dois arquivos ficam lado a lado e o Hugo liga um ao outro.
- Um post pode existir em uma língua só. `layouts/_partials/section-pages.html` inclui nas listagens de cada língua os posts da outra língua que não têm tradução, marcados com "em inglês" / "in Portuguese".
- Textos fixos da interface ficam em `i18n/en.toml` e `i18n/pt.toml`. Nada de texto fixo de interface direto nos templates; usar `{{ i18n "chave" }}`.
- `data/talks.yaml`: cada entrada aceita um bloco `pt:` com campos em português que substituem os em inglês em `/pt/`. Publicações ficam só em inglês (títulos dos artigos).
- Links internos nos templates com `relLangURL`, para respeitar o prefixo da língua.
- O seletor de língua no menu aponta para a tradução da página quando existe, e para a página inicial da outra língua quando não existe.

## Comandos

```
hugo server -D            # servidor local em http://localhost:1313, inclui rascunhos
hugo --gc --minify        # build de produção em public/
```

Critério de pronto para qualquer mudança: `hugo --gc --minify` termina sem ERROR e sem WARN, e a página afetada foi conferida em tela larga e estreita (até ~390 px), nos temas claro e escuro.

## Estrutura

- `hugo.toml`: configuração, menu principal, e-mail, GitHub, caminho do PDF do CV.
- `content/_index.md`: texto da página inicial (campo `lede` = subtítulo).
- `content/cv.md`: currículo em Markdown, com `class: "cv"` (estilo próprio: cada `###` é cargo/curso e o parágrafo logo abaixo é instituição e período). Usa os shortcodes `{{< publications >}}` e `{{< talks >}}`. Campo `pdf` aponta para `static/files/cv.pdf`; o link só aparece se o arquivo existir.
- `content/projects/`, `content/notes/`: um `.md` por item, ou um diretório com `index.md` e imagens (page bundle) quando houver figuras.
- Miniatura e capa: `cover.*` dentro do page bundle, ou `cover: "img/covers/x.png"` no front matter (caminho em `assets/`). `coverAlt` é o texto alternativo. Aparece como miniatura 3:2 nas listas e como capa no topo do post.
- `data/publications.yaml`: publicações, mais recente primeiro. O BibTeX é gerado pelo partial a partir dos campos. Exibidas na página inicial e no CV; não há página separada de publicações.
- `data/talks.yaml`: palestras, minicursos e workshops. Exibidos na página inicial e no CV.
- `layouts/`: sistema de templates novo do Hugo (>= 0.146). `baseof.html`, `home.html`, `section.html`, `page.html`, `taxonomy.html`, `term.html`, `404.html`; partials em `layouts/_partials/` (`cover.html` resolve a imagem de capa); shortcodes em `layouts/_shortcodes/`.
- `layouts/_partials/spectrum.html`: SVG estático do traço espectral sob o cabeçalho (sinc² de BPSK sobre piso de ruído). Gerado por script; não editar o path à mão.
- `assets/css/main.css`: todo o CSS. Processado com minify e fingerprint.
- `assets/img/`: imagens processadas pelo Hugo (redimensionamento). Não converter para webp nem usar SCSS, para não exigir a edição extended.
- `static/fonts/`: Spectral e Public Sans em woff2, servidas localmente.
- `static/files/`: PDFs.
- `.github/workflows/hugo.yml`: workflow do GitHub Pages. Publica a cada push na `main`.

## Publicação

Repositório `Allao255/allao255.github.io`, site em https://allao255.github.io/ (GitHub Pages, fonte "GitHub Actions"). Push na `main` dispara o build e o deploy.

## Convenções

- Conteúdo do site em inglês e português (ver "Línguas"). Comentários de código, README e este arquivo em português.
- Texto sóbrio, direto, sem travessões (—). Sem tom de marketing.
- Não usar APIs depreciadas do Hugo: `hugo.Data` (não `.Site.Data`), `site.Language.Locale` (não `LanguageCode`), qualidade de imagem por formato em `[imaging.jpeg]` e `[imaging.webp]`.
- Sem Node, npm, PostCSS, Tailwind ou JavaScript de terceiros. JavaScript só se indispensável, inline e pequeno.
- Fontes e assets sempre locais; nada de CDN.
- Nomes de arquivo em minúsculas e sem espaço (Linux e GitHub Pages diferenciam maiúsculas).
- Não inventar conteúdo factual (publicações, cargos, resultados de projeto). Quando faltar informação, deixar comentário `<!-- Preencher: ... -->` e avisar.
- Não publicar detalhes do estágio sem confirmação explícita do Daniel.

## Design

Paleta (variáveis em `:root`, com versão escura em `prefers-color-scheme: dark`):

| Token | Claro | Escuro | Uso |
|---|---|---|---|
| `--bg` | `#f3f5f7` | `#10161d` | fundo |
| `--ink` | `#18202b` | `#e2e7ed` | texto |
| `--muted` | `#586474` | `#93a0af` | texto secundário |
| `--rule` | `#d3dae2` | `#2a3440` | divisórias |
| `--accent` | `#b4400e` | `#f0883e` | sublinhado de links, foco |
| `--trace` | `#3c6e9f` | `#6fa3d6` | traço espectral |

Tipografia: Spectral (texto e títulos), Public Sans (navegação, datas, metadados, rodapé). Coluna de texto com no máximo 40rem. O traço espectral é o único elemento decorativo; evitar adicionar outros (gradientes, cards com sombra, animações de entrada, rótulos em caixa alta).

## Pendências

1. Adicionar PDFs das publicações em `static/files/` quando permitido.
2. `content/cv.md`: preencher estágio atual e revisar lista de skills.
3. Revisar a bio em `content/_index.md`.
4. Rascunhos em `content/notes/quartz-oscillator.md` e `content/projects/gps-signal-simulation.md` (este último é só exemplo de estrutura, com capa de exemplo em `assets/img/covers/gps-l1-psd.png`, gerada sinteticamente).
5. Domínio próprio (opcional): `allaodan.com` não tem DNS hoje. Se for usado, ajustar `baseURL`, criar `static/CNAME` e configurar o domínio em Settings > Pages.
