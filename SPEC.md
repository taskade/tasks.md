# TASKS.md specification

Version 1.0 draft. Status: shipped behavior. The reference implementation seeds the Requirements section with its `States:` line since September 2026.

## TL;DR (non-normative)

- One `TASKS.md` per body of work, at the repository root. Nobody renames it, deletes it or makes a second one.
- Up to six sections, in this order: `Destination`, `Open decisions`, `Out of scope`, `Requirements`, `Done`, `Knowledge`.
- The first four sections are the map: current state, edited in place. The last two are the log: dated entries, newest first.
- `Requirements`, `Done` and `Knowledge` stay in the file once they exist.
- `Requirements` has one bullet per clause of the request. Its first child is the `States:` line, copied exactly. Its heading carries a `[n/N]` count.
- Each clause starts `OPEN` and ends `BUILT`, `DEFAULTED` or `DECLARED`, with the evidence or the sentence on the bullet and a closed stamp.
- `BUILT` needs evidence: the artifact for a thing that exists, the observed outcome for a thing that happens. Intent is never evidence.
- A `Done` entry is a `YYYY-MM-DD HH:mm UTC - title` line with `What changed`, `Where it lives` and `Next` bullets.
- `Knowledge` holds one lasting fact per bullet. Update a bullet instead of adding a second one.
- No credential value goes anywhere in the file.

The sections below are the normative text.

This document defines TASKS.md, a file that an agent keeps during a build. The file holds what was asked, what was decided, what was built and what was learned, so that this state outlives a context reset. The words "must" and "can" have their plain meaning. A file conforms when every "must" holds.

## 1. Scope

TASKS.md is a build ledger: the record of one body of work, kept for the agents and people who continue it. It is not a task queue, not a product specification, and not a replacement for version control.

The format is plain Markdown: a fixed set of section headings and a fixed grammar for dated entries. You can read and write it without a parser. A harness can automate parts of it. Every rule below applies with or without that automation.

## 2. Location and identity

- Each body of work has exactly one TASKS.md. In a repository, it is at the root. In a workspace with no file system, it is the single document with this name in the memory area of the agent.
- The file is permanent and unique. An agent must never rename it, delete it or make a second one. It edits the file in place.
- A harness can identify the file by a role instead of by its name, for example a `ledger` role on the document record. Then the role decides identity, and the name is the fallback. A harness that does this must keep one ledger per workspace.
- If the file does not exist, the agent creates it once, with at least the Requirements section and the two log sections, and uses that file from then on.

## 3. Sections

The file is a map and then a log, with up to six sections in this order:

1. `Destination`
2. `Open decisions`
3. `Out of scope`
4. `Requirements`
5. `Done`
6. `Knowledge`

Sections 1 to 4 are the map. The map is current state: edited in place, a few lines per section, and no dated entries. Sections 5 and 6 are the log. `Done` holds dated entries and is append-only. `Knowledge` holds one fact per bullet.

A section is a top-level heading, or a top-level list item, whose text is the section name. Sections keep the order above. A section that the work never needs can be absent. `Requirements`, `Done` and `Knowledge` are in every seeded file, and once present they must not be removed.

Size the map to the work. A small edit changes only the log. Write the map when the work runs longer than one or two turns, or when it is costly to lose sight of the goal. Work with a clear path needs no map.

If the log and the map disagree, the map is right. The map says what is true now. The log says how it became true.

### 3.1 Destination

One or two lines that describe the finished result in the words of the requester. The agent writes it when a build or a multi-step task starts, and changes it only when the requester changes the goal. After an interruption, this line keeps the agent on the result that was asked for, not on a vague summary of it.

When a specification is too large for the map, it goes in its own file. That file holds the requirements of the requester word for word, one top-level node per requirement, with the details nested below it. `Destination` links to that file and never copies the whole specification. On resume, the agent opens the link and reads the top-level nodes first. It expands a part only when it works on that part.

### 3.2 Open decisions

