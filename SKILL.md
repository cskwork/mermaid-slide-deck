---
name: mermaid-slide-deck
description: Turn logic, a code path, an architecture, or a verification result into a horizontal slide deck (dark-theme PPT-style, Mermaid diagrams, coloured panels) as a single self-contained HTML file. Use when the user asks to explain how something works visually, as slides, as a deck, or with diagrams/flowcharts. Korean triggers — "시각적으로 로직/코드 만들어줘", "슬라이드로 설명", "비주얼하게 설명", "PPT 형식으로", "다이어그램으로 설명", "플로우로 보여줘".
---

# Mermaid Slide Deck

Build a **horizontal, flip-through slide deck** (single HTML) that explains code/logic.
No vertical scroll — one slide = one screen = one message. Diagrams lead; prose is minimal.

## Quick start

1. Read the real code/query/flow you are explaining first — verify, don't guess.
2. Copy [`TEMPLATE.html`](TEMPLATE.html), fill the slides (reuse the CSS/JS skeleton as-is).
3. Save and open it (`open` / `xdg-open` / `start`). Tell the user the absolute path.

## Slide decomposition (default order — adapt to the subject)

- Title — headline + one-line takeaway + navigation hint
- Overview / analogy — a 30-second analogy for non-experts (3 cards)
- Core flow — `flowchart LR` (horizontal suits wide screens); colour branches stop vs go
- Relationship / sequence — `sequenceDiagram` for "what is created when"
- Key point (WHY) — the single most confusing thing, as one large panel
- Before / After — what the change prevents (two diagrams + a value panel)
- Evidence / data — a measured table + FACTS panel + stat cards
- One-slide summary — three key points + sources

## Authoring rules (checklist)

- [ ] One message per slide. Content must fit one screen (100vh) — if it overflows, split the slide.
- [ ] Diagrams are the content. Move long prose into coloured panels (WHY green / CAUTION amber / FACTS grey).
- [ ] Every Mermaid block follows the safe subset — no pipe `|`, no single quotes, no HTML entities (`&gt;`), quote multi-char labels. See [`MERMAID-SAFE-SUBSET.md`](MERMAID-SAFE-SUBSET.md).
- [ ] After writing, grep the mermaid blocks for risky patterns (snippet below) — all counts 0.
- [ ] Navigation: prev/next buttons + keyboard (Arrow / Space / Home / End) + touch swipe + dots + progress bar.
- [ ] Accessibility: `role` / `aria-roledescription="slide"` / skip link / `:focus-visible` / `prefers-reduced-motion`. Pair colour with text (never colour alone).
- [ ] Clarity & hierarchy: single purpose, the title slide answers what/why in 5 seconds, large type, real data as content.
- [ ] No emojis. Keep code identifiers in their original form.

## Mermaid safety check (run after writing)

```bash
F=<generated html>
awk '/class="mermaid"/{f=1} f{print} /<\/div>/{if(f)f=0}' "$F" > /tmp/mm.txt
grep -c '|' /tmp/mm.txt; grep -cE '&gt;|&lt;|&amp;' /tmp/mm.txt; grep -c "'" /tmp/mm.txt   # all 0
```

If `mmdc` is available, also render-check. Otherwise rely on the safe subset + the grep above, and
ask the user to confirm the diagrams render in the browser.

## Technical notes

- Single self-contained HTML. Mermaid loads from CDN (`mermaid@10` ESM); if blocked, the
  `<figcaption>` still conveys each diagram's meaning.
- Horizontal deck: `display:flex` + `scroll-snap-type:x mandatory`, slides `flex:0 0 100vw; height:100vh`.
  `html,body{overflow:hidden}` removes vertical scroll.
- Current slide is tracked with `IntersectionObserver` (keeps counter/dots/progress synced on swipe).
- Type sizes use `clamp()`. To make everything bigger, raise the `:root` token values.
- Full skeleton, theme, and JS live in `TEMPLATE.html`.
