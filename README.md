# explanashown

**Show it, then say it.** A Claude Code plugin that makes Claude lead every explanation with a diagram (what happened, where it broke, where it came from) followed by three short lines instead of a wall of text.

## Skills

| Skill | Triggers when | Status |
|---|---|---|
| `explain-bug` | Claude reports a bug's root cause, or you ask "why is this broken?" | ✅ |
| `explain-test` | Explaining a failing or new test | planned |
| `explain-improvement` | Proposing a refactor or improvement | planned |
| `visual-style` | Loaded by the others: shared colors, markers, layout | internal |

Skills load automatically when they match. You can also call one directly, for example `/explanashown:explain-bug`.

## Install

```
/plugin marketplace add noaoren95/explanashown
/plugin install explanashown@explanashown
```

To try a local copy before pushing:

```
/plugin marketplace add ./path/to/explanashown
/plugin install explanashown@explanashown
```

## What you get

In the Claude desktop app the diagram is drawn inline in the chat. In a plain terminal, Claude falls back to a text diagram:

```
 request ──▶ [ router ] ──"2026-11-25"──▶ [~ parseDate() ~] ──null──▶ [!! saveOrder() !!] ──✗──▶ ( email sent )
```

> **What broke:** `saveOrder()` crashes on `null.getTime()` (orders.py:142)
> **Why:** `parseDate()` returns `null` for ISO dates without a time part
> **Fix:** accept date-only strings in `parseDate()` (dates.py:31)

## Adding a new skill

1. Create `skills/<name>/SKILL.md` with `name` and a `description` that says when it should trigger.
2. Start its body with: load `explanashown:visual-style` and follow it.
3. Describe what the diagram's focal point is for this use case, and what the three lines are called.
4. Bump `version` in `.claude-plugin/plugin.json`.
