---
date_created: <% tp.date.now("YYYY-MM-DD") %>
type: meeting
company:
summary:
status: Scheduled
location:
startdate:
start_time:
meeting_link:
deal:
tags:
  - meeting
---
[[Meetings MOC]]

```ad-note
title: Meeting Status Progression
collapse: closed
Scheduled → Completed | Follow-up Pending | Cancelled | Rescheduled

- **Scheduled** — Confirmed, hasn't happened yet
- **Completed** — Done, notes written
- **Follow-up Pending** — Done, action item required before closing
- **Cancelled** — Did not happen, not rescheduled
- **Rescheduled** — Moved — update startdate

See [[Status Standards]] for the full reference.
```

> [!NOTE] Logistics
> **Company:** `INPUT[text:company]`
> **Status:** `INPUT[inlineSelect(option(Scheduled), option(Completed), option(Follow-up Pending), option(Cancelled), option(Rescheduled)):status]`
> **Date:** `INPUT[datePicker:startdate]`

---


```ad-note
title: Meeting Summary

_Write a short summary about the meeting._

```

<br> 

# Attendees
<br> 

-

<br> 

---

# Agenda
<br> 

-

<br> 

```ad-question

_Questions for the meeting._

```

---

# Notes
<br> 

-

<br> 

---

# To-do 
<br>

- ....
- ...


