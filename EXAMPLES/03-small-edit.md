# TASKS.md

## Requirements [0/0]

  States: OPEN | BUILT(!evidence) DEFAULTED(@the default you chose) DECLARED(@what only the user can supply)

## Done

2026-09-02 10:31 UTC - Orders list sorted by pickup time
  - What changed: OrdersPage.tsx sorts orders by the Pickup field, earliest first, before it renders them. /orders now opens with the 08:00 pickup on top and the 17:30 pickup last. No data change.
  - Where it lives: [Orders](<link>) still feeds the page. The sort reads the Pickup time field.
  - Next: nothing owed. The owner mentioned a filter by status. Offered as a follow-up, not built.

2026-09-01 16:05 UTC - Orders page shipped
  - What changed: added pages/OrdersPage.tsx, routed from App.tsx, with a link in ShopNav.
  - Where it lives: [Orders](<link>) feeds it. Pickup is a time field.
  - Next: add the remaining sample orders so the page shows a full day.

## Knowledge

2026-09-01 16:05 UTC - Identifiers and data decisions
  - Table: [Orders](<link>). Pickup is a time field, not text in the row title, so the page can sort by it.
  - Order numbers are unique row titles, and the page uses them as keys.
