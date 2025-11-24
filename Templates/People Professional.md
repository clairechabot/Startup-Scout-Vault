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
relationship_status: New
Institutions: 
---

[[People MOC]]

# Profile

> [!NOTE] Bio
> _(Copy bio or quick summary here)_

**Details**
- **Role:** `VIEW[{role}]`
- **Location:** `VIEW[{location}]`
- **Sector:** `VIEW[{sector_focus}]`

## Relationship Context
- **Status:** `INPUT[inlineSelect(option(New), option(Warm), option(Strong), option(Cold)):relationship_status]`

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