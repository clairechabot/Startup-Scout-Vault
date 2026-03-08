---
title: Status Standards
tags: [admin, reference, standards]
date_created: 2026-03-08
---

[[ellipsis dashboard]]

> [!important] Single Source of Truth
> This document defines every status enum used in the vault. **All templates and queries must use exactly these values** (case-sensitive). When adding a new status, update this document first.

---

# deal_status

Used in: `Templates/Startup Template.md` | Queried in: `Startup MOC`, `ellipsis dashboard`

**Field:** `deal_status`

```
Screening → Call Scheduled → In DD → Active → Ongoing → Invested
                                              └→ Paused
                                              └→ Passed
```

| Value | Meaning |
|---|---|
| `Screening` | First pass — reviewing deck, website, basic diligence |
| `Call Scheduled` | Intro or follow-up call booked |
| `In DD` | Active due diligence in progress |
| `Active` | Ongoing engagement, not yet committed |
| `Ongoing` | Deep process — term sheet or LOI stage |
| `Invested` | Capital deployed |
| `Paused` | Temporarily on hold — revisit later |
| `Passed` | Decision made not to proceed |

**Dataview filter examples:**
```dataview
WHERE contains(lower(deal_status), "in dd")
WHERE deal_status = "Invested"
WHERE deal_status = "Passed" OR deal_status = "Paused"
```

> [!warning] Deprecated values
> Do not use: `Ongoing` and `Active` are distinct. `Ongoing` = near-term commitment stage. `Active` = engaged but pre-commitment.

---

# deal_temperature

Used in: `Templates/Startup Template.md` | Queried in: `Startup MOC`

**Field:** `deal_temperature`

| Value | Meaning |
|---|---|
| `Hot` | Immediate opportunity — time-sensitive, act this week |
| `Warm` | Strong fit, timing not yet confirmed |
| `Cold` | Monitor — not ready or not a priority right now |

> [!note] Note
> Temperature is independent of `deal_status`. A deal can be `Screening` (early status) but `Hot` (urgent). Use temperature to sort your attention, not to replace the pipeline stage.

---

# submission_status

Used in: `Templates/Scout Submission Form Template.md` | Queried in: `Scout Submissions MOC`, `Startup MOC`

**Field:** `submission_status`

```
Draft → In Progress → Submitted → Reviewed → Accepted
                                          └→ Rejected
                                          └→ Withdrawn
```

| Value | Meaning |
|---|---|
| `Draft` | Skeleton created, not yet being written |
| `In Progress` | Actively being written — key sections still incomplete |
| `Submitted` | Sent to Airtable form |
| `Reviewed` | Partner team has seen it |
| `Accepted` | Deal moved into partner pipeline |
| `Rejected` | Passed by the fund |
| `Withdrawn` | Scout pulled the submission (founder asked, or thesis mismatch) |

> [!warning] Deprecated values
> Do not use: `Ready` — replaced by `In Progress`.

---

# meeting_status

Used in: `Templates/Meeting Template.md`, `Templates/First Meeting Template.md` | Queried in: `Meetings MOC`, `ellipsis dashboard`

**Field:** `status`

```
Scheduled → Completed
         → Follow-up Pending
         → Cancelled
         → Rescheduled
```

| Value | Meaning |
|---|---|
| `Scheduled` | Confirmed in calendar, hasn't happened yet |
| `Completed` | Meeting done, notes written |
| `Follow-up Pending` | Done, but a specific follow-up action is required before closing |
| `Cancelled` | Did not happen, not rescheduled |
| `Rescheduled` | Moved — update `startdate` and keep this status until it happens |

> [!warning] Deprecated values
> Do not use: `planned`, `done`. Both replaced above.

---

# relationship_status

Used in: `Templates/People Professional.md` | Queried in: `People MOC`, `Network MOC`, `Startup Template`

**Field:** `relationship_status`

```
Prospect → Introduced → Active → Dormant
                               └→ Archived
```

| Value | Meaning |
|---|---|
| `Prospect` | On the radar — not yet met or formally connected |
| `Introduced` | Have met or exchanged messages — relationship is warming |
| `Active` | Regular engagement, strong mutual trust |
| `Dormant` | Was active, has gone quiet — worth a nudge |
| `Archived` | Relationship ended or no longer relevant |

> [!warning] Deprecated values
> Do not use: `New`, `Warm`, `Strong`, `Cold` — all replaced above.

---

# Quick Reference Card

| Field | Options |
|---|---|
| `deal_status` | Screening, Call Scheduled, In DD, Active, Ongoing, Invested, Paused, Passed |
| `deal_temperature` | Hot, Warm, Cold |
| `submission_status` | Draft, In Progress, Submitted, Reviewed, Accepted, Rejected, Withdrawn |
| `meeting_status` (field: `status`) | Scheduled, Completed, Follow-up Pending, Cancelled, Rescheduled |
| `relationship_status` | Prospect, Introduced, Active, Dormant, Archived |
