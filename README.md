# mermaid-slide-deck

Turn any logic, code path, or architecture into a **horizontal slide deck** — a single,
self-contained HTML file you flip through left-to-right (no vertical scroll), built around
Mermaid diagrams instead of walls of text.

It ships as a [Claude Code](https://docs.claude.com/en/docs/claude-code) **Agent Skill**, but the
`TEMPLATE.html` is a plain HTML file you can also use by hand — no build step, no framework.

[![live demo](https://img.shields.io/badge/live%20demo-cskwork.github.io-60a5fa?logo=github)](https://cskwork.github.io/mermaid-slide-deck/) ![no vertical scroll](https://img.shields.io/badge/scroll-horizontal%20only-34d399) ![license](https://img.shields.io/badge/license-MIT-aebccf)

## Live demo

- **Landing page** → https://cskwork.github.io/mermaid-slide-deck/
- **Example deck** → https://cskwork.github.io/mermaid-slide-deck/examples/checkout-validation.html — a generic "validate before you charge" walkthrough. Flip with arrow keys, Space, on-screen buttons, or swipe.

## Why

Explaining "how this works" in chat or a wiki usually produces a long scroll of prose. People
skim it and miss the logic. A deck forces the opposite discipline:

- **One slide = one screen = one message.** If it doesn't fit, split the slide.
- **Diagrams are the content.** Flowcharts, sequence diagrams, and before/after pairs carry the
  load; prose shrinks into small coloured panels (WHY / CAUTION / FACTS).
- **Flip, don't scroll.** Arrow keys, Space, on-screen buttons, touch swipe, and dots.

## Quick start (no install)

1. Copy [`TEMPLATE.html`](TEMPLATE.html).
2. Fill the `{{...}}` placeholders, add or remove `<section class="slide">` blocks (the navigation
   counts them automatically).
3. Open it in a browser. Done.

See a finished deck: [`examples/checkout-validation.html`](examples/checkout-validation.html)
(a generic "validate before you call the payment service" walkthrough).

## Use as a Claude Code skill

```bash
git clone https://github.com/cskwork/mermaid-slide-deck.git ~/.claude/skills/mermaid-slide-deck
```

Then it auto-triggers when you ask things like *"explain this logic visually / as slides / as a
deck"*, or in Korean *"시각적으로 로직 만들어줘 / 슬라이드로 설명 / 다이어그램으로 보여줘"*. You can also invoke
it explicitly: "use the mermaid-slide-deck skill". The agent reads your real code, decomposes it
into slides, validates every Mermaid block, writes the HTML, and opens it.

## What's inside

| File | Purpose |
|------|---------|
| `SKILL.md` | Skill router + the slide-decomposition workflow and checklist |
| `TEMPLATE.html` | Self-contained deck skeleton (dark theme, scroll-snap nav, 8 slide types) |
| `MERMAID-SAFE-SUBSET.md` | The Mermaid syntax subset that renders without errors |
| `examples/checkout-validation.html` | A complete generic example deck |

## How it works

- A flex row with `scroll-snap-type: x mandatory`; each slide is `flex: 0 0 100vw; height: 100vh`.
  `html, body { overflow: hidden }` kills vertical scroll.
- An `IntersectionObserver` keeps the counter, progress bar, and dots in sync when you swipe.
- Mermaid loads from CDN (`mermaid@10` ESM). Diagrams still carry a `<figcaption>` so the meaning
  survives even if the CDN is blocked.
- Type sizes use `clamp()` so text scales with the viewport.

## Accessibility

Semantic `<section role="group">` slides, a skip link, visible `:focus-visible` outlines,
`role="progressbar"`, keyboard navigation (Arrow / Space / Home / End), `prefers-reduced-motion`
support, and colour always paired with text (never colour alone).

## Credits

- Diagrams: [Mermaid](https://mermaid.js.org/)
- Design discipline informed by award-criteria web-design principles (clarity, visual hierarchy,
  content-as-design, accessibility).

## License

MIT — see [LICENSE](LICENSE).
