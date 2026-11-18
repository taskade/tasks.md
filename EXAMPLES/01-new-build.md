# TASKS.md

## Destination

A portfolio site for a freelance photographer: a gallery, a case-study page per shoot, a contact form that files an inquiry and alerts her, and a monthly refresh that pulls the newest shoots in.

## Open decisions

- Which address receives the inquiry alert? (the account address for now, see Requirements)

## Out of scope

- No online booking or payments - she wants the site live before the season starts.
- No client login - galleries are public.

## Requirements [3/4]

  States: OPEN | BUILT(!evidence) DEFAULTED(@the default you chose) DECLARED(@what only the user can supply)
  - Gallery and a case-study page per shoot: BUILT - opened / and /shoots/<slug>, both show the seeded shoots - closed 2026-08-20 14:07 UTC
  - Contact form files an inquiry: BUILT - sent a test message from /contact, a new row appeared in Inquiries - closed 2026-08-20 14:09 UTC
  - Alert me on every new inquiry: DEFAULTED - @the alert goes to your account address. Change the recipient in the job "New inquiry alert" - closed 2026-08-20 14:12 UTC
  - Monthly refresh of the gallery: OPEN

## Done

2026-08-20 14:12 UTC - Contact form and inquiry alert shipped
  - What changed: added pages/Contact.tsx, the Inquiries table with name, email, message and date fields, and the job "New inquiry alert" that starts on each new Inquiries row.
  - Where it lives: [Inquiries](<link>) holds the messages. [New inquiry alert](<link>) is on and shows one run for the test message.
  - Next: build the monthly refresh (the last Requirements bullet) as a scheduled job that reads [Shoots](<link>) again and rebuilds the gallery index.

2026-08-20 14:07 UTC - Gallery and case-study pages shipped
  - What changed: added pages/Gallery.tsx and pages/Shoot.tsx, routed from App.tsx, with links in SiteNav.tsx. Added six sample shoots.
  - Where it lives: [Shoots](<link>) feeds both pages. The Cover field holds the image and the Story field holds the text.
  - Next: the contact form and the alert.

2026-08-20 13:40 UTC - Ledger created - sample entry, follow this form
  - What changed: created TASKS.md with Requirements, Done and Knowledge, so a later session reads the state here and does not work it out again.
  - Where it lives: TASKS.md at the repository root. One dated entry per milestone or edit, newest entry on top.
  - Next: add an entry here each time a build step or an edit lands. After a context reset, only what is written here remains.

## Knowledge

2026-08-20 14:09 UTC - Identifiers and data decisions
  - Tables: [Shoots](<link>), [Inquiries](<link>). Job: [New inquiry alert](<link>).
  - The shoot date is a date field, not text in the row title, so the gallery can sort by it.
  - Inquiries.email is an email field, so the alert job can read it as an address.
  - The alert goes to the account address until she names another one (see Open decisions).

2026-08-20 13:40 UTC - How to use this section - sample entry
  - Identifiers: link each record, agent and job by name, so a later session does not look them up again.
  - Decisions: one line each, with the reason.
  - Lessons: anything that took more than one attempt, so the next session gets it right the first time.
  - One fact per bullet. If a fact changes, edit its bullet. Do not add a second bullet about the same thing.
