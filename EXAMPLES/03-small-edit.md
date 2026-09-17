# TASKS.md

## Requirements [1/1]

  States: OPEN | BUILT(!evidence) DEFAULTED(@the default you chose) DECLARED(@what only the user can supply)
  - Sort the heroes table by win rate, highest first: BUILT - /heroes renders with the top row at 71% and the last at 38% - closed 2026-07-31 15:02 UTC

## Done

2026-07-31 15:02 UTC - Heroes table sorted by win rate
  - What changed: HeroesPage.tsx sorts rows by the Win Rate field descending before render. No schema change.
  - Where it lives: [Match History](<link>) still feeds the page; the sort reads the Win Rate number field.
  - Next: nothing owed. She mentioned a filter by role; offered as a follow-up, not built.

2026-07-31 14:07 UTC - Heroes page shipped
  - What changed: added pages/HeroesPage.tsx, routed from App.tsx, nav link in TrackerNav.
  - Where it lives: [Match History](<link>) feeds it; win rate is the Win Rate number field.
  - Next: seed 12 more heroes so the comfort-pick ranking has enough rows.

## Knowledge

2026-07-31 14:07 UTC - Ids and schema decisions
  - Store: [Match History](<link>). Win Rate is a number field (0 to 100), not a percentage string, so the page can sort on it.
  - Hero names are unique row titles; the page keys on them.