Questions that are still open, one line each. Example: "Who receives the order alert?". When a decision is made, the agent removes its line and records the answer, with the reason, as a `Knowledge` bullet. An empty section means that no work waits on a decision, and that is the normal state.

### 3.3 Out of scope

Items left out on purpose, each with its reason. Example: "No online payments - the shop takes cash at pickup for now". The reason stops a later turn from adding the item back.

### 3.4 Requirements

The clauses that the work owes: one bullet per clause, in the words of the requester, each marked with its state when it settles. The section has no dated entries, is never pruned, and is edited in place for the whole build. Section 4 gives its grammar.

### 3.5 Done

Finished work: one dated entry per milestone or edit, newest first. A harness can also write its own progress summaries here as dated entries. Section 5 gives the entry grammar.

### 3.6 Knowledge

Facts that stay useful: identifiers, decisions with their reasons, what the agent learned about the data model, and anything that took more than one attempt. One fact per bullet. Section 6 gives the upkeep rules.

## 4. The Requirements block

### 4.1 Grammar

```
Requirements [n/N]
  States: OPEN | BUILT(!evidence) DEFAULTED(@the default you chose) DECLARED(@what only the user can supply)
  - <clause in the requester's words>: <STATE>[ - <evidence or sentence>][ - closed <stamp>]
```

- The heading holds the section name and a `[n/N]` count, called the cookie.
- The first child of the section is the `States:` line, copied exactly. It is a plain bullet with no date.
- Each other child is one clause: short, and in the words of the requester.

### 4.2 The States line

The `States:` line puts the vocabulary inside the file. States to the left of the `|` still need work. States to the right of it are final. `!` marks evidence: something the agent observed. `@` marks a sentence that the requester will read.

| State | Meaning |
|---|---|
| `OPEN` | Not settled yet. Every clause starts here, because nothing is built when the plan is made. |
| `BUILT` | It exists and it works. The bullet names the evidence (section 7). |
| `DEFAULTED` | The clause needed a detail that the agent had no way to know. The agent picked a sensible default that is easy to change, built the clause with it, and states the default in one line. |
| `DECLARED` | The agent cannot build it without something that only the requester can provide: a connection to an outside account, a decision that cannot be undone, or data that does not exist yet. The bullet says what is missing, why, and the one thing that will finish the clause. |

A detail that the agent can default is never `DECLARED`. The agent picks the default and builds.

A clause that names a scheduled or event-driven behavior ends as `BUILT` or `DECLARED`. Inline code that only imitates the behavior does not count. Client-side code that mirrors the behavior for instant feedback is welcome as an addition, never as the replacement.

### 4.3 The cookie

The agent computes the `[n/N]` cookie. It never types the numbers by hand. `n` is the number of clauses that are no longer `OPEN`. `N` is the number of all clauses. Each time the agent edits the section, it counts both again from the bullets. It never adds one to the old value. The bullets are the source of truth, and the cookie only summarizes them. No clause is ever removed, so `N` never goes down.

### 4.4 The closed stamp

When a clause leaves `OPEN`, the agent adds ` - closed YYYY-MM-DD HH:mm UTC` to the end of its bullet, in the same form as an entry stamp (section 8). The stamp shows the order in which the clauses settled, not the exact time, so an approximate time is acceptable.

### 4.5 Rules

- Write the clauses during planning, before you build. For a new build, that is after the first plan exists, never before it.
- Every clause starts `OPEN`. A clause changes state only when the agent has evidence that meets the level in section 7. The bullet then records that evidence, not the plan.
- Keep every clause in the section until the build ends. Do not remove a clause because it is finished. If the list shrinks to the work that is left, nobody can see what was asked, and the final check has nothing to compare against.
- Write down the requirement, never the secret. If the request includes a password, a key, a token or another credential, record what it is for and leave out the value. The file is long-lived memory, and people and agents read it long after the secret was needed.
- The clause list lives only in Requirements. Never put a copy of it in a `Done` entry or in any other place. A `Done` entry names the clauses that it settled. The state of each clause lives only on its Requirements bullet.
- When a turn resumes earlier work, the agent opens the file and reads Requirements again before it decides what is left. No mechanism loads the section into the context of the agent automatically.

