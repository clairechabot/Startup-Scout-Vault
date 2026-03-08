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
const ongoing  = by("ongoing").length + by("in dd").length + by("active").length + by("call").length;
const invested = by("invested").length;
const paused   = by("paused").length;
const archived = by("archiv").length + by("passed").length;
const hot      = pages.where(p => (p.deal_temperature ?? "").toLowerCase() === "hot").length;
const recent   = pages.where(p => p.file.mtime.ts >= (Date.now() - 1000*60*60*24*30)).length;

// --- RENDER DASHBOARD ---
const html = `
<div style="display: flex; gap: 14px; margin-bottom: 16px; flex-wrap: wrap; align-items: stretch;">

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

  <div style="flex: 1; min-width: 120px; padding: 20px; border-radius: 12px; border: 1px solid rgba(255, 80, 60, 0.7); background: rgba(255, 80, 60, 0.08); text-align: center; box-shadow: 0 0 14px rgba(255, 80, 60, 0.2);">
    <div style="font-size: 28px; font-weight: 800; color: #ff503c; line-height: 1; margin-bottom: 8px;">${hot}</div>
    <div style="font-size: 11px; text-transform: uppercase; letter-spacing: 2px; opacity: 0.8; color: #fff;">Hot</div>
  </div>

</div>

<div style="padding: 12px 16px; border-radius: 8px; border-left: 4px solid #00ffff; background: linear-gradient(90deg, rgba(0,255,255,0.1) 0%, transparent 100%); font-size: 13px; color: #ddd; display: flex; align-items: center;">
  <span style="font-size: 16px; margin-right: 10px;">&#9889;</span>
  <span><b>Momentum:</b> <span style="color: #00ffff;">${recent}</span> startups updated in the last 30 days.</span>
</div>
`;
dv.paragraph(html);
```

---

# Hot Deals

_Temperature marked `hot` — act on these first._

```dataviewjs
const pages   = dv.pages('"100_Network/Startups"').where(p => (p.deal_temperature ?? "").toLowerCase() === "hot");
const meetings = dv.pages('"100_Network/Meetings"');

if (pages.length === 0) {
    dv.paragraph("_No hot deals right now. Mark a deal as `hot` in its frontmatter to surface it here._");
} else {
    const rows = pages.map(s => {
        const lastMeeting = meetings
            .where(m => m.company && (m.company.path === s.file.path || (m.company.toString() ?? "").includes(s.file.name)))
            .sort(m => m.startdate, "desc")
            .first();
        const lastDate = lastMeeting?.startdate ?? "-";

        const people = (s.related_people ?? []).map(p => p.path ? `[[${p.path.split("/").pop()}]]` : p.toString()).join(", ");

        return [
            s.file.link,
            s.deal_status ?? "-",
            lastDate,
            s.next_action ?? "-",
            s.next_action_date ?? "-",
            people || "-"
        ];
    });

    dv.table(
        ["Company", "Status", "Last Meeting", "Next Action", "Due", "Key People"],
        rows.sort(r => r[4])
    );
}
```

---

# Master Pipeline

```dataviewjs
// --- CONFIGURATION ---
const startupFolder = '"100_Network/Startups"';
const meetingFolder = '"100_Network/Meetings"';

const startups = dv.pages(startupFolder).where(p => p.file.name !== "Startup Template");
const meetings = dv.pages(meetingFolder);

const tempLabel = t => {
    const v = (t ?? "").toLowerCase();
    if (v === "hot")  return "🔴 Hot";
    if (v === "warm") return "🟡 Warm";
    if (v === "cold") return "🔵 Cold";
    return "-";
};

const rows = startups.map(s => {
    const companyMeetings = meetings.where(m => {
        if (!m.company) return false;
        const path = m.company.path ?? "";
        const text = m.company.path ? "" : m.company.toString();
        return path === s.file.path || text.includes(s.file.name);
    });

    let lastMeetingDate = "-";
    if (companyMeetings.length > 0) {
        const sorted = companyMeetings.sort(m => m.startdate, "desc");
        if (sorted[0].startdate) lastMeetingDate = sorted[0].startdate;
    }

    const formStatus = s.scout_form === true || s.scout_form === "yes" ? "✅"
                     : s.scout_form === false || s.scout_form === "no"  ? "❌"
                     : "⬜";

    return [
        s.file.link,
        s.date_created,
        lastMeetingDate,
        formStatus,
        tempLabel(s.deal_temperature),
        s.deal_status
    ];
});

dv.table(
    ["Company", "First Contacted", "Last Meeting", "Form", "Temp", "Status"],
    rows.sort(r => r[1], "desc")
);
```

---

# Follow-ups Due

_Next 14 Days_

```dataview
TABLE company AS Company, sector AS Sector, stage AS Stage, next_action AS "Next Action", next_action_date AS "Due"
FROM "100_Network/Startups"
WHERE deal_status AND next_action_date != null AND next_action_date <= date(today) + dur(14 days)
SORT next_action_date ASC
```

---

# Sector Overview

```dataviewjs
const pages = dv.pages('"100_Network/Startups"').where(p => p.file.name !== "Startup Template");

const sectorMap = {};
pages.forEach(p => {
    const sector = p.sector ?? "Unknown";
    if (!sectorMap[sector]) sectorMap[sector] = { total: 0, hot: 0, warm: 0 };
    sectorMap[sector].total += 1;
    const temp = (p.deal_temperature ?? "").toLowerCase();
    if (temp === "hot")  sectorMap[sector].hot  += 1;
    if (temp === "warm") sectorMap[sector].warm += 1;
});

const rows = Object.entries(sectorMap)
    .sort((a, b) => b[1].total - a[1].total)
    .map(([sector, c]) => [sector, c.total, c.hot > 0 ? `🔴 ${c.hot}` : "-", c.warm > 0 ? `🟡 ${c.warm}` : "-"]);

dv.table(["Sector", "Total", "Hot", "Warm"], rows);
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
id: "new-scout-form-btn"
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
TABLE target_company AS "Startup", date_created AS "Started"
FROM "100_Network/Scout Submissions"
WHERE submission_status = "Draft"
SORT date_created DESC
```

### Submitted

```dataview
TABLE target_company AS "Startup", date_created AS "Date"
FROM "100_Network/Scout Submissions"
WHERE submission_status = "Submitted"
SORT date_created DESC
```
