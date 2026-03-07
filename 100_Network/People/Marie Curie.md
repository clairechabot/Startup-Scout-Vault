---
name: Marie Curie
company: Radium Therapeutics
role: Scientific advisor, clinical expert
title: Chief Scientific Advisor
location: Paris
country: France
sector_focus: oncology, radiology, imaging
stage_focus: Series A, Series B
email: marie.curie@example.com
phone: 
linkedin: https://www.linkedin.com/in/marie-curie-radium
website: https://radium-therapeutics.com
aliases: 
picture: 
tags:
  - people
  - contact
  - expert
date_created: 2025-11-23
relationship_status: New
Institutions: [[Curie Institute of Scientific Discovery]]
last_contact: 2025-09-20
---

[[People MOC]]

# Profile

> [!NOTE] Bio
> Marie is a clinical and scientific expert in oncology and imaging. She advises multiple therapeutic and diagnostics startups on trial design and translational strategy.

**Details**
- **Role:** `VIEW[{role}]`
- **Location:** `VIEW[{location}]`
- **Sector:** `VIEW[{sector_focus}]`

## Relationship Context
- **Status:** `INPUT[inlineSelect(option(New), option(Warm), option(Strong), option(Cold)):relationship_status]`

---

## Notes & Context
**How we met:**
- Panel discussion on AI in oncology at [[Founders Salon. AI & Sciences (Paris 2025)]].

**Track Record / Background:**
- Long track record in translational oncology and imaging.
- Often brought in to pressure-test clinical strategy & endpoints.

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