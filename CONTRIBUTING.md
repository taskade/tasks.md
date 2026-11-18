# Contributing

Thank you for reading the spec closely enough to want to change it.

## How to propose a change

1. Open an issue that states the problem the change solves. A concrete ledger that the current spec handles badly is the strongest form of evidence.
2. Say which part of the spec the change touches: the section list, the `States:` line, the entry grammar, the stamp grammar, the evidence rules, or prose only.
3. If the change alters the section list, the `States:` line or the stamp grammar, it is a new major version. Name a second implementer, besides the reference implementation, who will adopt it.
4. Send a pull request that changes `SPEC.md` and, when the reader needs it, `skills/tasks-md/`. Keep the examples in `EXAMPLES/` in format.

## Add your tool to "Who uses TASKS.md"

If your tool or project reads or writes TASKS.md, open a pull request that adds one line to the list in the README. Link to a page or a file that shows where your tool uses the format.

## Rules for the text

- Plain sentences. Short. One rule per sentence where possible.
- "must" for a rule, "can" for an option. No "should".
- Hyphens, not dashes.
- No claims about efficacy. The spec describes a shape, not a measured effect.
- No vendor names outside the parts of the README about Taskade and the list in "Who uses TASKS.md".

## Where the source lives

This repository is the source of truth for the format. Open a pull request here. The reference implementation follows the text of this repository.

## License

By contributing you agree that your contribution is licensed under the Apache License 2.0, the same license as the spec.
