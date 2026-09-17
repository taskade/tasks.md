# Why a ledger and not a queue

## The file outlives the conversation

An agent works inside a context window. The window resets, gets summarized, or is handed to a different agent. Everything the agent knew about the work that is not written down is gone at that moment. The plan it was following, the clause it had not built yet, the reason it excluded a feature, the identifier it needs for the next step: all of it.

A conversation is lossy. TASKS.md is the lossless spine around it. What is written in the file survives. What is not written does not. That is the whole design constraint, and every rule in the spec follows from it.

## A queue cannot tell you what was asked

The common `tasks.md` is a queue: the list of what to do next, with items removed as they finish. That shape is useful while the work runs, and it fails at the exact moment it is needed most. When the work ends and the agent writes its summary, a queue that shrank to the remaining items cannot say what the request was. The closing pass reads what is left instead of what was asked, and the clause that was dropped is the one clause nothing points back at, because nothing was built for it.

The half-built request is the failure this format exists to catch. The half that was built works, so every visible signal says success. The missing half is a negative the requester discovers only by missing what never arrived: the alert that never came, the rows that were never de-duplicated. A list that keeps every clause for the whole build, with its state written beside it, turns that negative into a line the agent must read before it claims to be done.

## The counter-thesis, answered

One school of thought, held by tools such as Beads and tasksmd, says that done work belongs in the commit log and that a task file must hold only what remains. It is not a theoretical position. Ledgers in the wild state it as policy, including files written by humans specifically for an AI agent to execute: keep the file clean, delete the finished items, read git history if you need them. Version control does record what changed. It does not record what was asked, what was decided against, or what evidence the agent held when it called a clause finished. A diff shows a form was added. It does not show whether a submission ever reached anyone. The ledger holds the layer above the diff: intent, exclusions, and evidence. Both records are worth keeping. Only one of them can check the agent's closing claims.

## Built is a claim about evidence

The most important rule in the spec is the smallest. A clause is `BUILT` when the agent holds the thing that proves it, at the bar the clause claims, and the two bars differ.

A clause that says something EXISTS is settled by the artifact: an identifier came back, a route rendered, a control was found on the page. A clause that says something HAPPENS is settled only by an outcome the agent observed: it submitted the form, it found the row, it saw the run. An artifact proves wiring and says nothing about behavior. Most of the clauses a requester cares about are HAPPENS clauses, because a build can get the behavior wrong while every visible part is present.

The trap is that intent leaves a trace. An agent that planned a clause, described it, and meant to build it has its own words in context, and those words look the same as the words it wrote after building. That is why the format asks for evidence on the bullet and not a state alone. `BUILT - submitted /contact, row landed in Inquiries` can be checked. `BUILT` cannot.

## The file teaches its own vocabulary

The `States:` line is written verbatim as the first child of the Requirements section in every file. That looks redundant until the reader is someone who never saw the agent's instructions: a resumed session under a different prompt, an agent from another vendor, or the requester. The file explains itself to whoever opens it. Structure that carries its own meaning beats an instruction that lives somewhere else.

The same reasoning puts a worked example entry in the seeded log sections. A format the agent can copy is a format it gets right on the first try. A format it has to invent from a description is a format with as many shapes as there are agents.

## A map and a log

The four map sections are live state. They are edited in place and never dated. The two log sections are history. They are dated and appended. Mixing the two is the common failure of memory files: a decision recorded as a dated note is hard to find and easy to contradict, while a plan appended as history goes stale the moment it changes.

When the map and the log disagree, the map wins. The map is what is true now. The log is how it got there.

The map is also optional in proportion to the work. A small edit updates only the log. A build that spans many turns charts its Destination first, because after an interruption that one line is what keeps the agent building the thing that was asked for and not the thing a summary half-remembers.

## Memory is context, never a work order

A ledger that says what to do next is one step from a ledger that decides what the next turn does. The format draws that line on purpose. What the agent recalls says where the work left off. The requester's latest message says what the turn is for. A `Next` bullet is a suggestion the agent offers back, not an instruction it executes, unless the message asks for it. The file serves the person, never the reverse.

## What the format does not claim

The format claims that a file of this shape survives a context reset and lets a closing pass check its claims against the request. It does not claim a measured effect on build quality, cost, or speed. Those are properties of a harness and a model, and they belong in that harness's own results, not in a format specification.
