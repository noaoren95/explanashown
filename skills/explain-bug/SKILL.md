---
name: explain-bug
description: Explain a bug visually before any prose. Use whenever you report the root cause of a bug, error, crash, failing behavior, regression or unexpected output you investigated, or when the user asks "why is this broken", "what's the root cause", "what went wrong" or "explain this error". Leads with a diagram showing where it broke and where the bad state came from, then three short lines (what broke, why, fix), and nothing more unless asked.
---

# Explain a bug: picture first

The user doesn't have the time or energy to read a long paragraph. Your job is to make the root cause **visible** before you write a single sentence of explanation.

## Before you draw

1. Load the shared style: use the Skill tool with `explanashown:visual-style` and follow it (renderer choice, colors, markers, the three lines).
2. Investigate first. Don't draw a guess. You need these four facts, each tied to real code (file:line, function, endpoint, value):
   - **Trigger:** what the user/system did (request, input, event, test).
   - **Origin:** where the bad value or state was *created* (often not where it crashed).
   - **Break point:** where it visibly failed (exception, wrong output, hang).
   - **Blast radius:** what didn't happen or went wrong downstream because of it.
   If you can't pin down one of them, draw it as a box labeled `?` and say so in the "Why" line. Never invent it.

## What to draw

A left-to-right flow from **trigger → origin → break point → blast radius**, with:

- the **break point** in red (the focus),
- the **origin** in orange if it's a different place from the break point (this is usually the real root cause, so make it obvious),
- the **actual bad value** on the edge where it travels (`null`, `"undefined"`, `status=500`, `[]`),
- skipped/unreached steps greyed out,
- optionally the fix as a green dashed arrow on the origin box.

Pick the shape that fits the bug instead of forcing a flow when it's clearer:

| Bug type | Better shape |
|---|---|
| Wrong data passed along | Flow with values on edges (default) |
| Race condition / ordering | Two swimlanes on a timeline, the bad interleaving in red |
| Expected vs actual output | Two side-by-side columns, differing cells in red |
| State machine stuck / wrong transition | States as boxes, the wrong transition in red |
| Config / environment mismatch | Two boxes (works here / breaks there), the differing setting in red |

## Then write exactly this

> **What broke:** `<the visible failure, with location>`
> **Why:** `<the root cause at the origin, one sentence>`
> **Fix:** `<the change, with file:line>`
>
> Want the full trace, the evidence, or the patch?

Stop there. No paragraphs above the picture, no recap below the three lines. If you already fixed it in this turn, change the last line to **Fixed:** with the file:line and how you verified it.

## If the user asks for more

Then (and only then) give the detail they asked for: the stack trace, the evidence (log lines, values you inspected), alternative hypotheses you ruled out, or the patch. Keep reusing the same diagram if you show it again, so the reader's mental map doesn't change.
