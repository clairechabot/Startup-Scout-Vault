---
name: Albert Einstein
company: Relativity Ventures
role: General Partner, deep-tech investor
title: General Partner
location: Zurich
country: Switzerland
sector_focus: AI, quantum tech, advanced materials
stage_focus: Pre-seed, Seed, Series A
email: albert.einstein@example.com
phone: 
linkedin: https://www.linkedin.com/in/albert-einstein-relativity
website: https://relativity.vc
aliases: 
picture: 
tags:
  - people
  - contact
  - investor
  - vc
date_created: 2025-11-23
relationship_status: Warm
Institutions: [[Curie Institute of Scientific Discovery]]
last_contact: 2025-11-01
---

[[People MOC]]

# Profile

> [!NOTE] Bio
> Albert is a GP at Relativity Ventures, a Zurich-based deep-tech fund backing AI and physics-driven companies at pre-seed and seed.

**Details**
- **Role:** `VIEW[{role}]`
- **Location:** `VIEW[{location}]`
- **Sector:** `VIEW[{sector_focus}]`

## Relationship Context
- **Status:** `INPUT[inlineSelect(option(New), option(Warm), option(Strong), option(Cold)):relationship_status]`

---

## Notes & Context
**How we met:**
- First conversation at [[Founders Salon. AI & Sciences (Paris 2025)]], then follow-up call on AI infra & scientific tooling.

**Track Record / Background:**
- Ex-physicist turned investor.
- Focus on AI for scientific discovery, simulation, and compute efficiency.

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

