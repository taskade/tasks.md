# Reference implementation

This file describes ONE implementation of the format: [Taskade](https://www.taskade.com). Nothing here is normative. A `TASKS.md` in a repository is a complete, valid ledger on its own, and you need none of this to use the format with any agent or tool. [SPEC.md](./SPEC.md) is the normative text.

In Taskade, Taskade EVE keeps a TASKS.md ledger in the workspace of each Taskade Genesis app. Taskade adds the file when Taskade Genesis creates the app workspace, and Taskade EVE updates it while it builds.

The question "why keep the map, and not only a queue" has a concrete answer once a ledger is more than flat text.

## One ledger, many views

Taskade stores a ledger as a project. A project is a tree, not a flat document, so the same content answers different questions and nobody rewrites it.

<img src="https://www.taskade.com/static_images/blog/taskade-project-views.gif" alt="One project rendered as a list, a board, a mind map and an org chart" width="640"> <!-- Reference implementation asset -->

The six sections stay top-level nodes, and their entries become children. No view needs a copy of the content.

Note (non-normative): you can try this with any TASKS.md. Paste the file into a Taskade project as plain text. The section [TASKS.md is an outline](./README.md#tasksmd-is-an-outline) in the README lists how each kind of Markdown line becomes a row.

## A clause state that you can filter

The Requirements block gains the most. Each clause is an item, and its state can become a typed field. Then "what is still OPEN" is a filter, not a reading exercise.

A state that is a field can also drive an action.

<img src="https://www.taskade.com/static_images/newsletter-gifs/2025-02-19-filter-custom-fields-automation.gif" alt="A typed field's allowed values driving an automation filter" width="640"> <!-- Reference implementation asset -->

## Knowledge that is read, not pasted

Taskade EVE keeps its memory, the ledger included, as projects in the workspace. You can attach a project to an agent as a live knowledge source. Then what the agent knows follows what the ledger says. A bullet corrected in this turn is correct for the next question. See [Long-Term Memory](https://www.taskade.com/docs/guides/long-term-memory).

<img src="https://www.taskade.com/static_images/ai-agent-knowledge-upload-v2.gif" alt="A live project attached to an agent as a knowledge source" width="640"> <!-- Reference implementation asset -->

The Knowledge rules assume this difference. A pasted copy goes stale. An attached source stays current.

## One write path, not four

A person who edits the outline, an agent that writes back what it learned, an automation that files a `Done` entry, and a generated interface that records an outcome are not four integrations.

```
   person     ----+
   agent      ----+---->  one write path  ---->  one tree
   automation ----+
   interface  ----+
```

All four write to the same tree in the same way. So a ledger cannot drift from what an interface shows: no second copy exists that can drift.

## None of this is required

The format is plain Markdown and stays useful as plain Markdown. Read [SPEC.md](./SPEC.md) for what is normative.
