<p align="center">
  <img src="./assets/social-preview.png" alt="TASKS.md" width="640">
</p>

<h1 align="center">TASKS.md</h1>

<p align="center">
  <a href="./SPEC.md">Spec</a> ·
  <a href="./EXAMPLES/">Examples</a> ·
  <a href="./skills/tasks-md/assets/TASKS.md.template">Template</a> ·
  <a href="./skills/tasks-md/">Skill</a> ·
  <a href="./PHILOSOPHY.md">Philosophy</a> ·
  <a href="./REFERENCE.md">Reference implementation</a>
</p>

**TASKS.md is an open Markdown format for the build ledger that a coding agent keeps: what was asked, what was decided, and what was built, with evidence.**

```markdown
# TASKS.md

## Requirements [2/3]

  States: OPEN | BUILT(!evidence) DEFAULTED(@the default you chose) DECLARED(@what only the user can supply)
  - Contact form files an inquiry: BUILT - sent a test message from /contact, a new row appeared in Inquiries - closed 2026-08-20 14:09 UTC
  - Email me on each new inquiry: DEFAULTED - @alerts go to your account address for now - closed 2026-08-20 14:12 UTC
  - Monthly gallery refresh: OPEN

## Done

2026-08-20 14:12 UTC - Contact form and alert shipped
  - What changed: added pages/Contact.tsx, the Inquiries table and the job "New inquiry alert".
  - Where it lives: commit 3f2c1a9. The alert job reads the Email field of Inquiries.
  - Next: build the monthly gallery refresh.

## Knowledge

2026-08-20 14:09 UTC - Data decisions
  - Inquiries.email is an email field, so the alert job can read it as an address.
```

The agent filed three clauses before it built. Two are settled, each with its proof. One is still open, in plain sight. The next session, or the next agent, starts here.

## Add it to your repository

Install the skill. Your agent then creates the file and keeps it:

```
npx skills add taskade/tasks.md
```

Or copy the template to the root of your repository:

```
curl -fsSL https://raw.githubusercontent.com/taskade/tasks.md/main/skills/tasks-md/assets/TASKS.md.template -o TASKS.md
```

If your agent does not load skills, add one line to your `AGENTS.md`: `Keep the build ledger in TASKS.md. The format is at https://github.com/taskade/tasks.md`

## Three files, three questions

