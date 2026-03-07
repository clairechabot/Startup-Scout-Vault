---
mapofcontent: yes
title: Scout Submissions MOC
moc_type: index
tags: [MOC, submissions, scout]
---

[[ellipsis dashboard]]

```ad-quote
_"Speed matters. The best deals don't wait for perfect write-ups."_
```

```meta-bind-button
label: New Submission
icon: "file-plus"
hidden: false
class: ""
tooltip: "Start a new scout submission draft"
id: "new-submission-btn"
style: primary
actions:
  - type: templaterCreateNote
    templateFile: Templates/Scout Submission Form Template.md
    folderPath: 100_Network/Scout Submissions
    fileName: New Scout Form
    openNote: true
    openIfAlreadyExists: false
```

<br>

```dataviewjs
// --- SUBMISSION PIPELINE STATS ---
const subs = dv.pages('"100_Network/Scout Submissions"');

const drafts    = subs.where(p => p.submission_status === "Draft").length;
const ready     = subs.where(p => p.submission_status === "Ready").length;
const submitted = subs.where(p => p.submission_status === "Submitted").length;
const total     = subs.length;

// Average composite score for submitted deals
const scoredSubs = subs.where(p =>
    p.submission_status === "Submitted" &&
    p.team_score && p.value_prop_score && p.traction_score && p.market_score
);
let avgScore = "-";
if (scoredSubs.length > 0) {
    const sum = scoredSubs.reduce((acc, p) => {
        return acc + ((p.team_score + p.value_prop_score + p.traction_score + p.market_score) / 4);
    }, 0);
    avgScore = (sum / scoredSubs.length).toFixed(1);
}

const html = `
<div style="display: flex; gap: 14px; margin-bottom: 24px; flex-wrap: wrap; align-items: stretch;">

  <div style="flex: 1; min-width: 120px; padding: 20px; border-radius: 12px; border: 1px solid rgba(0, 255, 255, 0.5); background: rgba(0, 255, 255, 0.05); text-align: center; box-shadow: 0 0 10px rgba(0, 255, 255, 0.1);">
    <div style="font-size: 28px; font-weight: 800; color: #00ffff; line-height: 1; margin-bottom: 8px;">${total}</div>
    <div style="font-size: 11px; text-transform: uppercase; letter-spacing: 2px; opacity: 0.8; color: #fff;">Total</div>
  </div>

  <div style="flex: 1; min-width: 120px; padding: 20px; border-radius: 12px; border: 1px solid rgba(255, 215, 0, 0.5); background: rgba(255, 215, 0, 0.05); text-align: center; box-shadow: 0 0 10px rgba(255, 215, 0, 0.1);">
    <div style="font-size: 28px; font-weight: 800; color: #ffd700; line-height: 1; margin-bottom: 8px;">${drafts}</div>
    <div style="font-size: 11px; text-transform: uppercase; letter-spacing: 2px; opacity: 0.8; color: #fff;">Drafts</div>
  </div>

  <div style="flex: 1; min-width: 120px; padding: 20px; border-radius: 12px; border: 1px solid rgba(255, 0, 255, 0.5); background: rgba(255, 0, 255, 0.05); text-align: center; box-shadow: 0 0 10px rgba(255, 0, 255, 0.1);">
    <div style="font-size: 28px; font-weight: 800; color: #ff00ff; line-height: 1; margin-bottom: 8px;">${ready}</div>
    <div style="font-size: 11px; text-transform: uppercase; letter-spacing: 2px; opacity: 0.8; color: #fff;">Ready to Send</div>
  </div>

  <div style="flex: 1; min-width: 120px; padding: 20px; border-radius: 12px; border: 1px solid rgba(0, 255, 120, 0.5); background: rgba(0, 255, 120, 0.05); text-align: center; box-shadow: 0 0 10px rgba(0, 255, 120, 0.1);">
    <div style="font-size: 28px; font-weight: 800; color: #00ff78; line-height: 1; margin-bottom: 8px;">${submitted}</div>
    <div style="font-size: 11px; text-transform: uppercase; letter-spacing: 2px; opacity: 0.8; color: #fff;">Submitted</div>
  </div>

