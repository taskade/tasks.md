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
  - Gallery and a case-study page per shoot: BUILT - / and /shoots/<slug> both render with the seeded shoots - closed 2026-08-20 14:07 UTC
  - Contact form files an inquiry: BUILT - submitted /contact, row landed in Inquiries - closed 2026-08-20 14:09 UTC
  - Alert me on every new inquiry: BUILT - recipient set to studio@<her-domain>, job "New inquiry alert" shows a run to that address - closed 2026-08-21 09:15 UTC
  - Monthly refresh of the gallery from my cloud album: DECLARED - @the refresh job exists, disabled, and needs your cloud album account connected under Connections; connect it and I will enable the job - closed 2026-08-21 09:40 UTC

## Done

2026-08-21 09:40 UTC - Monthly refresh job created, awaiting the album connection
  - What changed: added the job "Monthly gallery refresh" with a schedule trigger on the 1st, a fetch step against the album, and an upsert into Shoots by album id. Created disabled.
  - Where it lives: [Monthly gallery refresh](<link>). The album connection slot is empty; the job cannot run until it is filled.
  - Next: when the connection exists, enable the job and run it once by hand, then record the run here.

2026-08-21 09:15 UTC - Alert recipient set from her answer
  - What changed: the "New inquiry alert" job now sends to studio@<her-domain>. Removed the Open decisions line.
  - Where it lives: [New inquiry alert](<link>), one new run to the new address after a test submission.
  - Next: the monthly refresh job.

2026-08-20 14:12 UTC - Contact form and inquiry alert shipped
  - What changed: added pages/Contact.tsx, the Inquiries store with name, email, message and date fields, and the job "New inquiry alert" that runs on a new Inquiries row.
  - Where it lives: [Inquiries](<link>) holds the rows; [New inquiry alert](<link>) is enabled and shows one run for the test submission.
  - Next: build the monthly refresh job.

2026-08-20 14:07 UTC - Gallery and case-study pages shipped
  - What changed: added pages/Gallery.tsx and pages/Shoot.tsx, routed from App.tsx, nav in SiteNav.tsx. Seeded six shoots.
  - Where it lives: [Shoots](<link>) feeds both pages; the cover image is the Cover field, the write-up is the Story field.
  - Next: the contact form and the alert.

2026-08-20 13:52 UTC - Session summary
  - Built the gallery, the shoot pages and the first pass of the contact form. The alert job was drafted but not yet enabled.
  - Open: the alert recipient, the monthly refresh.

## Knowledge

2026-08-20 14:09 UTC - Ids and schema decisions
  - Stores: [Shoots](<link>), [Inquiries](<link>). Jobs: [New inquiry alert](<link>), [Monthly gallery refresh](<link>).
  - Shoot date is a date field, not text in the row title, so the gallery can sort by it.
  - Inquiries.email is a text field with the email preset, so the alert job can read it as an address.
  - The alert recipient is studio@<her-domain> (her answer on 2026-08-21; the account-address default is gone).
  - Shoots.album_id is the upsert key for the refresh job. A second row with the same album id was created on the first dry run and deleted; the upsert now matches on that field, not on the title.

2026-08-20 13:40 UTC - Ledger conventions - example entry
  - Durable ids: link each store, agent and job by name so a later turn never re-derives them.
  - Decisions: one line with the reason.
  - Learnings: whatever cost you a retry, written so the next turn does not repeat it.
  - The Requirements section carries a States line and a [n/N] cookie - the file teaches its own vocabulary.
  - One fact per bullet, and update the bullet you already have instead of adding a second one about the same thing.
  - When work proves a bullet here wrong, correct or delete that bullet in the same turn.

## Archive

2026-08-20 13:40 UTC - Ledger ready - example entry, copy this shape
  - [changed] TASKS.md seeded with its Requirements, Done and Knowledge sections, so the next turn reads state instead of re-deriving it.
  - [where] Pinned at the top of this workspace, one dated entry per milestone or edit, newest first.
  - [next] File your own entry here when a build phase or an edit lands, because a context reset keeps only what was written down.
