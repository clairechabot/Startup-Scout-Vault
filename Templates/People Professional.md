---
name: 
company: 
role: 
title: 
location: 
country: 
sector_focus: 
stage_focus: 
email: 
phone: 
linkedin: 
website: 
aliases: 
picture: 
tags:
  - people
  - contact
date_created: <% tp.date.now("YYYY-MM-DD") %>
relationship_status: Prospect
---

[[People MOC]]

```ad-note
title: Relationship Status Progression
collapse: closed
Prospect → Introduced → Active → Dormant | Archived

- **Prospect** — On radar, not yet met
- **Introduced** — Have connected, relationship warming
- **Active** — Regular engagement, strong trust
- **Dormant** — Gone quiet, worth a nudge
- **Archived** — No longer relevant

See [[Status Standards]] for the full reference.
```

# Profile

> [!NOTE] Bio
> _(Copy bio or quick summary here)_

**Details**
- **Role:** `VIEW[{role}]`
- **Location:** `VIEW[{location}]`
- **Sector:** `VIEW[{sector_focus}]`

## Relationship Context
- **Status:** `INPUT[inlineSelect(option(Prospect), option(Introduced), option(Active), option(Dormant), option(Archived)):relationship_status]`

---

## Notes & Context
**How we met:**
- 

**Track Record / Background:**
- 

---

# Activity Log

## Shared Meetings

```dataview
TABLE startdate as "Date", summary AS "Summary"
FROM "100_Network/Meetings"
WHERE contains(attendees, this.file.link) OR contains(attendees, this.file.name)
SORT startdate desc
```

---

# Published Research 

```dataview
TABLE year AS "Year", title AS "Title", related_startup AS "Startup"
FROM "500_Resources/Research/Paper Notes"
WHERE contains(related_people, this.file.link) OR contains(related_people, this.file.name)
SORT year DESC
```