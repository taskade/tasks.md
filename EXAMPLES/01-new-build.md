# TASKS.md

## Destination

A portfolio site for a freelance photographer: a gallery, a case-study page per shoot, a contact form that files an inquiry and alerts her, and a monthly refresh that pulls the newest shoots in.

## Open decisions

- Which address gets the inquiry alert? (defaulted to the account address for now, see Requirements)

## Out of scope

- No online booking or payments - she wants the site live before the season starts.
- No client login - galleries are public.

## Requirements [3/4]

  States: OPEN | BUILT(!evidence) DEFAULTED(@the default you chose) DECLARED(@what only the user can supply)
  - Gallery and a case-study page per shoot: BUILT - / and /shoots/<slug> both render with the seeded shoots - closed 2026-08-20 14:07 UTC
  - Contact form files an inquiry: BUILT - submitted /contact, row landed in Inquiries - closed 2026-08-20 14:09 UTC
  - Alert me on every new inquiry: DEFAULTED - @the alert goes to your account address; change the recipient in the job "New inquiry alert" - closed 2026-08-20 14:12 UTC
  - Monthly refresh of the gallery: OPEN

## Done

2026-08-20 14:12 UTC - Contact form and inquiry alert shipped
  - What changed: added pages/Contact.tsx, the Inquiries store with name, email, message and date fields, and the job "New inquiry alert" that runs on a new Inquiries row.
  - Where it lives: [Inquiries](<link>) holds the rows; [New inquiry alert](<link>) is enabled and shows one run for the test submission.
  - Next: build the monthly refresh job (Requirements, last bullet) - a schedule trigger that re-reads [Shoots](<link>) and rebuilds the gallery index.

2026-08-20 14:07 UTC - Gallery and case-study pages shipped
  - What changed: added pages/Gallery.tsx and pages/Shoot.tsx, routed from App.tsx, nav in SiteNav.tsx. Seeded six shoots.
  - Where it lives: [Shoots](<link>) feeds both pages; the cover image is the Cover field, the write-up is the Story field.
  - Next: the contact form and the alert.

2026-08-20 13:40 UTC - Ledger ready - example entry, copy this shape
  - [changed] TASKS.md seeded with its Requirements, Done and Knowledge sections, so the next turn reads state instead of re-deriving it.
  - [where] Pinned at the top of this workspace, one dated entry per milestone or edit, newest first.
  - [next] File your own entry here when a build phase or an edit lands, because a context reset keeps only what was written down.

## Knowledge

2026-08-20 14:09 UTC - Ids and schema decisions
  - Stores: [Shoots](<link>), [Inquiries](<link>). Job: [New inquiry alert](<link>).
  - Shoot date is a date field, not text in the row title, so the gallery can sort by it.
  - Inquiries.email is a text field with the email preset, so the alert job can read it as an address.
  - The alert recipient is the account address until she names one (Open decisions).

2026-08-20 13:40 UTC - Ledger conventions - example entry
  - Durable ids: link each store, agent and job by name so a later turn never re-derives them.
  - Decisions: one line with the reason.
  - Learnings: whatever cost you a retry, written so the next turn does not repeat it.
  - The Requirements section carries a States line and a [n/N] cookie - the file teaches its own vocabulary.
  - One fact per bullet, and update the bullet you already have instead of adding a second one about the same thing.
  - When work proves a bullet here wrong, correct or delete that bullet in the same turn.
