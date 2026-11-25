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
  "days": { "mon": { "f2f": ["Jane Doe"], "bread": [], "contacts": [], "events": [], "hours": 2, "thanks": [], "referrals": [], "shows": [] }, "tue": {}, "wed": {}, "thu": {}, "fri": {} },
  "notes": "",
  "recorded": false,
  "recordedAt": "ISO timestamp",
  "recordedScore": 0,
  "recordedTotals": { "f2f": 1 }
}
```

Every cell is a list of names. **Each name counts as 1 point**, so a category's total is the number of names logged Mon–Fri. The exception is `hours` (Hours Prospected), which is a number of hours per day (half hours allowed) and is summed.

**Weekly score** = average over the 8 categories of `min(total / goal, 1)`, × 100, rounded.

**Friday recording:** sum each category over Mon–Fri, compute the score, then `update` the week's doc with
`recorded: true`, `recordedAt`, `recordedScore`, `recordedTotals`, `recordedBy: "duet"`.

## Calendar review

The page reads the day's Google Calendar appointments (`list_events`) and prompts for each one:
Face to Face, Break Bread, GREAT Contact, Event, or Doesn't count. A choice adds the name to that day's list and is kept in
`reviewed: { <calendar event id>: { cat, name, day, title } }` so it isn't asked again (`cat: "skip"` = doesn't count).
It then suggests Hours Prospected from the length of the counted appointments; confirming writes the day's `hours`
and sets `hoursReviewed: { <day>: true }`.

A weekday reminder at 4:47 PM Pacific prompts the review.

## Weekly CSV export (Step 2 after recording)

Once a week is recorded, the page shows **Save CSV to Dropbox**. It writes
`/Andy Cruz/Cruz Productions Inc/Greatness_Tracker_Results/Greatness_Tracker_Week_<monday>.csv`
(`_v2`, `_v3`… if that week was already saved) and stores `exported: { path, at, score }` on the week.

CSV columns: `Week Of, Row Type, Day, Date, Category, Entry, Points, Goal, Calendar Appointment`.
Row types: `Entry` (each name, or the day's hours), `Not Counted` (calendar appointments marked Doesn't count),
`Category Total`, `Weekly Score`, `Notes`.
