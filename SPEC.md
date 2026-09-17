# TASKS.md specification

Version 1.0 draft. Status: the Requirements section is specified here ahead of the reference implementation seeding it; everything else is shipped behavior.

This document defines the TASKS.md format: one file that an agent keeps while it builds, so that what was asked, what was decided, what was built and what was learned survive a context reset. The words "must" and "can" carry their plain meaning. A file conforms when every "must" holds.

## 1. Scope

TASKS.md is a build ledger. It records the state of one body of work for the agents and people who continue it. It is not a task queue, not a specification of the product, and not a substitute for version control.

The format is plain Markdown with a fixed set of section headings and a fixed grammar for dated entries. No parser is required to read or write it. A harness can add automation on top of the format. The rules below hold with or without that automation.

## 2. Location and identity

- There is exactly one TASKS.md per body of work. In a repository the file lives at the repository root. In a workspace that has no file system, the file is the one document with this name in the agent's memory area.
- The file is a permanent singleton. An agent must never rename it, delete it, or create a second one. It updates the file in place.
- A harness can mark the file by role rather than by name, for example with a `ledger` role on the document record. When it does, the role is the identity and the name is the fallback. A harness that does this must hold one ledger per workspace.
- When the file is missing, the agent creates it once, with at least the Requirements section and the two log sections, and reuses it from then on.

## 3. Sections

The file is a map followed by a log, up to six sections in this order:

1. `Destination`
2. `Open decisions`
3. `Out of scope`
4. `Requirements`
5. `Done`
6. `Knowledge`

The first four sections are the map: live state, edited in place, a handful of lines each, never dated entries. The last two are the log: `Done` is dated and append-only, `Knowledge` is one fact per bullet.

Each section is a top-level heading or a top-level list item whose text is the section name. Sections keep the order above. A section that the work never needs can be absent. `Requirements`, `Done` and `Knowledge` are present in every seeded file and must not be removed once present.

Scope the map to the work. A small edit updates only the log. Chart the map when the work spans more than a turn or two, or when losing the goal is expensive. If the agent already knows the way, it does not need a map.

When the log and the map disagree, the map wins. The map is the current truth, and the log is how it got there.

### 3.1 Destination

One or two lines: what finished looks like, in the requester's own words. The agent charts it when a build or multi-step effort begins and rewrites it only when the requester changes the goal. After an interruption this line is what keeps the agent building the thing that was asked for, not the thing a summary half-remembers.

A specification too big for the map gets its own file. The requester's requirements go into that file verbatim, one top-level node per requirement with its breakdown nested underneath. `Destination` holds the link to that file, never the whole specification. On resume, the agent follows the link and reads the top-level nodes first. It unfolds a section only when it works in it.

### 3.2 Open decisions

What is not yet decided, one line each. Example: "Which staff get the alert?". When a decision resolves, the agent deletes its line and files the answer as a `Knowledge` bullet with the reason. Empty is the healthy state.

### 3.3 Out of scope

What was excluded on purpose, with the reason. Example: "No payments - the requester wants launch first". The reason is what stops a later turn from building the item back in.

### 3.4 Requirements

The clauses the work owes, one bullet per clause, in the requester's own words, each annotated in place as it lands. This section is never dated, never pruned, and edited in place for the whole build. Section 4 defines its grammar.

### 3.5 Done

Completed work, one dated entry per milestone or edit, newest first. A harness can file its own progress summaries here as dated entries. Section 5 defines the entry grammar.

### 3.6 Knowledge

Durable facts: identifiers, decisions with their reason, schema learnings, and whatever cost a retry. One fact per bullet. Section 6 defines the maintenance rules.

## 4. The Requirements block

### 4.1 Grammar

```
Requirements [n/N]
  States: OPEN | BUILT(!evidence) DEFAULTED(@the default you chose) DECLARED(@what only the user can supply)
  - <clause in the requester's words>: <STATE>[ - <evidence or sentence>][ - closed <stamp>]
```

- The heading carries the section name and a `[n/N]` cookie.
- The first child of the section is the `States:` line, written verbatim. It is a plain undated bullet.
- Every other child is one clause, kept short, in the requester's own words.

### 4.2 The States line

The `States:` line declares the vocabulary. States left of the `|` still need work. States right of it are terminal. `!` marks evidence: a thing the agent observed. `@` marks a sentence the requester will read.

