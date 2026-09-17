# Reference implementation

This file describes ONE implementation of the format. The specification requires none of it. A `TASKS.md` in a repository is a complete, valid ledger on its own, and nothing here is needed to use the format with any agent or any tool.

It exists because the question "why keep the map, not just a queue" has a concrete answer once a ledger stops being flat text.

Reference implementation: Taskade, which has run this format in production since July 2026.

## One ledger, many views

Reference implementation: Taskade stores a ledger as a tree rather than a document, so the same content answers different questions without being rewritten.

<img src="https://www.taskade.com/static_images/blog/taskade-project-views.gif" alt="One project rendered as a list, a board, a mind map and an org chart" width="640"> <!-- Reference implementation asset -->

The six sections stay as top-level nodes and their entries become children. Nothing is duplicated to get a second view.

## A clause state that can be filtered

The Requirements block gains the most. Each clause becomes an item and its state becomes a typed field, so "what is still OPEN" stops being a reading exercise and becomes a filter.

A state that is a field can also drive something.

<img src="https://www.taskade.com/static_images/newsletter-gifs/2025-02-19-filter-custom-fields-automation.gif" alt="A typed field's allowed values driving an automation filter" width="640"> <!-- Reference implementation asset -->

## Knowledge that is read, not pasted

Reference implementation: Taskade attaches the ledger to an agent as a live source, so what the agent knows tracks what the ledger says. A bullet corrected this turn is corrected for the next question asked.

<img src="https://www.taskade.com/static_images/ai-agent-knowledge-upload-v2.gif" alt="A live project attached to an agent as a knowledge source" width="640"> <!-- Reference implementation asset -->

This is the difference the Knowledge rules assume. A pasted snapshot rots. An attached source does not.

## One write path, not four

A person editing the outline, an agent writing back what it learned, an automation filing a `Done` entry, and a generated interface recording an outcome are not four integrations.

```
   person     ----+
   agent      ----+---->  one commit path  ---->  one tree
   automation ----+
   interface  ----+
```

They are four callers of the same commit function against the same tree. That is why a ledger cannot drift from what an interface displays: there is no second copy to drift from.

## None of this is required

The format is plain markdown and stays useful as plain markdown. Read [SPEC.md](./SPEC.md) for what is actually normative.
