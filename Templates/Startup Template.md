---
company:
competitors:
sector:
subsector:
location:
founded:
founders:
stage:
funding:
last_round:
investors:
deal_status: Screening
scout_form: false
summary:
investment_thesis:
date_created: <% tp.date.now("YYYY-MM-DD") %>
tags:
  - company
  - deal
Institutions:
---

[[ellipsis dashboard]]

# Deal Overview
> [!NOTE] Quick Actions
> **Status:** `INPUT[inlineSelect(option(Screening), option(Call Scheduled), option(In DD), option(Active), option(Ongoing), option(Invested), option(Paused), option(Passed)):deal_status]`  |  **Scout Form Sent:** `INPUT[toggle:scout_form]`


## Scout Submission
```dataview
TABLE submission_status as "Status", date_created as "Drafted"
FROM "100_Network/Scout Submissions"
WHERE contains(target_company, this.file.link) OR contains(target_company, this.file.name)
```

<br> 

---

# 1. Summary

> [!abstract] One-Liner _(Paste the one-sentence pitch here)_

**Thesis / My Take:** _Why is this exciting? (or why is it confusing?)_


<br> 

---

# 2. Team (Key Priority)

> _"Bet on the jockey, not the horse."_

- **Founders:** (Backgrounds, Exits, Technical/Sales split)
    
- **Key Hires:**
    
- **Advisors:**
    

<br> 

---

# 3. Traction & Product

**Metrics**

- **Revenue:** (ARR / MRR)
    
- **Growth:** (MoM / YoY)
    
- **Users/Pilots:** **Product**
    
- **Core Problem:**
    
- **Solution:**
    
- **Moat/Defensibility:**
    

<br> 

---

# 4. Market & Landscape

## **Market Size**

- TAM/SAM/SOM:
    

## **Competitor Analysis**

### Direct Competitors 

```dataview

TABLE stage as "Stage", total_funding as "Funding", deal_status as "Our Status"
FROM "100_Network/Startups"
WHERE contains(this.competitors, file.link)

```

### Cross-References 

```dataview

TABLE stage as "Stage", total_funding as "Funding"
FROM "100_Network/Startups"
WHERE contains(competitors, this.file.link) OR contains(competitors, this.file.name)

```

<br> 

---

# 5. Deal Dynamics

- **Total Raised:**
    
- **Current Round:** (Target Amount)
    
- **Valuation:** (Cap / Pre-money)
    
- **Key Investors:**
    

> [!failure] Risks & Red Flags
> - 
> -

<br> 

---

# 6. Next Steps

- [ ]
    

<br> 

---

# Reference Log

## Meetings

```dataview

TABLE startdate as "Date", summary as "Summary"
FROM "100_Network/Meetings"
WHERE contains(company, this.file.link) OR contains(company, this.file.name)
SORT startdate desc

```

## Contacts

```dataview 

TABLE role as "Role", email as "Email"
FROM "100_Network/Contacts"
WHERE contains(company, this.file.link) OR contains(company, this.file.name)

```