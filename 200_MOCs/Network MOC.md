---
banner: "![[neon_network.jpg]]"
mapofcontent: yes
title: Network MOC
moc_type: index
banner: "![[neon_network.jpg]]"
banner_y: 0.532
tags: [MOC, network, crm]
---

[[ellipsis dashboard]]
%%Template: [[People Professional]]%%

```ad-quote
_"Your network is your net worth."_

```

---

```meta-bind-button

label: New Contact
icon: "user-plus"
hidden: false
class: ""
tooltip: "Add a new person to the CRM"
id: "new-contact-btn"
style: primary
actions:
  - type: templaterCreateNote
    templateFile: Templates/People Professional.md
    folderPath: 100_Network/Contacts
    fileName: New Contact
    openNote: true

```


<br>

```dataviewjs
// --- NETWORK STATS (EXPANDED) ---
const contacts = dv.pages('"100_Network/Contacts"');
const firms = dv.pages('"100_Network/Firms"');
const institutions = dv.pages('"100_Network/Institutions"');

// Calculate counts based on Contacts folder role (as per original logic)
const founders = contacts.where(p => (p.role ?? "").toString().toLowerCase().includes("founder")).length;
const investors = contacts.where(p => (p.role ?? "").toString().toLowerCase().includes("investor")).length;
const experts = contacts.where(p => (p.role ?? "").toString().toLowerCase().includes("expert")).length;

// New calculations for organizations
const total_firms = firms.length;
const total_institutions = institutions.length;
const total_network = contacts.length + firms.length + institutions.length; // Approximate total entities

// --- RENDER DASHBOARD ---
const html = `
<div style="display: flex; gap: 14px; margin-bottom: 24px; flex-wrap: wrap; align-items: stretch;">

  <div style="flex: 1; min-width: 100px; padding: 15px; border-radius: 12px; border: 1px solid rgba(0, 255, 255, 0.5); background: rgba(0, 255, 255, 0.05); text-align: center; box-shadow: 0 0 10px rgba(0, 255, 255, 0.1);">
    <div style="font-size: 24px; font-weight: 800; color: #00ffff; line-height: 1; margin-bottom: 5px;">${founders}</div>
    <div style="font-size: 10px; text-transform: uppercase; letter-spacing: 1px; opacity: 0.8; color: #fff;">Founders</div>
  </div>

  <div style="flex: 1; min-width: 100px; padding: 15px; border-radius: 12px; border: 1px solid rgba(255, 0, 255, 0.5); background: rgba(255, 0, 255, 0.05); text-align: center; box-shadow: 0 0 10px rgba(255, 0, 255, 0.1);">
    <div style="font-size: 24px; font-weight: 800; color: #ff00ff; line-height: 1; margin-bottom: 5px;">${total_firms}</div>
    <div style="font-size: 10px; text-transform: uppercase; letter-spacing: 1px; opacity: 0.8; color: #fff;">VC Firms</div>
  </div>

  <div style="flex: 1; min-width: 100px; padding: 15px; border-radius: 12px; border: 1px solid rgba(255, 215, 0, 0.5); background: rgba(255, 215, 0, 0.05); text-align: center; box-shadow: 0 0 10px rgba(255, 215, 0, 0.1);">
    <div style="font-size: 24px; font-weight: 800; color: #ffd700; line-height: 1; margin-bottom: 5px;">${total_institutions}</div>
    <div style="font-size: 10px; text-transform: uppercase; letter-spacing: 1px; opacity: 0.8; color: #fff;">Institutions</div>
  </div>

  <div style="flex: 1; min-width: 100px; padding: 15px; border-radius: 12px; border: 1px solid rgba(160, 160, 160, 0.5); background: rgba(160, 160, 160, 0.05); text-align: center; box-shadow: 0 0 10px rgba(160, 160, 160, 0.1);">
    <div style="font-size: 24px; font-weight: 800; color: #a0a0a0; line-height: 1; margin-bottom: 5px;">${total_network}</div>
    <div style="font-size: 10px; text-transform: uppercase; letter-spacing: 1px; opacity: 0.8; color: #fff;">Total Network</div>
  </div>

</div>
`;
dv.paragraph(html);
```


---

# Founders 

```dataview 

TABLE company AS "Startup", sector_focus AS "Sector", last_contact AS "Last Contact"
FROM "100_Network/Contacts"
WHERE contains(lower(role), "founder")
SORT last_contact ASC

```

---

# Investors

```dataview 

TABLE company AS "Firm", stage_focus AS "Stage", last_contact AS "Last Contact"
FROM "100_Network/Contacts"
WHERE contains(lower(role), "investor") OR contains(lower(role), "angel") OR contains(lower(role), "vc")
SORT company ASC

```

---

# Experts & Operators 

```dataview 

TABLE company AS "Org", sector_focus AS "Expertise"
FROM "100_Network/Contacts"
WHERE contains(lower(role), "expert") OR contains(lower(role), "advisor") OR contains(lower(role), "operator")
SORT file.name ASC

```

---

# Events & Conferences 

```dataview
TABLE date AS "Date", location AS "Location", organizer AS "Host"
FROM "100_Network"
WHERE type = "event" OR contains(tags, "event")
SORT date DESC
```

---

# Venture Firms 

```dataview 
TABLE firm_type AS "Type", stage_focus AS "Stage", location AS "HQ" FROM "100_Network/Firms" SORT file.name ASC
```

---

# Institutions & Hubs 

```dataview 

TABLE inst_type AS "Type", focus AS "Focus Area"
FROM "100_Network/Institutions"
SORT file.name ASC

```