## 5. Done entries

### 5.1 Grammar

```
YYYY-MM-DD HH:mm UTC - <title>
  - What changed: <the files, records or artifacts, named>
  - Where it lives: <the artifact linked by name>
  - Next: <the one thing to do next>
```

Each milestone or edit gets one dated entry, newest first, with three or four bullets: what changed, where it lives, and what comes next. Keep the title short. A harness can limit it to 48 characters, cut at a word boundary.

An entry is a dated title line with bullets below it. The stamp comes first on the title line. Section headings and plain bullets have no stamp. The stamp is the only signal that separates an entry from a structural line.

Example:

```
2026-08-20 14:12 UTC - Contact form and alert shipped
  - What changed: added pages/Contact.tsx, the Inquiries table and the job "New inquiry alert".
  - Where it lives: [Inquiries](<link>) holds the messages. The alert job reads its Email field.
  - Next: build the monthly gallery refresh.
```

### 5.2 Rules

- Do not write a bare count of steps, such as "Finished 4 of 4 tasks". A count gives the next turn no new information.
- Put each new entry at the top. Do not change older entries.
- Write a `Done` entry after an edit too, not only after a full build.
- A `Done` entry names the clauses that it settled but does not repeat their state. Section 4.5 holds that rule.
- `Where it lives` can hold a lasting reference: a commit, a pull request, a release tag or a named test. Pick one that still resolves after the working tree changes.

Proof appears in two places on purpose. A `Requirements` bullet holds what the agent OBSERVED, and that settles the claim at the moment it is made. A `Done` entry holds a lasting POINTER that a reader can follow months later. Many ledgers in public repositories keep only the pointer, so they show a commit but not what anyone checked. Keep both.

### 5.3 Priority and defects

Priority is a tag on an entry, never a heading. If you sort the file into P0 to P4 sections, those sections replace the six sections and the map is lost. That is the most common way for a real ledger to become unreadable. If priority matters, write the tier on the bullet.

A PRE-EXISTING defect, one that the current work did not cause, is not a decision, not new scope and not excluded scope. So no map section fits it as written. Record it as an `Open decisions` line that names the symptom and the choice it forces, or as a new `Requirements` clause if the work cannot finish without the fix.

A defect that the current work caused falls under section 7.5, which allows no delay: fix it in this turn or declare it. To record your own defect as an open decision is the delay that 7.5 forbids, and 7.5 takes precedence.

In both cases, do not add a permanent defect section. A ledger with a standing bug list has turned into an issue tracker, and that is a different tool.

## 6. Knowledge bullets

- One fact per bullet. If a new learning is about something that a bullet already covers, edit that bullet. Do not add a second one.
- If work shows that a bullet is wrong, fix or delete it in the same turn. A wrong fact costs the next turn more than a missing fact.
- Read each bullet as true at the time it was written, not as live state. If a tool result contradicts a bullet, the tool result is right. Correct the bullet and continue.
- Record what was LEARNED as well as what was done: identifiers that a later turn otherwise looks up again, decisions with their reasons, and anything that took more than one attempt.
- File at least one `Knowledge` entry in every build, not only `Done` entries.
- When a fact grows too large for one bullet, move it to its own file. The bullet becomes a link and one line that says why to open it.

## 7. Evidence

### 7.1 BUILT needs evidence, not intent

Before the agent marks a clause `BUILT`, it names the proof that it holds, at the level that the clause claims. There are two levels, and they are different.

- If a clause claims that something EXISTS, the artifact settles it. For a record, an agent or a job, that is the identifier the system returned. For a page, it is the route that the agent opened and the control that it found on that page. The file that the agent wrote is never enough.
- If a clause claims that something HAPPENS, no artifact settles it. "Each order sends a receipt" and "a new sign-up lands in the Leads table" describe behavior. An identifier or a visible control proves that the parts are connected. It does not prove that they run. The evidence is the outcome that the agent saw: it placed an order and saw the receipt, or it signed up and found the new row.

