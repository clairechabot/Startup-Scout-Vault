---
mapofcontent: yes
title: Deals MOC
moc_type: index
tags: [MOC, deals, startups]
---

[[ellipsis dashboard]]

```ad-quote
_"Collect, compare, decide — then follow up ruthlessly."_
```

```meta-bind-button
label: New Startup
icon: "plus"
hidden: false
class: ""
tooltip: "Create a new startup entry"
id: "new-startup-btn"
style: primary
actions:
  - type: templaterCreateNote
    templateFile: Templates/Startup Template.md
    folderPath: 100_Network/Startups
    fileName: New Startup
    openNote: true
```

<br> 

```dataviewjs

// --- DATA COLLECTION ---
const pages = dv.pages('"100_Network/Startups"');
const by = s => pages.where(p => (p.deal_status ?? "").toLowerCase().includes(s));

// Counts
const ongoing = by("ongoing").length + by("in dd").length + by("active").length + by("call").length;
const invested = by("invested").length;
const paused = by("paused").length;
const archived = by("archiv").length + by("passed").length;
const recent = pages.where(p => p.file.mtime.ts >= (Date.now() - 1000*60*60*24*30)).length;

// --- RENDER DASHBOARD (Clean HTML) ---
const html = `
<div style="display: flex; gap: 14px; margin-bottom: 24px; flex-wrap: wrap; align-items: stretch;">

  <div style="flex: 1; min-width: 120px; padding: 20px; border-radius: 12px; border: 1px solid rgba(0, 255, 255, 0.5); background: rgba(0, 255, 255, 0.05); text-align: center; box-shadow: 0 0 10px rgba(0, 255, 255, 0.1);">
    <div style="font-size: 28px; font-weight: 800; color: #00ffff; line-height: 1; margin-bottom: 8px;">${ongoing}</div>
    <div style="font-size: 11px; text-transform: uppercase; letter-spacing: 2px; opacity: 0.8; color: #fff;">Ongoing</div>
  </div>

  <div style="flex: 1; min-width: 120px; padding: 20px; border-radius: 12px; border: 1px solid rgba(255, 0, 255, 0.5); background: rgba(255, 0, 255, 0.05); text-align: center; box-shadow: 0 0 10px rgba(255, 0, 255, 0.1);">
    <div style="font-size: 28px; font-weight: 800; color: #ff00ff; line-height: 1; margin-bottom: 8px;">${invested}</div>
    <div style="font-size: 11px; text-transform: uppercase; letter-spacing: 2px; opacity: 0.8; color: #fff;">Invested</div>
  </div>

  <div style="flex: 1; min-width: 120px; padding: 20px; border-radius: 12px; border: 1px solid rgba(255, 215, 0, 0.5); background: rgba(255, 215, 0, 0.05); text-align: center;">
    <div style="font-size: 28px; font-weight: 800; color: #ffd700; line-height: 1; margin-bottom: 8px;">${paused}</div>
    <div style="font-size: 11px; text-transform: uppercase; letter-spacing: 2px; opacity: 0.8; color: #fff;">Paused</div>
  </div>

  <div style="flex: 1; min-width: 120px; padding: 20px; border-radius: 12px; border: 1px solid rgba(160, 160, 160, 0.5); background: rgba(160, 160, 160, 0.05); text-align: center;">
    <div style="font-size: 28px; font-weight: 800; color: #a0a0a0; line-height: 1; margin-bottom: 8px;">${archived}</div>
    <div style="font-size: 11px; text-transform: uppercase; letter-spacing: 2px; opacity: 0.8; color: #fff;">Archived</div>
  </div>

</div>

<div style="padding: 12px 16px; border-radius: 8px; border-left: 4px solid #00ffff; background: linear-gradient(90deg, rgba(0,255,255,0.1) 0%, transparent 100%); font-size: 13px; color: #ddd; display: flex; align-items: center;">
  <span style="font-size: 16px; margin-right: 10px;">⚡</span> 
  <span><b>Momentum:</b> <span style="color: #00ffff;">${recent}</span> startups updated in the last 30 days.</span>
</div>
`;

dv.paragraph(html);

```

---

# Master Pipeline

```dataviewjs
// --- CONFIGURATION ---
const startupFolder = '"100_Network/Startups"';
const meetingFolder = '"100_Network/Meetings"';

// Fetch pages
const startups = dv.pages(startupFolder).where(p => p.file.name !== "Startup Template");
const meetings = dv.pages(meetingFolder);

// Create the table rows
const rows = startups.map(s => {
    
    // 1. Find meetings related to this company
    // We check if the meeting's 'company' field links to this startup
    const companyMeetings = meetings.where(m => {
        if (!m.company) return false;
        // Handle if company is a Link [[Company]] or just text "Company"
        let linkPath = m.company.path ? m.company.path : ""; 
        let linkText = m.company.path ? "" : m.company.toString();
        
        return linkPath === s.file.path || linkText.includes(s.file.name);
    });

    // 2. Get the most recent meeting date
    let lastMeetingDate = "-";
    if (companyMeetings.length > 0) {
        // Sort by startdate descending and pick the first
        let sorted = companyMeetings.sort(m => m.startdate, 'desc');
        if (sorted[0].startdate) {
            lastMeetingDate = sorted[0].startdate;
        }
    }

    // 3. Scout Form Check (looks for 'scout_form: true' in YAML)
    let formStatus = "⬜"; // Default empty
    if (s.scout_form === true || s.scout_form === "yes") {
        formStatus = "✅";
    } else if (s.scout_form === false || s.scout_form === "no") {
        formStatus = "❌";
    }

    // 4. Map the row data
    return [
        s.file.link,
        s.date_created,       // First Contacted (Creation Date)
        lastMeetingDate,      // Most recent meeting found in Meetings folder
        formStatus,           // Scout Form Submitted?
        s.deal_status         // Final Decision/Status
    ];
});

// Render Table
dv.table(
    ["Company", "First Contacted", "Last Meeting", "Form Sent", "Status"], 
    rows.sort(r => r[1], 'desc') // Sort by First Contacted (Newest first)
);
```

---

# Follow-ups Due

_Next 14 Days_


```dataview

TABLE company AS Company, sector AS Sector, stage AS Stage, next_action AS "Next Action", next_action_date AS "Due"
FROM "100_Network/Startups"
WHERE deal_status AND next_action_date != null AND next_action_date <= date(today) + dur(14 days)
SORT next_action_date asc

```

---

# Sector Overview

```dataview 

TABLE length(rows) AS Count
FROM "100_Network/Startups"
GROUP BY sector
SORT Count desc

```

---

# Scout Submissions

```meta-bind-button
label: New Scout Submission Form
icon: ""
style: primary
class: ""
cssStyle: ""
backgroundImage: ""
tooltip: ""
id: ""
hidden: false
actions:
  - type: templaterCreateNote
    templateFile: Templates/Scout Submission Form Template.md
    folderPath: 100_Network/Scout Submissions
    fileName: New Scout Form
    openNote: true
    openIfAlreadyExists: false

```

### Drafts 
```dataview
TABLE target_company as "Startup", date_created as "Started"
FROM "100_Network/Scout Submissions"
WHERE submission_status = "Draft"
SORT date_created DESC
```

### Submitted 


```dataview
TABLE target_company as "Startup", date_created as "Date"
FROM "100_Network/Scout Submissions"
WHERE submission_status = "Submitted"
SORT date_created DESC
```