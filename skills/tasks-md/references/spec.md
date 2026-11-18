# TASKS.md format, condensed

The normative text is `SPEC.md` at the repository that hosts this skill. This page is the working subset.

## Location

One `TASKS.md` at the repository root. A permanent singleton: never renamed, never deleted, never duplicated. Create it once when missing, from `assets/TASKS.md.template`.

## Sections, in order

| # | Section | Kind | Rule |
|---|---|---|---|
| 1 | Destination | map | One or two lines, in the words of the user. Rewrite only when the goal changes. |
| 2 | Open decisions | map | One line each. Delete a line when it resolves and file the answer in Knowledge with the reason. |
| 3 | Out of scope | map | What was excluded, with the reason. |
| 4 | Requirements | map | One bullet per clause. Never dated, never pruned, edited in place. |
| 5 | Done | log | Dated entries, newest first, append-only. |
| 6 | Knowledge | log | One fact per bullet. Update, do not duplicate. |

Map sections 1 to 3 can be absent when the work never needs them. Sections 4 to 6 are always present. When the map and the log disagree, the map wins.

## Requirements grammar

```
## Requirements [n/N]

  States: OPEN | BUILT(!evidence) DEFAULTED(@the default you chose) DECLARED(@what only the user can supply)
  - <clause in the user's words>: <STATE>[ - <evidence or sentence>][ - closed YYYY-MM-DD HH:mm UTC]
```

- The `States:` line is the first child, verbatim. Left of `|` needs work. Right of `|` is terminal. `!` marks evidence you observed. `@` marks a sentence the user will read.
- Every clause starts `OPEN`.
- `n` counts clauses that left `OPEN`. `N` counts all clauses. Recount from the bullets on every write. Never increment. `N` never decreases.
- A clause that names a scheduled or event-driven behavior lands `BUILT` or `DECLARED`, never inline code that imitates it.
- Requirements is the one place the clause list lives. Do not copy it into Done.

## Done entry grammar

```
YYYY-MM-DD HH:mm UTC - <short title>
  - What changed: <files, records, jobs, named>
  - Where it lives: <artifact, linked by name>
  - Next: <the one thing to do next>
```

Newest first. Three or four bullets. Never a bare step count. File one after an edit turn too.

`Where it lives` can carry a durable reference: a commit, a pull request, a release tag or a named test. Prefer one that still resolves after the working tree has moved on. The `Requirements` clause records what you OBSERVED; the `Done` entry records the pointer a reader follows later.

## Priority and defects

Priority is a tag written inline on a bullet, never a heading. Reorganizing the file by tier replaces the six sections and loses the map.

A PRE-EXISTING defect, one this work did not cause, goes in `Open decisions` naming the symptom and the choice it forces, or becomes a new `Requirements` clause if it must be fixed to finish.

A defect THIS work caused goes in no map section. Fix it in this turn or declare it, as the evidence rules say. Do not record your own defect as an open decision: that is the delay the evidence rules forbid.

Never open a standing defect section. A ledger with a permanent bug list has become a tracker.

## Knowledge rules

- One fact per bullet.
- Edit the existing bullet. Do not add a second bullet on the same subject.
- If work shows a bullet is wrong, fix or delete it in the same turn.
- A bullet was true when it was written. If a tool result disagrees, trust the tool and fix the bullet.
- A fact that outgrows a bullet becomes its own file. The bullet shrinks to a link and a one-line hook.

## Stamp

Canonical: `YYYY-MM-DD HH:mm UTC`, at the start of an entry title and at the end of a closed clause.

```
^\d{4}-\d{2}-\d{2} \d{2}:\d{2} UTC\b
```

A closed stamp can be approximate. It shows the order in which the clauses settled, not the exact time.

## Resume order

1. Destination.
2. Requirements bullets still `OPEN`.
3. Open decisions.
4. The newest Done entry.
5. Only the Knowledge bullets the current work touches.

Stop when you know enough. Then continue the work. Do not recap it.

## Retention (reference values)

A harness that trims the file keeps 5 Done entries and folds the rest into a trailing `Archive` section, keeps 20 Knowledge entries, caps an entry title at 48 characters on a word boundary and an entry body at 60 lines. Requirements is never trimmed. An agent does not write to Archive.
