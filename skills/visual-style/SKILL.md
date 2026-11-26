---
name: visual-style
description: Shared visual rules for every explanashown skill (explain-bug, explain-test, ...). Load it before drawing any explanashown diagram. Not meant to be used on its own.
user-invocable: false
---

# explanashown visual style

Every explanashown answer follows the same contract: **picture first, three lines second, details only on request.** The reader should understand what happened from the picture alone, in under 10 seconds.

## 1. Pick the renderer

1. **Inline visual tool available** (for example `show_widget` in the Claude desktop app; call its `read_me` first if it requires one): draw an SVG diagram inline. This is the default. When something *moves* (data, requests, events, time), make it an animated HTML+SVG widget with a "Now / Expected" toggle instead of a static picture. Showing the motion beats describing it.
2. **No inline visual tool** (plain terminal, IDE panel): draw a text diagram inside a fenced code block, using the same layout rules below. Don't use Mermaid, because terminals don't render it.

Never skip the picture because the renderer is limited. A small text diagram still beats a paragraph.

## 2. Layout rules

- **Flow goes left to right** (or top to bottom if there are more than 5 steps). Each box is one real thing: a function, file, service, component or piece of data, labeled with its real name (`parseDate()`, `orders.py:142`, `POST /api/jobs`).
- **At most 7 boxes.** If the real flow is longer, collapse the healthy parts into one box, for example "auth ✓ (3 steps)".
- **Edge labels carry the data**: write the actual value flowing between boxes when it matters (`"2026-11-25"` → `null`).
- **Every diagram has exactly one focal point**: the thing the reader came for (the break, the failing assertion, the change).
- **Assume the reader doesn't know the project.** Every box names where it lives (`file:line` + function) so nobody has to ask "where is that?"
- **Words are a last resort.** Inside the picture: titles, file:line, and ≤4-word ✓/✗ labels. No sentences, no field tables (trigger / happened / expected). If a reader says "too many words", turn text into motion, color or a toggle; don't just shorten it.

## 3. Color and marking (same in every skill)

| Meaning | Inline SVG | Text fallback |
|---|---|---|
| Healthy / working as intended | neutral or green | `[ box ]` |
| **Where it broke / the focus** | red fill or thick red border | `[!! box !!]` |
| Origin of the bad value/state (if different from where it broke) | orange/amber | `[~ box ~]` |
| Not reached / skipped because of the break | grey, dashed | `( box )` |
| Proposed fix / new path | green dashed arrow | `==fix==>` |

Use CSS variables or theme-safe colors in SVG so it reads in light and dark mode. Always pair color with a marker (icon, thick border, label) so it doesn't rely on color alone.

## 4. The three lines (after the picture)

Exactly three short lines, each at most ~15 words, with these bold labels (skills may rename them to fit):

- **What broke:** ...
- **Why:** ...
- **Fix:** ...

## 5. Then stop

No further paragraphs by default. End with one line offering more, for example: *"Want the full trace, the evidence, or the patch?"* Only write the long explanation if the user asks.

## Text fallback example

```
 request ──▶ [ router ] ──"2026-11-25"──▶ [~ parseDate() ~] ──null──▶ [!! saveOrder() !!] ──✗──▶ ( email sent )
                                            returns null for          crashes: null.getTime()
                                            ISO dates without time
```
