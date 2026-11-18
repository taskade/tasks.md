# TASKS.md

## Destination

A portfolio site for a freelance photographer: a gallery, a case-study page per shoot, a contact form that files an inquiry and alerts her, and a monthly refresh that pulls the newest shoots in from her cloud album.

## Open decisions

(none)

## Out of scope

- No online booking or payments - she wants the site live before the season starts.
- No client login - galleries are public.
- No image editing in the site - she edits in her own tools and uploads finished files.

## Requirements [4/4]

  States: OPEN | BUILT(!evidence) DEFAULTED(@the default you chose) DECLARED(@what only the user can supply)
  - Gallery and a case-study page per shoot: BUILT - opened / and /shoots/<slug>, both show the seeded shoots - closed 2026-08-20 14:07 UTC
  - Contact form files an inquiry: BUILT - sent a test message from /contact, a new row appeared in Inquiries - closed 2026-08-20 14:09 UTC
  - Alert me on every new inquiry: BUILT - recipient set to studio@<her-domain>, the job "New inquiry alert" shows a run to that address - closed 2026-08-21 09:15 UTC
  - Monthly refresh of the gallery from my cloud album: DECLARED - @the refresh job exists but is off, because it needs your cloud album account connected under Connections. Connect it and I will turn the job on - closed 2026-08-21 09:40 UTC

## Done

2026-08-21 09:40 UTC - Refresh job created, waiting for the album
  - What changed: added the job "Monthly gallery refresh": a schedule on the 1st of each month, a step that fetches the album, and an upsert into Shoots by album id. Created switched off.
  - Where it lives: [Monthly gallery refresh](<link>). The album connection is empty, and the job cannot run until it is filled.
  - Next: when the connection exists, turn the job on, run it once by hand, and record the run here.

2026-08-21 09:15 UTC - Alert recipient set from her answer
  - What changed: the job "New inquiry alert" now sends to studio@<her-domain>. Removed the Open decisions line.
  - Where it lives: [New inquiry alert](<link>), one new run to the new address after a test message.
  - Next: the monthly refresh job.

2026-08-20 14:12 UTC - Contact form and inquiry alert shipped
  - What changed: added pages/Contact.tsx, the Inquiries table with name, email, message and date fields, and the job "New inquiry alert" that starts on each new Inquiries row.
  - Where it lives: [Inquiries](<link>) holds the messages. [New inquiry alert](<link>) is on and shows one run for the test message.
  - Next: build the monthly refresh job.

2026-08-20 14:07 UTC - Gallery and case-study pages shipped
  - What changed: added pages/Gallery.tsx and pages/Shoot.tsx, routed from App.tsx, with links in SiteNav.tsx. Added six sample shoots.
  - Where it lives: [Shoots](<link>) feeds both pages. The Cover field holds the image and the Story field holds the text.
  - Next: the contact form and the alert.

2026-08-20 13:52 UTC - Session summary
  - Built the gallery, the shoot pages and a first version of the contact form. The alert job existed but was not on yet.
  - Open: the alert recipient, the monthly refresh.

## Knowledge

2026-08-20 14:09 UTC - Identifiers and data decisions
  - Tables: [Shoots](<link>), [Inquiries](<link>). Jobs: [New inquiry alert](<link>), [Monthly gallery refresh](<link>).
  - The shoot date is a date field, not text in the row title, so the gallery can sort by it.
  - Inquiries.email is an email field, so the alert job can read it as an address.
  - The alert recipient is studio@<her-domain> (her answer on 2026-08-21. The account-address default no longer applies.).
  - Shoots.album_id is the upsert key for the refresh job. The first dry run created a duplicate row with the same album id, and that row was deleted. The upsert now matches on album_id, not on the title.

2026-08-20 13:40 UTC - How to use this section - sample entry
  - Identifiers: link each record, agent and job by name, so a later session does not look them up again.
  - Decisions: one line each, with the reason.
  - Lessons: anything that took more than one attempt, so the next session gets it right the first time.
  - One fact per bullet. If a fact changes, edit its bullet. Do not add a second bullet about the same thing.

## Archive

2026-08-20 13:40 UTC - Ledger created - sample entry, follow this form
  - What changed: created TASKS.md with Requirements, Done and Knowledge, so a later session reads the state here and does not work it out again.
  - Where it lives: TASKS.md at the repository root. One dated entry per milestone or edit, newest entry on top.
  - Next: add an entry here each time a build step or an edit lands. After a context reset, only what is written here remains.