| File | The question it answers | What it holds |
|---|---|---|
| [AGENTS.md](https://github.com/agentsmd/agents.md) | How do I work in this repository? | Instructions for coding agents: setup, build and test commands, code style, pull request rules. |
| [DESIGN.md](https://github.com/google-labs-code/design.md) | How must the product look? | A visual identity for coding agents: design tokens, and prose that explains how to apply them. |
| TASKS.md | What was asked, what was decided, what was built, and how do we know? | A ledger: each clause of the request, the decisions and exclusions, the evidence, and the lessons. |

The first two files tell an agent what to do. TASKS.md records what the agent did. Each file answers a different question, so a repository can hold all three.

## Why a ledger

- A context window resets. A file does not. What is written in TASKS.md survives a new session, a summary, or a change of agent.
- A person and an agent read the same file, and both can check it.
- A settled clause carries its evidence. "Done" becomes a fact that you can check, not a claim.
- Every clause stays in the list, so the final check compares the result with the whole request, not with what is left.
- A to-do queue drops an item when it is finished. A ledger keeps it, with its proof.

The full argument is in [PHILOSOPHY.md](./PHILOSOPHY.md).

## The shape

Up to six sections, in a fixed order. The first four are the map: current state, edited in place. The last two are the log: dated entries, newest first.

| Section | Kind | Holds |
|---|---|---|
| Destination | map | The finished result, in the words of the requester. One or two lines. |
| Open decisions | map | Questions that are still open, one line each. |
| Out of scope | map | What was left out on purpose, with the reason. |
| Requirements | map | One bullet per clause of the request, with its state. |
| Done | log | One dated entry per milestone or edit. |
| Knowledge | log | Lasting facts: identifiers, decisions, lessons. One fact per bullet. |

The rule that matters most: `BUILT` needs evidence. If a clause claims that something exists, the artifact proves it. If a clause claims that something happens, only the outcome that the agent saw proves it. Intent is never evidence. [SPEC.md](./SPEC.md) gives every rule, and starts with a ten-line summary.

## TASKS.md is an outline

A ledger is headings and nested list items, so it is already a tree. [SPEC.md](./SPEC.md) section 13 defines the tree:

```text
TASKS.md
├── Requirements [2/3]                    section  -> parent node
│   ├── States: OPEN | BUILT ...          legend   -> child node
│   ├── Contact form ...: BUILT - <proof> clause   -> child node, proof in its text
│   └── Monthly gallery refresh: OPEN     clause   -> child node, state as a field
├── Done                                  section  -> parent node
│   └── 2026-08-20 14:12 UTC - <title>    entry    -> child node
│       ├── What changed: ...             detail   -> child of the entry
│       ├── Where it lives: ...           detail   -> child of the entry
│       └── Next: ...                     detail   -> child of the entry
└── Knowledge                             section  -> parent node
    └── 2026-08-20 14:09 UTC - <title>    entry    -> child node
```

Because the state of a clause can become a field, "what is still OPEN" becomes a filter instead of a search.

To see this in [Taskade](https://www.taskade.com), paste a TASKS.md into a project as plain text. Taskade turns the Markdown into rows:

- `#` and `##` headings become heading rows, and the lines below them nest under them.
- `###` to `######` headings become bold rows. Nothing nests under them.
- Each list item becomes a row, and an indented list item nests under the item above it.
- Any other line, such as the `States:` line or the title of a dated entry, becomes a plain row.

The bullets of a dated entry land right after its title, at the same level, because the title is a plain line and not a list item. Leave out the `# TASKS.md` line when you paste, and each section becomes a top-level row. Then switch the project between views such as List, Board and Mind Map. To write Markdown that imports cleanly, use the [`taskade-markdown` skill](https://github.com/taskade/skills/tree/main/plugins/taskade/skills/taskade-markdown).

## Who uses TASKS.md

- [Taskade EVE](https://www.taskade.com/create) keeps a TASKS.md ledger in the workspace of each Taskade Genesis app. Taskade adds the file when Taskade Genesis creates the app workspace, and Taskade EVE updates it while it builds. [REFERENCE.md](./REFERENCE.md) describes this implementation.

Does your tool or project read or write TASKS.md? Open a pull request that adds one line to this list, with a link that shows where your tool uses the file.

## Files

| File | What it is |
|---|---|
| [SPEC.md](./SPEC.md) | The format: sections, entry grammar, the Requirements block, stamps, retention. |
| [EXAMPLES/](./EXAMPLES/) | Three complete ledgers: a new build, a resumed build, a small edit. |
| [skills/tasks-md/](./skills/tasks-md/) | The agent skill, with [the template](./skills/tasks-md/assets/TASKS.md.template) and the evidence reference. |
| [PHILOSOPHY.md](./PHILOSOPHY.md) | Why a ledger and not a queue. |
| [REFERENCE.md](./REFERENCE.md) | One implementation: a ledger as a tree, many views, live agent knowledge. |
| [CONTRIBUTING.md](./CONTRIBUTING.md) | How to propose a change. |
| [GOVERNANCE.md](./GOVERNANCE.md) | Who maintains the spec, and how a second maintainer joins. |

## FAQ

**Does it need Taskade?**
No. The format is plain Markdown, and a `TASKS.md` in a repository is a complete ledger on its own. Taskade is one implementation.

**Which agents can use it?**
Any agent that can read and write a file in the repository. The skill uses the [Agent Skills](https://agentskills.io) format, and agentskills.io lists the agents that load it.

**Is it a TODO file?**
No. A TODO file is a queue, and an item leaves it when it is finished. TASKS.md keeps every clause for the whole build and writes the evidence beside it.

**Is the git log not enough?**
The git log records what changed. It does not record what was asked, what was left out, or what the agent checked before it called a clause finished. Keep both.

## Related formats

- [AGENTS.md](https://agents.md): instructions for coding agents.
- [DESIGN.md](https://github.com/google-labs-code/design.md): a visual identity for coding agents.
- [llms.txt](https://llmstxt.org): what a website wants a language model to read.
- [Agent Skills](https://agentskills.io): the format that the TASKS.md skill uses.

## License

Apache-2.0. See [LICENSE](./LICENSE).

## Taskade

TASKS.md comes from [Taskade](https://www.taskade.com). The format does not need Taskade.

[Build an app from a prompt](https://www.taskade.com/create) · [Taskade App Factory](https://www.taskade.com/apps) · [Agent skills](https://github.com/taskade/skills) · [MCP server](https://github.com/taskade/mcp) · [Awesome Vibe Coding](https://github.com/taskade/awesome-vibe-coding) · [Docs](https://www.taskade.com/docs)
