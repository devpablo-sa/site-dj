# RPN Som e Iluminações — Design System

Formalização dos tokens e componentes já implementados em `index.html`. Extraído em 2026-09-13, sem alterar o site — `tokens.css` é um superset compatível com o `:root` atual (mesmos nomes, mesmos valores resultantes).

Arquivos:
- `tokens.json` — três camadas (primitiva → semântica → componente) em formato W3C DTCG.
- `tokens.css` — as mesmas camadas como CSS custom properties, prontas para uso.

## Identidade visual

| Papel | Claro | Escuro |
|---|---|---|
| Fundo (`paper`) | `#FAF6EF` | `#14111C` |
| Texto (`text`) | `#221D2C` | `#F3EEE6` |
| Amber (marca/CTA) | `#C97F17` / fill `#E3A13A` | `#F0B457` (ambos) |
| Magenta (acento) | `#9A2C71` / fill `#D6409F` | `#E762B4` (ambos) |
| Borda | `#E4DBC8` | `#322B3D` |
| Muted | `#6B6277` | `#B8AFC4` |

`ink` (`#14111C`) é fixo nas seções hero, faixa escura de diferenciais e CTA final — não muda com o tema, é uma decisão de arte direcional, não um valor de tema.

**Sem sombras.** O site não usa `box-shadow` em nenhum lugar — profundidade vem de blocos de cor sólida e bordas de 1px. Isso é intencional; não inventamos uma escala de elevação não utilizada.

## Tipografia

Três famílias via `@font-face` inline (base64):
- **Bebas RPN** — display condensado, uppercase (H1/H2, números grandes, marca)
- **Manrope RPN** — corpo/UI
- **Plex RPN** — mono (labels, eyebrow, nav, timestamps, microcopy de formulário)

A escala de `font-size` original tinha várias variações de décimo de rem quase idênticas (ex: `.92/.94/.95/.96rem` todas usadas como "texto secundário de card"). Consolidei essas variações em uma escala nomeada — ver tabela de mapeamento em `tokens.json`. Isso não muda a renderização atual (os valores ficam a <0.06rem do original) e evita que a próxima seção copiada traga mais um valor levemente diferente.

## Espaçamento

Escala bespoke (não é grid de 4px) com 19 degraus reais extraídos do CSS, de `.4rem` a `3rem`, mais 4 tokens de layout responsivo (`clamp()`) para padding de seção e hero.

## Achado a corrigir

`#8A8098` (cinza-arroxeado, texto secundário sobre fundo escuro) aparece **hardcoded** em 5 lugares — labels do hero trust, labels e nota do formulário de orçamento — mas nunca foi promovido a variável CSS. Formalizei como `--on-ink-muted-2` / `semantic.color.on-ink-muted-2`. Recomendo trocar os hardcodes por essa variável na próxima limpeza, para evitar drift se a paleta mudar.

## Componentes documentados

| Componente | Classe no site | Tokens principais |
|---|---|---|
| Botão | `.btn` + `.btn-amber` / `.btn-ghost-on-ink` / `.btn-line` | `--btn-*` |
| Card de serviço | `.svc-card` | `--card-*` |
| Item de diferencial (faixa escura) | `.diff-item` | `--diff-*` |
| Marcador de timeline | `.tl-num` | `--tl-*` |
| Frame de mídia / galeria de fotos | `.frame`, `.photo-gallery` | `--frame-*`, `--photo-*`, `--caption-*` |
| Painel de formulário | `.orc-form` | `--form-*`, `--field-*`, `--label-*` |
| Acordeão FAQ | `.faq` | `--faq-*` |

### Botão — variantes

| Propriedade | `amber` (primário) | `ghost-on-ink` | `line` |
|---|---|---|---|
| Fundo | `--amber-fill` | transparente | transparente |
| Texto | `--on-amber` (`#221605`) | `--on-ink` | `--text` |
| Borda | — | `rgba(243,238,230,.4)` | `--border` |
| Hover | `brightness(1.06)` | borda `#F3EEE6` | borda `--amber` |
| Radius | `--radius-control` (2px) | idem | idem |

Não há estado `:active` ou `:disabled` definido no CSS atual — não inventei esses estados nos tokens; documentar apenas o que existe.

## Como adotar

`tokens.css` pode substituir o bloco `:root{...}` + `@media(prefers-color-scheme:dark)` + `[data-theme]` do `index.html` sem mudança visual (a camada semântica reproduz os mesmos nomes e valores). Os tokens de componente são novos — hoje esses valores estão hardcoded inline nas regras `.btn-amber`, `.svc-card`, etc. Adotá-los exigiria trocar os literais por `var(--btn-amber-bg)` etc. nas regras existentes; isso é um refactor separado, não incluso aqui.
