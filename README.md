# Greatness Tracker

Cruz Productions Greatness Tracker, rebuilt from `Greatness_Tracker_2026.xls`.

Live page: https://claude.ai/artifact/W1ab9jAWBWPB7EjDUxwcjD (source: `greatness-tracker.html`)

## Categories and weekly goals

| Key | Category | Weekly goal |
|---|---|---|
| f2f | Face to Face | 15 |
| bread | Break Bread | 5 |
| contacts | GREAT Contacts (referrals, referral partners, current & past clients) | 50 |
| events | Events | 2 |
| hours | Hours Prospected | 21 |
| thanks | Thank You Calls/Cards | 10 |
| referrals | Referrals | 10 |
| shows | Shows Booked | 5 |

## Data (for Duet)

One document per week in the artifact database, collection `weeks`, id = that week's Monday (`YYYY-MM-DD`):

```json
{
  "weekOf": "2026-11-23",
  "days": { "mon": { "f2f": ["Jane Doe"], "bread": [], "contacts": [], "events": [], "hours": [], "thanks": [], "referrals": [], "shows": [] }, "tue": {}, "wed": {}, "thu": {}, "fri": {} },
  "notes": "",
  "recorded": false,
  "recordedAt": "ISO timestamp",
  "recordedScore": 0,
  "recordedTotals": { "f2f": 1 }
}
```

Every cell is a list of names. **Each name counts as 1 point**, so a category's total is the number of names logged Mon–Fri.

**Weekly score** = average over the 8 categories of `min(total / goal, 1)`, × 100, rounded.

**Friday recording:** sum each category over Mon–Fri, compute the score, then `update` the week's doc with
`recorded: true`, `recordedAt`, `recordedScore`, `recordedTotals`, `recordedBy: "duet"`.