| State | Meaning |
|---|---|
| `OPEN` | Not yet settled. Every clause starts here, because at plan time nothing is built. |
| `BUILT` | It exists and works. The bullet names the evidence (section 7). |
| `DEFAULTED` | The clause needed a detail the agent cannot know, so it chose a sensible reversible default, built the clause with it, and named the default in one line. |
| `DECLARED` | The agent cannot build it, because it needs something only the requester can supply: an external account connection, an irreversible decision, data that does not exist yet. The bullet names what is missing, why, and the one thing needed to finish it. |

A detail the agent was able to default is never `DECLARED`. The agent defaults it and builds.

A clause whose words name a scheduled or event-driven behavior lands as `BUILT` or `DECLARED`, never as inline code that imitates the behavior. Client-side logic that mirrors it for instant feedback is welcome in addition, never as the substitute.

### 4.3 The cookie

The `[n/N]` cookie in the heading is derived, never typed. `n` counts the clauses that have left `OPEN`. `N` counts all clauses. The agent recounts both from the bullets every time it touches the section. It never increments. The bullets are the truth and the cookie only summarizes them. Because no clause is ever removed, `N` never decreases.

### 4.4 The closed stamp

When a clause leaves `OPEN`, the agent appends ` - closed YYYY-MM-DD HH:mm UTC` to its bullet, the same shape as an entry stamp (section 8). The stamp records the order in which clauses settled, not wall time, so an approximate value is acceptable.

### 4.5 Rules

- File the clauses at plan time, before building. On a new build that means after the opening plan is written, never ahead of it.
- Every clause starts `OPEN`. A clause changes state only when the agent holds the evidence that settles it at the bar in section 7, and the bullet records that evidence, not the intention.
- Keep every clause in the section for the whole build. Never drop one because it is finished. A list that shrinks to the remaining work cannot tell you what was asked, and the closing pass has nothing to check its claims against.
- Record the requirement, never the credential. If the request carries a password, key, token or any other secret, write what it is FOR and leave the value out. The section is durable memory, read back long after the turn that needed the secret is gone.
- The Requirements section is the one place the clause list lives. Never copy it into a `Done` entry and never keep a second list elsewhere. A `Done` entry names the clauses it settled, by name. Their state lives only on the Requirements bullet.
- On a resumed turn, the agent opens the file and re-reads the Requirements section before it decides what is left to do. Nothing replays the section into its context for it.

## 5. Done entries

### 5.1 Grammar

```
YYYY-MM-DD HH:mm UTC - <title>
  - What changed: <the files, records or artifacts, named>
  - Where it lives: <the artifact linked by name>
  - Next: <the one thing to do next>
```

One dated entry per milestone or edit, newest first, three or four bullets: what changed, where it lives, what is next. The title is short. A harness can cap it at 48 characters on a word boundary.

An entry is a dated title with bullets underneath. The stamp is the first thing on the title line. A section heading or a plain bullet carries no stamp. That is the only signal that tells an entry apart from a structural line.

Example:

```
2026-07-31 14:07 UTC - Heroes page shipped
  - What changed: added pages/HeroesPage.tsx, routed from App.tsx, nav link in TrackerNav.
  - Where it lives: [Match History](<link>) feeds it; win rate is the Win Rate number field.
  - Next: seed 12 more heroes so the comfort-pick ranking has enough rows.
```

### 5.2 Rules

- Never write a bare step count such as "Build completed: 3 steps done". It tells the next turn nothing it did not already know.
- Append the newest entry on top and leave the older ones alone.
- File a `Done` entry after an edit turn too, not only after a full build.
- A `Done` entry names the clauses it settled. It does not carry their state. Section 4.5 holds that rule.
- An entry may carry a durable reference in `Where it lives`: a commit, a pull request, a release tag, or a named test. Prefer one that still resolves after the working tree has moved on.

The division of labor between the two places proof appears is deliberate. A `Requirements` clause records the evidence the agent OBSERVED, which is what settles the claim at the moment it is made. A `Done` entry records a durable POINTER, which is what a reader follows months later. Files in the wild converge on the second and have no place for the first, so they can show you a commit but not what was checked. Keep both.

### 5.3 Priority and defects