Most clauses that matter are HAPPENS clauses, because a build can have every visible part in place and still behave wrongly. A clause that the agent only planned, described or intended leaves the same trace in its context as a clause that it built: its own text. Intent meets neither level.

### 7.2 Not built is not DECLARED

A clause with no evidence is not `DECLARED` for that reason alone. `DECLARED` keeps exactly the meaning in section 4.2: the clause waits on something that only the requester can provide. An unbuilt clause is work still to do, and the check exists to find it. The agent builds it and records the evidence. Only when the agent cannot finish it does the clause leave the turn as `DECLARED`. It can never be `BUILT`.

### 7.3 Runs, not wiring

A connected job proves that the job exists. Only a run proves that it works. A written page proves that a file exists. Only a page that the requester can reach proves that the feature works. When the request names a way to put something IN, such as a form, an upload or a request page, the agent finds that input on the running system before it marks the clause. Every other clause can pass while the system still has no way to receive input.

### 7.4 Name a gap before you suggest it

If the closing suggestions say "Add a contact form", then no contact form exists, and the summary cannot say that messages reach anyone. The agent first says, in one plain line, that the item does not exist yet. Then it can suggest it.

### 7.5 A defect in your own work is never BUILT

When the agent sees that something it made is wrong, the item becomes `Fixed` or `DECLARED`. No other outcome exists.

- `Fixed`: the agent corrects it in the same turn, with the same tool that made it.
- `DECLARED`: the agent cannot correct it without something that only the requester can provide. The closing summary names it in one line: the fault, its location, and the effect it will have.

"It works" and "it is correct" are two different claims. When they disagree, "it is correct" wins. A wrong identifier, a wrong data shape, or a plain string where the format expects an object can pass today only because nothing reads it back yet. To record such an item as done, in the ledger or in a record, is to ship the defect without a word.

The agent does not call its output broken when its tools report that it works, and does not call it working when its tools report that it is broken. When the two disagree, it says so in one line and names both.

### 7.6 Words for observed outcomes only

The words **verified**, **confirmed**, **tested** and **complete** are only for outcomes that the agent saw happen. To read the code for a behavior shows that the behavior was WRITTEN. It never shows that it RUNS. When the agent cannot reach the level of evidence, the unchecked part comes first in the summary, in the terms of the requester, before any part that the agent did check. The agent repeats this warning on every turn while the clause stays open, not only on the first turn.

### 7.7 A second report cancels the evidence

When the requester says a second time that something reported as working does not work, the reading of the code by the agent no longer counts as evidence. The requester ran it, and the agent did not. The agent says plainly that its check cannot see what the requester sees, and it changes its approach. It asks for the one detail that tells the possible causes apart, adds a log line or an on-screen marker that makes the behavior visible, or gives the exact steps it uses when it operates the system itself. A repeat of a fix on the same evidence spends the time and money of the requester on a loop that already failed.

## 8. Timestamps

The canonical stamp is `YYYY-MM-DD HH:mm UTC`. It comes first in an entry title, or last on a closed clause:

```
^\d{4}-\d{2}-\d{2} \d{2}:\d{2} UTC\b
```

Agents also write other forms: a timezone token other than UTC, no timezone token, an ISO `T` separator, seconds, or a date with no time. A harness that normalizes stamps can detect such a stamp with the pattern below. The pattern accepts everything that the canonical pattern accepts, and more. It is anchored and not global, so it never changes a date that appears later in a title:

```
^\d{4}-\d{2}-\d{2}(?:[T ]\d{1,2}:\d{2}(?::\d{2})?(?:\s?(?:[A-Z]{1,4}T(?:[+-]\d{1,2}(?::?\d{2})?)?|UTC(?:[+-]\d{1,2}(?::\d{2})?)?|Z|[+-]\d{2}:?\d{2}))?)?(?=$|\s)
```

