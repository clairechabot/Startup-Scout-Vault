---
company: Hypatia Systems
competitors:
  - - - Archimedes Analytics
  - - - Newton Labs
sector: AI Infrastructure
subsector: Scientific Reasoning Engines
location: Zurich, Switzerland
founded: 2024
founders:
  - - - Hypatia
  - - - Nikola Tesla
stage: Pre-seed
funding: 850k CHF
last_round: Angel + Research grants
investors:
  - Aurora Scientific Angels
  - ETH Innovation Grant
deal_status: Invested
scout_form: true
summary: Building a scientific reasoning engine that automates hypothesis generation and experiment-design workflows for research labs.
investment_thesis: If they can validate the early hypothesis engine internally, the TAM for automated scientific reasoning could be extremely large. Early traction with lab partnerships is promising.
date_created: 2025-11-23
tags:
  - company
  - deal
Institutions:
  - ETH Zurich
  - Curie Institute of Scientific Discovery
---

[[ellipsis dashboard]]

# Deal Overview
> [!NOTE] Quick Actions
> **Status:** `INPUT[inlineSelect(option(Screening), option(Call Scheduled), option(In DD), option(Active), option(Ongoing), option(Invested), option(Paused), option(Passed)):deal_status]`  
> **Scout Form Sent:** `INPUT[toggle:scout_form]`

## Scout Submission
```dataview
TABLE submission_status as "Status", date_created as "Drafted"
FROM "100_Network/Scout Submissions"
WHERE contains(target_company, this.file.link) OR contains(target_company, this.file.name)
```

# 1. Summary

> [!abstract] One-Liner  
> AI infrastructure that generates, simulates, and ranks scientific hypotheses—like a “reasoning copilot” for research labs.

**Thesis / My Take:**  
If validated, this would sit between computational biology and generalist scientific AI. The defensibility hinges on their proprietary simulation engine + feedback loops with real lab data.

---

# 2. Team (Key Priority)

- **Founders:**
    
    - Hypatia: Mathematical theorist + ex-ETH research lead
        
    - Tesla: Built low-latency distributed compute systems for simulation engines
        
- **Key Hires:**
    
    - ML engineer with physics-informed model experience
        
    - Partnerships lead for lab integrations
        
- **Advisors:**
    
    - Dr. Rosalind Franklin (computational biology)
        
    - Prof. Alan Turing (ML systems)
        

---

# 3. Traction & Product

**Metrics**

- Revenue: 0 (pre-revenue)
    
- Growth: Weekly increase in alpha users (currently 14 labs)
    
- Pilots: ETH, Curie Institute, Utrecht Biohub
    

**Product**

- **Core Problem:** Scientists spend weeks manually generating/test-ranking hypotheses.
    
- **Solution:** A reasoning engine that outputs testable hypotheses + experimental plans.
    
- **Moat:** Proprietary simulation layer trained on multi-domain lab archives.
    

---

# 4. Market & Landscape

## Market Size

- TAM: Scientific software + AI infra ~35B+ globally
    
- SOM: SaaS for academic + early-industry labs
    

## Competitor Analysis

```dataview
TABLE stage as "Stage", total_funding as "Funding", deal_status as "Our Status"
FROM "100_Network/Startups"
WHERE contains(this.competitors, file.link)

```

## Cross-References 

```dataview 
TABLE stage as "Stage", total_funding as "Funding"
FROM "100_Network/Startups"
WHERE contains(competitors, this.file.link) OR contains(competitors, this.file.name)

```

---

# 5. Deal Dynamics

- **Total Raised:** ~850k CHF
    
- **Current Round:** Raising 1.5M CHF for pre-seed extension
    
- **Valuation:** 7.5M CHF pre-money
    
- **Key Investors:** ETH Innovation Grant, angels
    

> [!failure] Risks & Red Flags
> 
> - Highly technical product. Long validation cycles
>     
> - Need more enterprise-ready infrastructure
>     

---

# 6. Next Steps

-  [ ] Schedule screening call
    
-  [ ] Request architecture diagram + technical roadmap 📅 2025-11-25 
    
-  [ ] Ask for 3-month pilot results 📅 2026-02-23 
    

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