</div>

<div style="padding: 12px 16px; border-radius: 8px; border-left: 4px solid #00ffff; background: linear-gradient(90deg, rgba(0,255,255,0.1) 0%, transparent 100%); font-size: 13px; color: #ddd; display: flex; align-items: center;">
  <span style="font-size: 16px; margin-right: 10px;">&#9733;</span>
  <span><b>Avg Score (submitted):</b> <span style="color: #00ffff;">${avgScore}</span> / 5</span>
</div>
`;
dv.paragraph(html);
```

---

# Action Required

## Drafts in Progress

_Finish these before they go stale._

```dataview
TABLE target_company AS "Startup", date_created AS "Started", raising AS "Raising?"
FROM "100_Network/Scout Submissions"
WHERE submission_status = "Draft"
SORT date_created ASC
```

## Ready to Submit

_These are done — open Airtable and send._

```dataview
TABLE target_company AS "Startup", date_created AS "Drafted"
FROM "100_Network/Scout Submissions"
WHERE submission_status = "Ready"
SORT date_created ASC
```

---

# Submitted Pipeline

```dataview
TABLE target_company AS "Startup", date_created AS "Submitted", invest_verdict AS "Verdict"
FROM "100_Network/Scout Submissions"
WHERE submission_status = "Submitted"
SORT date_created DESC
```

---

# Score Analysis

_Composite score = avg of Team, Value Prop, Traction, Market (each 1-5)._

```dataviewjs
const subs = dv.pages('"100_Network/Scout Submissions"')
    .where(p => p.team_score && p.value_prop_score && p.traction_score && p.market_score);

const rows = subs.sort(p => {
    return -((p.team_score + p.value_prop_score + p.traction_score + p.market_score) / 4);
}).map(p => {
    const composite = ((p.team_score + p.value_prop_score + p.traction_score + p.market_score) / 4).toFixed(1);
    return [
        p.file.link,
        p.submission_status ?? "-",
        p.team_score + "/5",
        p.value_prop_score + "/5",
        p.traction_score + "/5",
        p.market_score + "/5",
        composite + "/5"
    ];
});

dv.table(
    ["Startup", "Status", "Team", "Value Prop", "Traction", "Market", "Composite"],
    rows
);
```

---

# Submissions by Sector

_Spot gaps in your sourcing coverage._

```dataviewjs
const subs = dv.pages('"100_Network/Scout Submissions"');
const startups = dv.pages('"100_Network/Startups"');

const sectorMap = {};
subs.forEach(s => {
    let sector = "Unknown";
    if (s.target_company) {
        const linked = startups.where(st =>
            st.file.link.path === (s.target_company.path ?? "") ||
            st.file.name === s.target_company.toString()
        ).first();
        if (linked && linked.sector) sector = linked.sector;
    }
    if (!sectorMap[sector]) sectorMap[sector] = { total: 0, submitted: 0 };
    sectorMap[sector].total += 1;
    if (s.submission_status === "Submitted") sectorMap[sector].submitted += 1;
});

const rows = Object.entries(sectorMap)
    .sort((a, b) => b[1].total - a[1].total)
    .map(([sector, counts]) => [sector, counts.total, counts.submitted]);

dv.table(["Sector", "Total", "Submitted"], rows);
```

---

# Verdict Breakdown

_Your own conviction track record._

```dataviewjs
const subs = dv.pages('"100_Network/Scout Submissions"')
    .where(p => p.invest_verdict);

const yes   = subs.where(p => p.invest_verdict === "Yes").length;
const maybe = subs.where(p => p.invest_verdict === "Maybe").length;
const no    = subs.where(p => p.invest_verdict === "No").length;

dv.table(
    ["Verdict", "Count"],
    [["Yes", yes], ["Maybe", maybe], ["No", no]]
);
```
