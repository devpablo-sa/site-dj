# RPN Som e Iluminações — Site

Site institucional de aluguel de som, iluminação e DJ para casamentos, aniversários, churrascos e eventos corporativos em Ribeirão Preto e região.

## Estrutura

- `index.html` — página única do site (Home + Serviços + Portfólio + FAQ + Orçamento). Documento HTML autocontido: fontes e imagens embutidas como `data:` URIs, sem dependências externas.
- `escopo-projeto.html` — documento interno de planejamento (identidade visual, arquitetura, copy). Não faz parte da navegação pública do site.

## Orçamento via WhatsApp

O formulário de orçamento (`#orcamento`) não usa backend: ao enviar, monta uma mensagem com os dados preenchidos e abre uma conversa no WhatsApp da RPN com o texto pronto.

## Segurança

- Documento único e autocontido — sem chamadas de rede, sem scripts externos, sem cookies, sem armazenamento local.
- `Content-Security-Policy` restritiva via `<meta>`: bloqueia carregamento de recursos externos (`connect-src 'none'`, `object-src 'none'`), permite apenas fontes/imagens embutidas (`data:`) e recursos inline já auditados.
- Todos os links externos (`target="_blank"`) usam `rel="noopener noreferrer"`.
- Nenhuma entrada do usuário é escrita no DOM (sem `innerHTML`/`document.write`); os campos do formulário só são lidos e codificados com `encodeURIComponent` para montar o link do WhatsApp.
- `frame-ancestors` incluído na CSP como reforço, mas por especificação só é aplicado quando entregue via cabeçalho HTTP — se o site for hospedado atrás de um proxy/CDN com controle de headers, vale reforçar lá (`X-Frame-Options: DENY`, `Strict-Transport-Security`, `X-Content-Type-Options: nosniff`).

## Como editar

Ambos os arquivos são HTML puro — abra em qualquer editor e edite diretamente. Fontes (Bebas Neue, Manrope, IBM Plex Mono) e imagens de portfólio já estão embutidas em base64 dentro do próprio arquivo.
