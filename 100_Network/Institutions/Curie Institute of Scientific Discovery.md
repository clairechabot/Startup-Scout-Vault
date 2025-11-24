---
type: institution
inst_type: Accelerator
focus: AI & scientific innovation, translational research
website: https://curie-discovery.org
location: Paris, France
tags:
  - institution
  - hub
date_created: 2025-11-23
---

[[Network MOC]]


> [!NOTE] Profile
> **Type:** `INPUT[inlineSelect(option(Accelerator), option(University), option(Research Lab), option(Community)):inst_type]`
> **Focus:** `INPUT[text:focus]`
> **Website:** `INPUT[text:website]`

```ad-info
title: Overview
collapse: open
_Key programs, batch dates, or research areas._

- Annual "AI & Sciences" acceleration program for technical founders.
- Strong ties to European universities and hospitals.
- Hosts [[Founders Salon. AI & Sciences (Paris 2025)]]. 
```

## Alumni & Founders

_People in my network associated with this institution._

`TABLE role AS "Role", company AS "Current Startup" FROM "100_Network/Contacts" WHERE contains(institutions, this.file.name) OR contains(institutions, this.file.link) OR contains(company, this.file.name) SORT file.name ASC`

## Spin-outs & Batches

_Startups that came out of here._

`TABLE deal_status AS "Status", stage AS "Stage", sector AS "Sector" FROM "100_Network/Startups" WHERE contains(institutions, this.file.name) OR contains(institutions, this.file.link) SORT date_created DESC`

## Notes

- Demo Day dates:
    
    - Main AI & Sciences batch demo day. Early October each year.
        
- Key Professors/Directors:
    
    - Program Director with oncology + AI background.
        
    - Deep-tech partner in residence from [[Relativity Ventures]].