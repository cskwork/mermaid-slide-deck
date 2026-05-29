# Mermaid safe subset

Stay inside this subset so diagrams render without "Syntax error in text" in the browser.
Tested against Mermaid v10 (CDN).

## Hard rules

| Rule | OK | Avoid |
|------|----|-------|
| Wrap every multi-character label in double quotes | `id["Some Label"]` | `id[Some Label]` |
| Quote edge labels too | `A -- "retry" --> B` | `A -- retry --> B` |
| Multi-line label: `<br/>` inside double quotes | `"Line one<br/>Line two"` | a bare newline |
| No pipe `\|` inside any label | `"retry or give up"` | `"retry \| give up"` |
| No single quote `'` inside a double-quoted label | `"10 minutes"` | `"'10 minutes'"` |
| No HTML entities | `"max"` | `"&gt;= max"` |
| Quote edge/labels containing `()` | `R -- "list()" --> X` | `R -- list() --> X` |
| Node IDs: ASCII only, no spaces | `S2`, `PayGw` | `Pay Gw` |
| Non-ASCII text (Korean, etc.): fine inside quotes | `["주문 생성"]` | unquoted with parens |

The pipe `|` is the edge-label delimiter, so it breaks the parser even inside quotes in some
versions — never put it in a label. Use "or" or a line break instead.

## Node shapes

| Shape | Syntax |
|-------|--------|
| Rectangle (safest) | `A["Label"]` |
| Rounded | `A("Label")` |
| Stadium | `A(["Label"])` |
| Cylinder (DB) | `A[("Label")]` (keep label short, single line) |
| Diamond (decision) | `A{"Label"}` |

## Edge cheat sheet

| Edge | Syntax |
|------|--------|
| Arrow | `A --> B` |
| Arrow + label | `A -- "label" --> B` |
| Dotted | `A -.-> B` |
| Dotted + label | `A -. "label" .-> B` |
| Thick | `A ==> B` |

## Reusable styles (classDef)

```
flowchart LR
  X["stop"]:::stop
  Y["go"]:::ok
  classDef stop fill:#371a1a,stroke:#f87171,color:#fecaca
  classDef ok fill:#103027,stroke:#34d399,color:#a7f3d0
```

Attach a class with the `:::name` suffix right after the node definition.

## Pre-flight checklist

Before writing a diagram into the HTML, scan each block for:

- [ ] Every multi-character label is wrapped in double quotes
- [ ] No `|` inside any label
- [ ] No `'` inside any double-quoted label
- [ ] No `&gt;` / `&lt;` / `&amp;` HTML entities
- [ ] Cylinder shapes use short single-line labels

Quick grep over the generated file (all counts should be 0):

```bash
awk '/class="mermaid"/{f=1} f{print} /<\/div>/{if(f)f=0}' deck.html > /tmp/mm.txt
grep -c '|' /tmp/mm.txt; grep -cE '&gt;|&lt;|&amp;' /tmp/mm.txt; grep -c "'" /tmp/mm.txt
```