A harness that replaces such a stamp must replace all of it with the canonical stamp from the server clock. Every writer produces the canonical form: the seeded example entry, each entry that a harness writes, and each corrected title have exactly the same shape.

## 9. Resume

When work resumes, the agent reads the file before it rebuilds any state from scratch. It reads in this order, and it stops when it knows enough:

1. `Destination`.
2. The `Requirements` bullets that are still `OPEN`.
3. `Open decisions`.
4. The newest `Done` entry.
5. Only the `Knowledge` bullets that the current work touches.

Then it continues the work directly. It does not announce that it reads its memory, and it does not recap what was built. It acts on the newest `Next` bullet, unless the latest message of the requester replaced it or asks for something else.

The file informs the turn. It does not direct it. What the agent remembers tells it where the work stopped. The latest message of the requester decides what the turn is for. If that message asks for one specific change, the agent makes that change and stops. It does not pick up deferred plans that the message did not ask for. It offers them as a one-line follow-up. If the requester limits the turn ("only this", "nothing else"), that limit wins over everything in the file for the whole turn. A `Next` bullet that the agent was told to leave alone is still valid. It stays in the ledger for a later turn that asks for it.

## 10. Retention

A harness that keeps the file small applies these limits. The numbers are those of the reference implementation. Another harness can choose other numbers, and must state them.

| Section | Limit | On overflow |
|---|---|---|
| `Done` | 5 entries | Older entries move to a trailing `Archive` section, newest first like every other section. Nothing is deleted. |
| `Knowledge` | 20 entries | The oldest entries are removed. Age does not make a fact false, so the rule "update, do not duplicate" in section 6 is what keeps this section under the limit. |
| Entry title | 48 characters | Cut at a word boundary, never inside a word. |
| Entry body | 60 lines | A harness truncates a longer body when it files the entry. |
| `Requirements` | none | Never pruned. |

`Archive` is a seventh section, after `Knowledge`. It exists only when a harness moves entries into it. An agent does not write to it.

## 11. Conformance

A TASKS.md file conforms to this version when:

- The sections that are present keep the order in section 3.
- `Requirements`, when present, starts with the exact `States:` line and carries a `[n/N]` cookie that matches its bullets.
- Every `Done` entry starts with a canonical stamp and holds no bare step count.
- No `Knowledge` bullet duplicates another.
- No bullet anywhere holds a credential value.

A harness conforms when it keeps one ledger per body of work, never writes the map sections, writes only canonical stamps, and never prunes `Requirements`.

## 12. Versioning

This is version 1.0. A change to the section list, the `States:` line or the stamp grammar is a new major version and gets its own dated directory. A change to prose only is a patch.

## 13. Optional tree projection

The interchange format is the Markdown file in sections 3 to 8. A harness can also keep the same ledger as a tree. The projection is optional. A file that never leaves Markdown still conforms.

- Each section is one node. Each top-level item under a section is a child of that node: a Requirements bullet, or a dated entry in `Done` or `Knowledge`. A dated entry is a node too, and the bullets under its title are its children, not children of the section. Every bullet attaches to its direct parent, so nesting in the file is nesting in the tree. Nothing is copied to get a second shape.
- The state token on a Requirements bullet (`OPEN`, `BUILT`, `DEFAULTED`, `DECLARED`) is a field on that node, parsed from the bullet. A harness can filter, group on a board, or chart by that field. The bullet text stays the source.
- List, outline, board and table are views of one tree, not four copies.
- A person, an agent, an automation and a generated interface all write through one commit path into that tree. No second document exists that can drift.
- YAML or JSON is a projection of the tree, not a second specification. A projection conforms only when it round-trips to the Markdown grammar in this document: the same sections, the same `States:` line, the same stamps, and no credential values.

A harness that stores the tree must still be able to render the Markdown file. Conformance is judged on that file, not on the projection.
