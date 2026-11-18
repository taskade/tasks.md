---
name: tasks-md
description: Create and maintain a TASKS.md build ledger at the repo root so work survives a context reset. Use when starting a multi-step task, resuming an interrupted one, when the user asks to track progress or "remember where we left off", or when a request carries several clauses that can be silently half-built. Not for a to-do queue of future work or a one-off question that changes no files. Defines six sections (Destination, Open decisions, Out of scope, Requirements, Done, Knowledge), the Requirements block with its States line, the dated-entry format, and the evidence bar a clause must clear before it is recorded as done.
license: Apache-2.0
compatibility: Requires read/write access to the repository working tree. No network or system packages needed.
metadata:
  spec-version: "1.0"
  spec-url: "https://github.com/taskade/tasks.md"
allowed-tools: Read Write Edit Glob Grep
---

# TASKS.md

TASKS.md is the ledger you keep of what was asked, what was decided and excluded, what you built with evidence, and what you learned. It lives at the repository root. It is the one file that survives a context reset, so treat every write to it as the only copy of your plan.

AGENTS.md says how to work. TASKS.md says what happened.

## When to use this skill

- The request carries more than one clause ("add the form AND notify me").
- The work spans more than a turn or two.
- You are resuming work that another session started.
- The user asks you to track progress, or to remember where you left off.

A one-clause edit still gets a `Done` entry. It does not need the map.

## The file

Six sections, in this order. The first four are the map: live state, edited in place, never dated. The last two are the log: dated entries, newest first.

| Section | Holds |
|---|---|
| Destination | The finished result, in the words of the user. One or two lines. |
| Open decisions | Questions that are still open, one line each. An empty section is the normal state. |
| Out of scope | What you excluded on purpose, with the reason. |
| Requirements | One bullet per clause of the request, with its state annotated in place. |
| Done | One dated entry per milestone or edit, newest first. |
| Knowledge | Durable facts: ids, decisions with reasons, learnings. One fact per bullet. |

The template in `assets/TASKS.md.template` is the file to copy when none exists. The full grammar is in `references/spec.md`.

## Procedure

### 1. Open the file first

Before you plan, read `TASKS.md` at the repository root. If it does not exist, create it once from the template and reuse it from then on. Never create a second one. Never rename or delete it.

On resume, read in this order and stop when you have enough: Destination, the Requirements bullets still OPEN, Open decisions, the newest Done entry, then only the Knowledge bullets the current work touches. Then continue the work without a recap. Act on the newest `Next` bullet unless the user's latest message made it obsolete or asks for something else.

### 2. Chart the map when the work needs one

If the work spans more than a turn or two, write Destination in the user's own words. Add Open decisions and Out of scope as they appear. A small edit skips this step.

### 3. File the clauses before you build

Split the request into its clauses, in the user's own words, and write them under Requirements, one bullet per clause, every one `OPEN`. Keep the `States:` line as the first child, verbatim:

```
## Requirements [0/3]

  States: OPEN | BUILT(!evidence) DEFAULTED(@the default you chose) DECLARED(@what only the user can supply)
  - Contact form files an inquiry: OPEN
  - Alert me on every new inquiry: OPEN
  - Monthly refresh of the gallery: OPEN
```

Write down the requirement, never the secret. If the request includes a password, key or token, record what it is for and leave out the value.

### 4. Build, then settle each clause with evidence

A clause leaves `OPEN` only when you hold the evidence, and the bullet records the evidence:

- `BUILT - <what you observed>` when it exists and works.
- `DEFAULTED - @<the default you chose>` when you chose a reversible default for a detail you cannot know, and built with it.
- `DECLARED - @<what only the user can supply>` when you cannot build it without something only the user has.

Append ` - closed YYYY-MM-DD HH:mm UTC` to the bullet. Recount the `[n/N]` cookie from the bullets. Never increment it.

The evidence bar is in `references/evidence.md`. In short: the artifact settles a clause that says something EXISTS. Only an outcome you saw settles a clause that says something HAPPENS. Intent settles neither.

### 5. File a Done entry at each milestone and after every edit

```
2026-08-20 14:12 UTC - Contact form and inquiry alert shipped
  - What changed: <files, records, jobs, named>
  - Where it lives: <the artifact, linked by name>
  - Next: <the one thing to do next>
```

Newest first. Three or four bullets. Never a bare step count. Name the clauses the entry settled, but do not copy their state here. The state lives only on the Requirements bullet.

### 6. Write back what you learned

File at least one Knowledge entry per build: ids a later turn will otherwise re-derive, decisions with the reason, and whatever cost a retry. One fact per bullet. When a fact you learn is about something a bullet already covers, update that bullet. When work proves a bullet wrong, correct or delete it in the same turn.

### 7. Close the turn from the file, not from memory

Before you write the closing message, read Requirements again. Every clause is `BUILT`, `DEFAULTED` or `DECLARED`, or the summary names it as missing in one plain line. A clause you have not built is unfinished work. Build it, or declare it. It is never `BUILT`.

## Rules that are easy to break

- Do not remove a clause because it is finished. The list keeps every clause for the whole build.
- Use **verified**, **confirmed**, **tested** or **complete** only for an outcome you saw happen. Code you read shows that a behavior was written, not that it runs.
- If your closing suggestions include "Add X", then X does not exist yet, and the summary says so first.
- A defect you find in your own work ends as Fixed or Declared. No other outcome exists.
- If the user reports the same problem a second time, your reading of the code no longer counts as evidence. Change your approach.
- The file tells you where the work stopped. The latest message of the user decides what the turn is for.

## References

- `references/spec.md` - the section list, the Requirements grammar, the entry grammar, the stamp.
- `references/evidence.md` - the evidence bar in full.
- `assets/TASKS.md.template` - the seed file.