Priority is a tag on an entry, never a heading. Reorganizing the file by tier replaces the six sections with P0 through P4 and loses the map, which is the most common way a ledger in the wild stops being readable. Write the tier inline on the bullet if it matters.

A PRE-EXISTING defect, one the current work did not cause, is not a decision to make, not new scope, and not excluded scope, so it belongs in none of the map sections as written. File it as an `Open decisions` line naming the symptom and the choice it forces, or as a new `Requirements` clause if it must be fixed to finish.

A defect the current work introduced is governed by section 7.5 instead, which admits no deferral: fix it this turn or declare it. Filing your own breakage as an open decision is the deferral 7.5 exists to forbid, and 7.5 takes precedence.

Either way, do not open a standing defect section. A ledger with a permanent bug list has become a tracker, and a tracker is a different tool.

## 6. Knowledge bullets

- One fact per bullet. When a new learning is about something a bullet already covers, update that bullet instead of adding a second one.
- When work proves a bullet wrong, correct or delete it in the same turn. A stale fact costs the next turn more than a missing one.
- Treat what is written as what was true when it was written, not as live state. If it disagrees with what a tool just returned, the tool is right. Fix the bullet and move on.
- Write back what was LEARNED, not only what was done: identifiers that a later turn will otherwise re-derive, decisions with their reason, and whatever cost a retry.
- File at least one `Knowledge` entry during every build, not only `Done` entries.
- A fact that outgrows one bullet becomes its own file. The bullet shrinks to a link plus a one-line hook.

## 7. Evidence

### 7.1 Built is a claim about evidence, not about intent

Before a clause goes down as `BUILT`, the agent names the thing it holds that proves it, at the bar the clause claims. The two bars are not the same.

- A clause that says something EXISTS is settled by the artifact: the identifier returned for a record, an agent or a job, or for a page the route the agent opened and the control it found there. The file it wrote is never enough.
- A clause that says something HAPPENS is not settled by an artifact. "The form creates a lead", "the upload files a row", "a new lead notifies me" describe behavior. An identifier or a rendered control proves the wiring and says nothing about whether it runs. The evidence is the outcome the agent observed: it submitted the form, it found the row, it saw the run.

Most clauses that matter are HAPPENS clauses, because a build can get the behavior wrong while every visible part is present. A clause the agent planned, described or meant to build leaves the same trace in its context as one it built: its own words. Intent is the one kind of evidence that settles neither bar.

### 7.2 Not thereby Declared

A clause with nothing behind it is not thereby `DECLARED`. `DECLARED` keeps exactly the meaning in section 4.2: blocked on something only the requester can supply. A clause the agent has not built yet is unfinished work, and finding it is the whole point of the check. The agent builds it and records the evidence. Only when it cannot finish does the clause leave the turn as `DECLARED`. What it can never be is `BUILT`.

### 7.3 The run bar

Wiring proves the agent built a job. Only runs prove it works. Writing a page proves a file exists. Only a surface the requester can reach proves the feature does. When the request names a way for someone to put something IN (a form, an upload, a request page) the agent finds that input on the running system before it claims the clause. A build can satisfy every other clause and still leave no way in.

### 7.4 A gap you are about to suggest is a gap you name first

If the closing suggestions include "Add contact form", then the contact form is not built, and the summary cannot say submissions reach anyone. The agent states in one plain line that the item is not there yet, and only then suggests it.

### 7.5 A defect you found in your own work is never Built

The moment the agent notices that something it produced is wrong, the item lands as `Fixed` or `DECLARED`. There is no third state.

- `Fixed`: correct it in the same turn, with the same tool that created it.
- `DECLARED`: the agent cannot correct it, because it needs something only the requester can supply. Name it in the closing summary in one line: what is wrong, where it is, and what it will break.

"It works" and "it is correct" are different claims. When they disagree, the second one wins. A wrong identifier, a wrong shape, or a field written as a bare string where the format needs an object passes today because nothing has read it back yet. Writing such an item down as done in the ledger or in a record is shipping it silently.

The agent never describes its output as broken when its tools say it works, and never as working when its tools say it is broken. When the two conflict, it says so in one line and names both.

### 7.6 Banned words

