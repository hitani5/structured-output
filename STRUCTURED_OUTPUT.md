# Response format

## Structured responses

When a turn involves doing or diagnosing work — implementing, fixing, refactoring,
debugging, investigating, or explaining a bug — structure the final response in
sections, in this order:

```
## ✅ What I did          (always)
## 🔧 How I did it        (always)
## ❌ What failed         (only when something genuinely failed)
## 💡 Suggested next steps (only when there is something worth raising)
## 📋 To do               (only when something is open; always last)
```

No numbering. Order is what carries the meaning: outcome first, detail second,
open work last. Sections without content are omitted entirely — never write a
heading followed by "none" or "nothing to report".

### The fixed ends

`## ✅ What I did` always opens the response and `## 🔧 How I did it` always
follows it. When there are open items, `## 📋 To do` always closes the response —
nothing comes after it, no summary, no sign-off. `## 💡 Suggested next steps`,
when present, sits immediately before the to-do list.

Between `## 🔧 How I did it` and `## 💡 Suggested next steps` is a free band.
`## ❌ What failed` is the usual occupant, but another section belongs there when
a turn genuinely calls for one — findings from an investigation, a comparison
that drove a decision, output worth reading on its own. Give it its own heading
and a fitting emoji. Do not invent sections to pad a response.

### What belongs in each section

**✅ What I did** — Open with one line restating the goal in my own words, so a
session returned to hours later is immediately legible. Then the outcome: what
now works that didn't before. Results, not process. Keep it scannable.

**🔧 How I did it** — The detail: approach, files touched (as `path:line`),
decisions and trade-offs, commands run, what the output showed, why one route was
taken over another. This section has no length limit. Everything explanatory goes
here rather than being cut.

**❌ What failed** — Reserved for things that must draw attention: work that could
not be completed, an external service or dependency being unavailable, tests
failing, a blocker that stopped progress. The presence of this heading is itself
the signal, so it has to stay meaningful — do not open it for minor caveats.

Nothing may be quietly dropped to keep this section away. When something was
skipped, assumed under ambiguity, or left partly done but doesn't clear the bar
above, it still gets stated plainly inside `## 🔧 How I did it`. The section is
conditional; the honesty is not.

**💡 Suggested next steps** — Improvements, follow-up work, or concerns worth
raising, when there are any worth raising. Omit the section entirely when there
are none. Do not pad it.

**📋 To do** — Markdown checkboxes, one per open item:

```
- [ ] Specific, actionable item
```

Each item must be concrete enough to act on without rereading the sections above.
Include only genuinely open work — not things already completed.

### Other formatting

Three conventions beyond the headings, each chosen because it survives being
skimmed. Nothing else is needed.

**The goal line is a blockquote.** The opening line of `## ✅ What I did` is
written as a blockquote so it separates visually from the outcome text beneath it:

```
> Goal: one line, in my own words, describing what this turn was for.
```

This is the first thing read when returning to a terminal after time away. It
answers "what is this session even about" before anything else is parsed.

**Code changes are shown as diffs, not described.** When a turn changed code,
put the essential change in a fenced `diff` block inside `## 🔧 How I did it`.
It renders with real color — additions green, removals red — which is the only
place color is available:

```diff
- $total = $items->sum(fn ($i) => $i->price * $i->qty);
+ $total = $items->sum->subtotal();
```

Show only the lines that carry the decision. Never paste a whole file, and never
use a diff block for something that isn't a change.

**Two-dimensional information goes in a table.** When facts have more than one
axis — files and what happened to each, options and their trade-offs, checks and
their results — a table is read at a glance where prose has to be parsed:

| File | Change |
|---|---|
| `app/Services/Cart.php:42` | Replaced the manual sum with a relation aggregate |

Write file references as `path:line` throughout. They are clickable in the
terminal.

### What to avoid

These compete with the headings for attention and make the response harder to
scan, not easier:

- Bold lead-ins on every bullet
- Horizontal rules between sections — the headings already separate them
- Strikethrough
- Subheadings inside a section, unless `## 🔧 How I did it` genuinely runs long

### Emoji discipline

The heading emoji are fixed: ✅ 🔧 ❌ 💡 📋. Always use them, always the same ones,
always in that order. They are the visual anchors that make a section findable at
a glance when scrolling back through a terminal.

Keep emoji out of body text, bullet points and to-do items. The headings only
stand out because nothing around them competes. Decorating the prose destroys the
signal the headings carry.

### Do not compress

This format exists to make responses scannable, not shorter. Never drop detail,
caveats, or findings to make a section tidy. Long output is fine; unstructured
output is not. If something doesn't fit the other sections, it belongs in
`## 🔧 How I did it`.

### When to use this format, and when not to

The test is what the message asks for, not whether files happened to change.

**Use the format** when asked to *do* something: implement, fix, refactor,
debug, investigate a real problem, or explain a bug in actual code. These turns
leave behind state worth tracking.

**Answer in plain prose, with no sections**, when asked *about* something:
opinions, explanations, trade-offs, design and approach questions, planning
conversations, configuration discussions, or anything exploratory. A question
phrased as "is there a way to…", "what do you think about…", "can we…",
"why did you…" is a discussion, and stays a discussion even if a file gets
edited along the way. When a prose turn did change something, say so plainly in
a sentence — never hide a change just because the response has no sections.

Also plain prose for quick factual questions, clarifying questions asked back
mid-task, and progress updates while work is still ongoing. The format belongs
on the response that closes out the work.

When genuinely in doubt, ask whether `## 📋 To do` would have any items in it.
If it would be empty, the response almost certainly wants prose.

### Switching modes

Plain language switches the mode in either direction, and it sticks.

Phrases like "quick question", "just curious", "let's discuss", "talk me
through", "what do you think", "no sections" or "don't format this" mean prose.
Once that switch happens it stays in prose for the rest of the discussion — do
not silently revert to sections on the next turn. The discussion ends when an
instruction to do work arrives, and the format resumes there.

Phrases like "format that", "as sections", or "give me the sections" mean the
format, including as a request to restate something already said in prose.
