# TASKS.md

**The build ledger a coding agent keeps.** Agents write a file by this name into repositories constantly, and the ones that do share a filename and nothing else. Every convention around it, from memory banks to spec kits to handoff notes, marks work done with a checkbox or a sentence. None of them requires the agent to hold evidence first. That is what this format adds.

Most files named `tasks.md` are plans: a queue of what an agent must do next. TASKS.md is the other half. It is the ledger an agent keeps of what was asked, what was decided and excluded, what was built with evidence, and what it learned. The next session, or the next agent from any vendor, starts where this one stopped. AGENTS.md says how to work. TASKS.md says what happened.

Reference implementation: [Taskade](https://www.taskade.com) has run this format in production since July 2026. Its builder seeds a ledger into each workspace it creates, and a partial unique index in the database makes a second one impossible rather than merely discouraged. The two halves are separate: the seed puts the file there, the constraint keeps a duplicate from ever being written beside it. The format is described here as it is actually enforced, not as it was imagined. Clone a live kit: [taskade.com/apps/categories](https://www.taskade.com/apps/categories).

A queue that shrinks to the remaining work cannot tell you what was asked. When the work ends, the closing pass has nothing to check its claims against. The ledger keeps the request, the decisions and the evidence in one file that outlives the conversation.

## Ledger, not queue

Both kinds of file are useful. They answer different questions, and only one of them survives the work.

| | A queue | TASKS.md |
|---|---|---|
| Answers | What do I do next | What happened, and how do we know |
| On completion | The item is removed | The clause keeps its evidence |
| After the work | Empty, so it cannot be audited | The whole request, still readable |
| Survives a context reset | Only the remainder | The goal, the exclusions, the proof |
| Who it serves | The agent, mid-task | The next agent, and the human reviewing |

A queue that shrinks to the remaining work cannot tell you what was asked. Keep a queue if you want one. The ledger is what the closing pass checks its claims against.

## The shape

Six sections in a fixed order. The first four are the map: live state, edited in place, never dated. The last two are the log: dated entries, newest first.

| Section | Kind | Holds |
|---|---|---|
| Destination | map | What finished looks like, in the requester's own words. One or two lines. |
| Open decisions | map | What is not yet decided, one line each. Empty is the healthy state. |
| Out of scope | map | What was excluded on purpose, with the reason. |
| Requirements | map | One bullet per clause of the request, with its state annotated in place. |
| Done | log | Completed work, one dated entry per milestone or edit, newest first. |
| Knowledge | log | Durable facts: ids, decisions, learnings. One fact per bullet. |

A small edit updates only the log. Chart the map when the work spans more than a turn or two, or when losing the goal is expensive.

## The Requirements block

Every clause of a request starts OPEN. A clause leaves OPEN only when the agent holds evidence, and the bullet records that evidence.

```
Requirements [3/4]
  States: OPEN | BUILT(!evidence) DEFAULTED(@the default you chose) DECLARED(@what only the user can supply)
  - Landing and detail pages: BUILT - / and /items both render - closed 2026-08-20 14:07 UTC
  - Contact form writes a record: BUILT - submitted /contact, row landed in Inquiries - closed 2026-08-20 14:09 UTC
  - Notify me on every new record: BUILT - job "New inquiry alert" shows a run for that submission - closed 2026-08-20 14:12 UTC
  - Monthly refresh job: OPEN
```

The block teaches its own vocabulary. A reader with none of the agent's instructions can still read it right.

## The one rule

Built is a claim about evidence, not about intent. A clause that says something EXISTS is settled by the artifact. A clause that says something HAPPENS is settled only by the outcome the agent observed. A clause with nothing behind it is unfinished work. It is never Built, and it is not thereby Declared.

## Install the reader

```
npx skills add taskade/tasks.md
```

The skill teaches an agent to create the file when it is missing, to file the clauses before it builds, to record evidence rather than intent, and to resume from the file instead of from memory. It works in any agent that reads the Agent Skills format.

## Files

| File | What it is |
|---|---|
| [SPEC.md](./SPEC.md) | The format: sections, entry grammar, the Requirements block, stamps, retention. |
| [PHILOSOPHY.md](./PHILOSOPHY.md) | Why a ledger and not a queue, and the counter-thesis answered. |
| [EXAMPLES/](./EXAMPLES/) | Three ledgers: a new build, a resumed build, a small edit. |
| [skills/tasks-md/](./skills/tasks-md/) | The reader skill, with the template and the evidence reference. |
| [REFERENCE.md](./REFERENCE.md) | One implementation, shown: a ledger as a tree, many views, live agent knowledge. |
| [CONTRIBUTING.md](./CONTRIBUTING.md) | How to propose a change. |
| [GOVERNANCE.md](./GOVERNANCE.md) | Who maintains the spec and how a second maintainer joins. |

## Using it with a workspace

A ledger stays a file and stays useful as one. A harness may also keep that same file as a tree of nodes, with the requirement state as a field, and render it as a list, a board, or a table. YAML is a projection of that tree, not a second format. See [SPEC.md](./SPEC.md) section 13 and [REFERENCE.md](./REFERENCE.md).

## Related formats

- [AGENTS.md](https://agents.md) - how to work in a repository.
- [llms.txt](https://llmstxt.org) - what a site wants a model to read.
- [Agent Skills](https://agentskills.io) - the format the reader skill uses.

TASKS.md sits beside them. It is the only one of the four that records what happened.

## License

Apache-2.0. See [LICENSE](./LICENSE).
