---
type: institution
inst_type: Accelerator
focus: 
website: 
location: 
tags:
  - institution
  - hub
date_created: <% tp.date.now("YYYY-MM-DD") %>
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

````

## Alumni & Founders

_People in my network associated with this institution._

```dataview
TABLE role AS "Role", company AS "Current Startup"
FROM "100_Network/Contacts"
WHERE contains(institutions, this.file.name) OR contains(institutions, this.file.link) OR contains(company, this.file.name)
SORT file.name ASC
```

## Spin-outs & Batches

_Startups that came out of here._

```dataview
TABLE deal_status AS "Status", stage AS "Stage", sector AS "Sector"
FROM "100_Network/Startups"
WHERE contains(institutions, this.file.name) OR contains(institutions, this.file.link)
SORT date_created DESC
```

## Notes

- Demo Day dates:
    
- Key Professors/Directors:
    

