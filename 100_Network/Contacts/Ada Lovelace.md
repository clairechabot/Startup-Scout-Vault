---
name: Ada Lovelace
company: Analytical Engines AI
role: Founder & CEO, technical founder
title: Co-founder & CEO
location: London
country: UK
sector_focus: AI infrastructure, dev tools
stage_focus: Pre-seed, Seed
email: ada.lovelace@example.com
phone: 
linkedin: https://www.linkedin.com/in/ada-lovelace-analytical-engines
website: https://analyticalengines.ai
aliases: 
picture: 
tags:
  - people
  - contact
  - founder
date_created: 2025-11-23
relationship_status: New
Institutions: [[Curie Institute of Scientific Discovery]]
last_contact: 2025-10-15
---

[[People MOC]]

# Profile

> [!NOTE] Bio
> Ada is the technical founder of Analytical Engines AI, building tools that translate messy scientific code into scalable AI workflows. Background in mathematics and early computing theory.

**Details**
- **Role:** `VIEW[{role}]`
- **Location:** `VIEW[{location}]`
- **Sector:** `VIEW[{sector_focus}]`

## Relationship Context
- **Status:** `INPUT[inlineSelect(option(New), option(Warm), option(Strong), option(Cold)):relationship_status]`

---

## Notes & Context
**How we met:**
- Met at [[Founders Salon. AI & Sciences (Paris 2025)]], intro via another founder.

**Track Record / Background:**
- Previously worked on numerical methods for scientific computing.
- Known for clear thinking around AI infra for research labs.

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

