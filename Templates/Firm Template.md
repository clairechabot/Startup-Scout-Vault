---
type: firm
firm_type: VC
stage_focus: Seed
website: 
location: 
tags:
  - firm
  - investor
date_created: <% tp.date.now("YYYY-MM-DD") %>
---

[[Network MOC]]


> [!NOTE] Firm Profile
> **Type:** `INPUT[inlineSelect(option(VC), option(CVC), option(PE), option(Family Office), option(Angel Group)):firm_type]`
> **Stage:** `INPUT[inlineSelect(option(Pre-Seed), option(Seed), option(Series A), option(Growth), option(Generalist)):stage_focus]`
> **Website:** `INPUT[text:website]`

```ad-abstract
title: Investment Thesis
collapse: open
_What do they invest in? (e.g. "B2B SaaS in Europe", "Deep Tech AI")_
```

## Key Contacts

_People in my network who work here._

```dataview
TABLE role AS "Role", last_contact AS "Last Spoken"
FROM "100_Network/Contacts"
WHERE contains(company, this.file.name) OR contains(company, this.file.link)
SORT role ASC
```

## Portfolio / Co-Investments

_Startups in my vault that they have invested in._


```dataview
TABLE deal_status AS "Status", stage AS "Stage"
FROM "100_Network/Startups"
WHERE contains(investors, this.file.name) OR contains(investors, this.file.link)
SORT date_created DESC
```

## Notes & Intel

- Check size range:
    
- Leading or Following?