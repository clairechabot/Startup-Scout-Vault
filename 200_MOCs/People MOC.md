---
mapofcontent: yes
title: People MOC
moc_type: index
tags: [MOC, people, crm]
---

[[ellipsis dashboard]]

```ad-quote
_"Strong relationships compound. Every warm connection is a future deal."_
```

```meta-bind-button
label: New Person
icon: "user-plus"
hidden: false
class: ""
tooltip: "Add a new person or org to the network"
id: "new-person-btn"
style: primary
actions:
  - type: templaterCreateNote
    templateFile: Templates/People Professional.md
    folderPath: 100_Network/People
    fileName: New Person
    openNote: true
```

<br>

```dataviewjs
// --- PEOPLE STATS ---
const people = dv.pages('"100_Network/People"');

const founders   = people.where(p => (p.role ?? "").toLowerCase().includes("founder")).length;
const investors  = people.where(p => (p.role ?? "").toLowerCase().includes("investor") || (p.role ?? "").toLowerCase().includes("vc") || (p.role ?? "").toLowerCase().includes("angel")).length;
const advisors   = people.where(p => (p.role ?? "").toLowerCase().includes("advisor") || (p.role ?? "").toLowerCase().includes("expert") || (p.role ?? "").toLowerCase().includes("operator")).length;
const warm       = people.where(p => p.relationship_status === "Warm" || p.relationship_status === "Strong").length;

const html = `
<div style="display: flex; gap: 14px; margin-bottom: 24px; flex-wrap: wrap; align-items: stretch;">

  <div style="flex: 1; min-width: 120px; padding: 20px; border-radius: 12px; border: 1px solid rgba(0, 255, 255, 0.5); background: rgba(0, 255, 255, 0.05); text-align: center; box-shadow: 0 0 10px rgba(0, 255, 255, 0.1);">
    <div style="font-size: 28px; font-weight: 800; color: #00ffff; line-height: 1; margin-bottom: 8px;">${people.length}</div>
    <div style="font-size: 11px; text-transform: uppercase; letter-spacing: 2px; opacity: 0.8; color: #fff;">Total People</div>
  </div>

  <div style="flex: 1; min-width: 120px; padding: 20px; border-radius: 12px; border: 1px solid rgba(255, 0, 255, 0.5); background: rgba(255, 0, 255, 0.05); text-align: center; box-shadow: 0 0 10px rgba(255, 0, 255, 0.1);">
    <div style="font-size: 28px; font-weight: 800; color: #ff00ff; line-height: 1; margin-bottom: 8px;">${founders}</div>
    <div style="font-size: 11px; text-transform: uppercase; letter-spacing: 2px; opacity: 0.8; color: #fff;">Founders</div>
  </div>

  <div style="flex: 1; min-width: 120px; padding: 20px; border-radius: 12px; border: 1px solid rgba(255, 215, 0, 0.5); background: rgba(255, 215, 0, 0.05); text-align: center; box-shadow: 0 0 10px rgba(255, 215, 0, 0.1);">
    <div style="font-size: 28px; font-weight: 800; color: #ffd700; line-height: 1; margin-bottom: 8px;">${investors}</div>
    <div style="font-size: 11px; text-transform: uppercase; letter-spacing: 2px; opacity: 0.8; color: #fff;">Investors / VCs</div>
  </div>

  <div style="flex: 1; min-width: 120px; padding: 20px; border-radius: 12px; border: 1px solid rgba(0, 255, 120, 0.5); background: rgba(0, 255, 120, 0.05); text-align: center; box-shadow: 0 0 10px rgba(0, 255, 120, 0.1);">
    <div style="font-size: 28px; font-weight: 800; color: #00ff78; line-height: 1; margin-bottom: 8px;">${warm}</div>
    <div style="font-size: 11px; text-transform: uppercase; letter-spacing: 2px; opacity: 0.8; color: #fff;">Warm / Strong</div>
  </div>

</div>
`;
dv.paragraph(html);
```

---

# Relationship Heat Map

_Prioritise the warm ones. Cold relationships decay — schedule a touch._

## Strong

```dataview
TABLE role AS "Role", company AS "Org", sector_focus AS "Thesis Fit"
FROM "100_Network/People"
WHERE relationship_status = "Strong"
SORT file.mtime DESC
```

## Warm

```dataview
TABLE role AS "Role", company AS "Org", sector_focus AS "Thesis Fit"
FROM "100_Network/People"
WHERE relationship_status = "Warm"
SORT file.mtime DESC
```

## New (needs nurturing)

```dataview
TABLE role AS "Role", company AS "Org", date_created AS "Added"
FROM "100_Network/People"
WHERE relationship_status = "New" OR !relationship_status
SORT date_created DESC
LIMIT 20
```

## Cold (dormant)

```dataview
TABLE role AS "Role", company AS "Org"
FROM "100_Network/People"
WHERE relationship_status = "Cold"
SORT file.mtime ASC
LIMIT 10
```

---

# Browse by Role

## Founders

```dataview
TABLE company AS "Startup", sector_focus AS "Sector", relationship_status AS "Status"
FROM "100_Network/People"
WHERE contains(lower(role), "founder") OR contains(lower(role), "ceo") OR contains(lower(role), "cto")
SORT relationship_status ASC
```

## Investors & VCs

```dataview
TABLE company AS "Firm", stage_focus AS "Stage", sector_focus AS "Focus", relationship_status AS "Status"
FROM "100_Network/People"
WHERE contains(lower(role), "investor") OR contains(lower(role), "vc") OR contains(lower(role), "partner") OR contains(lower(role), "angel")
SORT relationship_status ASC
```

## Advisors & Experts

```dataview
TABLE company AS "Org", sector_focus AS "Expertise", relationship_status AS "Status"
FROM "100_Network/People"
WHERE contains(lower(role), "advisor") OR contains(lower(role), "expert") OR contains(lower(role), "operator")
SORT file.name ASC
```

---

# Thesis Relevance

_People whose sector focus matches the Ellipsis thesis (agentic AI, infrastructure, deep tech)._

```dataviewjs
const people = dv.pages('"100_Network/People"');
const thesisKeywords = ["ai", "ml", "infra", "infrastructure", "biotech", "security", "edge", "data", "agentic", "saas"];

const relevant = people.where(p => {
    const sector = (p.sector_focus ?? "").toLowerCase();
    return thesisKeywords.some(k => sector.includes(k));
});

dv.table(
    ["Name", "Role", "Sector Focus", "Status"],
    relevant.sort(p => p.relationship_status).map(p => [
        p.file.link,
        p.role ?? "-",
        p.sector_focus ?? "-",
        p.relationship_status ?? "New"
    ])
);
```

---

# Recent Activity

_People notes touched in the last 30 days._

```dataview
TABLE role AS "Role", company AS "Org", relationship_status AS "Status"
FROM "100_Network/People"
WHERE file.mtime >= date(today) - dur(30 days)
SORT file.mtime DESC
LIMIT 15
```