**verified**, **confirmed**, **tested** and **complete** belong to outcomes the agent watched happen. Reading the code that implements a behavior is evidence the behavior was WRITTEN, never evidence it RUNS. When the bar is out of reach, the unverified part goes in the first line of the summary, in the requester's terms, before anything the agent did manage to check. The caveat is owed on every turn the clause stays open, not only on the turn the agent first noticed.

### 7.7 The second report voids the evidence

When the requester reports a second time that a thing reported as working does not work, the agent's reading of the code is finished as evidence. They ran it and the agent did not. The agent says plainly that its check cannot see what they see, and changes what it does: it asks for the one detail that separates the candidates, adds the log line or on-screen marker that makes the behavior observable, or hands over the exact steps it takes when it drives the system itself. Repeating a fix on the same evidence spends the requester's time and money on a loop that already failed.

## 8. Timestamps

The canonical stamp is `YYYY-MM-DD HH:mm UTC`, anchored at the start of an entry title or at the end of a closed clause:

```
^\d{4}-\d{2}-\d{2} \d{2}:\d{2} UTC\b
```

Agents write variant stamps: a bare or non-UTC timezone token, a missing token, an ISO `T` separator, trailing seconds, or a date with no time. A harness that normalizes stamps can recognize an attempt with this tolerant pattern, a strict superset of the canonical one, anchored and non-global so a date later in a title is never touched:

```
^\d{4}-\d{2}-\d{2}(?:[T ]\d{1,2}:\d{2}(?::\d{2})?(?:\s?(?:[A-Z]{1,4}T(?:[+-]\d{1,2}(?::?\d{2})?)?|UTC(?:[+-]\d{1,2}(?::\d{2})?)?|Z|[+-]\d{2}:?\d{2}))?)?(?=$|\s)
```

A harness that replaces an attempt must replace the whole attempt with the canonical server stamp. The canonical form is what every writer produces: the seeded example entry, every harness-written entry, and every corrected title carry a byte-identical shape.

## 9. Resume

On resume, the agent reads the file first instead of re-deriving state from scratch, in this order, and stops when it has enough:

1. `Destination`.
2. The `Requirements` bullets still `OPEN`.
3. `Open decisions`.
4. The newest `Done` entry.
5. Only the `Knowledge` bullets the current work touches.

Then it resumes without ceremony: no "checking my memory", no recap of what was built. It acts on the newest `Next` bullet unless the requester's latest message made it obsolete or asks for something else.

Memory is context, never a work order. What the agent recalls says where the work left off. The requester's latest message decides what the turn is FOR. When that message asks for a specific change, the agent delivers that change and stops. It does not resume deferred plans the message did not ask for. It offers them back as a one-line follow-up. When the requester scopes the turn explicitly ("only do this", "nothing else"), that instruction outranks everything recalled for the whole turn. A `Next` bullet the agent was told not to act on is not obsolete. It stays in the ledger for a turn that asks for it.

## 10. Retention

A harness that keeps the file small applies these caps. The numbers are the reference implementation's. Another harness can choose others, and must state them.

| Section | Cap | On overflow |
|---|---|---|
| `Done` | 5 entries | The oldest entries move to a trailing `Archive` section, which stays newest first like every other section. Nothing is destroyed. |
| `Knowledge` | 20 entries | The oldest entries are pruned. A fact does not become false by being old, so update-not-duplicate (section 6) is what keeps this section under the cap. |
| Entry title | 48 characters | Cut on a word boundary, never mid-word. |
| Entry body | 60 lines | Longer bodies are truncated when filed by a harness. |
| `Requirements` | none | Never pruned. |

`Archive` is a seventh section, after `Knowledge`, that exists only when a harness folds entries into it. An agent does not write to it.

## 11. Conformance

A TASKS.md file conforms to this version when:

- The sections present keep the order in section 3.
- `Requirements`, when present, starts with the verbatim `States:` line and carries a `[n/N]` cookie that matches its bullets.
- Every `Done` entry starts with a canonical stamp and holds no bare step count.
- No `Knowledge` bullet duplicates another.
- No bullet anywhere holds a credential value.

A harness conforms when it holds one ledger per body of work, never writes the map sections, writes only canonical stamps, and never prunes `Requirements`.

## 12. Versioning

This is version 1.0. A change to the section list, the `States:` line or the stamp grammar is a new major version and gets its own dated directory. A change to prose only is a patch.
