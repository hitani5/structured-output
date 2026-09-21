# Structured Output

A Claude Code plugin that makes Claude close out work turns in fixed, scannable
sections instead of a wall of text.

The goal is not shorter responses — it is responses you can pick up at a glance
after working in another terminal for twenty minutes. You read the top to learn
what the session was for, and the bottom to learn what is still open. Everything
between is there when you want it.

## What a response looks like

```
## ✅ What I did
> Goal: one line describing what this turn was for.
What now works that didn't before.

## 🔧 How I did it
The detail — approach, files as `path:line`, trade-offs, commands and their
output. No length limit; this is where everything explanatory goes.

## ❌ What failed
Only appears when something genuinely failed or blocked progress.

## 💡 Suggested next steps
Only appears when there is something worth raising.

## 📋 To do
- [ ] Specific, actionable item
```

`✅` and `🔧` always open. `📋` always closes, when anything is open. `❌` and `💡`
appear only when they have real content — so seeing `❌` means something actually
went wrong.

Discussion stays prose. Asking Claude *about* something — trade-offs, design,
opinions, "is there a way to…" — gets a normal conversational answer. Only turns
where Claude *does* work get the sections. You can switch modes in plain language
("quick question", "let's discuss", "no sections" / "format that", "as sections").

## Install

```
/plugin marketplace add byte5/claude-structured-output
/plugin install structured-output@byte5
```

Then restart your session, or run `/clear`.

To turn it off at any point: `/plugin` and disable it. Nothing is written to your
own config, so disabling fully reverts the behavior.

## How it works

A `SessionStart` hook reads `STRUCTURED_OUTPUT.md` and injects it into the session
as context. That is the whole mechanism — it does not modify your `~/.claude/CLAUDE.md`
or any other file you own, which is why it can be removed cleanly.

Because it is a hook, it runs a small shell script when a session starts. The
script is `hooks/session-start`; it reads one file and prints JSON.

## Feedback

The rules live in one file: [`STRUCTURED_OUTPUT.md`](STRUCTURED_OUTPUT.md). If
something is off, it is worth saying *which rule* produced the behavior, since
that is usually a one-line fix.

Things worth reporting:

- `❌` appearing for things that don't warrant it, or not appearing when it should
- Sections drifting — explanation ending up in `✅` instead of `🔧`
- Tables wrapping or rendering badly in your terminal
- The format firing on discussion turns, or failing to fire on work turns
- Emoji not rendering cleanly in your terminal font

## Status

v0.1.0 — in testing. Verified working on Claude Code 2.1.278 on macOS.
