---
type: weekly_review
week_start: <% tp.date.now("YYYY-MM-DD", 0 - ((parseInt(tp.date.now("E")) + 6) % 7)) %>
week_end: <% tp.date.now("YYYY-MM-DD", 6 - ((parseInt(tp.date.now("E")) + 6) % 7)) %>
week_number: <% tp.date.now("WW") %>
date_created: <% tp.date.now("YYYY-MM-DD") %>
tags:
  - reflection
  - weekly
---

[[ellipsis dashboard]]

# Week <% tp.date.now("WW") %> · <% tp.date.now("YYYY-MM-DD", 0 - ((parseInt(tp.date.now("E")) + 6) % 7)) %> → <% tp.date.now("YYYY-MM-DD", 6 - ((parseInt(tp.date.now("E")) + 6) % 7)) %>

> [!abstract] Headline
> _One sentence: what defined this week?_

---

# 1. Deal Flow Metrics

```dataviewjs
// --- WEEK WINDOW ---
const created    = dv.date(dv.current().date_created);
const dow        = created.weekday; // 1=Mon, 7=Sun (ISO)
const weekStart  = created.minus({ days: dow - 1 });
const weekEnd    = weekStart.plus({ days: 6 });

const startups   = dv.pages('"100_Network/Startups"').where(p => p.file.name !== "Startup Template");
const newDeals   = startups.where(p => {
    const d = p.file.cday;
    return d >= weekStart && d <= weekEnd;
});
const meetings   = dv.pages('"100_Network/Meetings"')
    .where(p => p.file.name !== "Meeting Template" && p.file.name !== "First Meeting Template");
const founderMeetings = meetings.where(p => {
    const d = p.startdate;
    if (!d) return false;
    return (p.tags ?? []).includes("startup_intro") &&
           d >= weekStart && d <= weekEnd;
});
const hotDeals   = startups.where(p => (p.deal_temperature ?? "").toLowerCase() === "hot");
const sectors    = [...new Set(newDeals.map(p => p.sector ?? "Unknown").array())];

const html = `
<div style="display: flex; gap: 12px; margin-bottom: 20px; flex-wrap: wrap;">

  <div style="flex: 1; min-width: 110px; padding: 18px; border-radius: 10px; border: 1px solid rgba(0, 255, 255, 0.5); background: rgba(0, 255, 255, 0.05); text-align: center; box-shadow: 0 0 8px rgba(0, 255, 255, 0.1);">
    <div style="font-size: 26px; font-weight: 800; color: #00ffff; line-height: 1; margin-bottom: 6px;">${newDeals.length}</div>
    <div style="font-size: 10px; text-transform: uppercase; letter-spacing: 2px; opacity: 0.8; color: #fff;">New Deals</div>
  </div>

  <div style="flex: 1; min-width: 110px; padding: 18px; border-radius: 10px; border: 1px solid rgba(255, 0, 255, 0.5); background: rgba(255, 0, 255, 0.05); text-align: center; box-shadow: 0 0 8px rgba(255, 0, 255, 0.1);">
    <div style="font-size: 26px; font-weight: 800; color: #ff00ff; line-height: 1; margin-bottom: 6px;">${founderMeetings.length}</div>
    <div style="font-size: 10px; text-transform: uppercase; letter-spacing: 2px; opacity: 0.8; color: #fff;">Founders Met</div>
  </div>

  <div style="flex: 1; min-width: 110px; padding: 18px; border-radius: 10px; border: 1px solid rgba(255, 80, 60, 0.6); background: rgba(255, 80, 60, 0.07); text-align: center; box-shadow: 0 0 8px rgba(255, 80, 60, 0.15);">
    <div style="font-size: 26px; font-weight: 800; color: #ff503c; line-height: 1; margin-bottom: 6px;">${hotDeals.length}</div>
    <div style="font-size: 10px; text-transform: uppercase; letter-spacing: 2px; opacity: 0.8; color: #fff;">Hot Deals</div>
  </div>

  <div style="flex: 1; min-width: 110px; padding: 18px; border-radius: 10px; border: 1px solid rgba(160, 160, 160, 0.4); background: rgba(160, 160, 160, 0.05); text-align: center;">
    <div style="font-size: 26px; font-weight: 800; color: #a0a0a0; line-height: 1; margin-bottom: 6px;">${sectors.length}</div>
    <div style="font-size: 10px; text-transform: uppercase; letter-spacing: 2px; opacity: 0.8; color: #fff;">Sectors Touched</div>
  </div>

</div>
`;
dv.paragraph(html);
```

## New Deals This Week

```dataviewjs
const created   = dv.date(dv.current().date_created);
const dow       = created.weekday;
const weekStart = created.minus({ days: dow - 1 });
const weekEnd   = weekStart.plus({ days: 6 });

const newDeals  = dv.pages('"100_Network/Startups"')
    .where(p => p.file.name !== "Startup Template" && p.file.cday >= weekStart && p.file.cday <= weekEnd);

if (newDeals.length === 0) {
    dv.paragraph("_No new deals created this week._");
} else {
    dv.table(
        ["Company", "Sector", "Stage", "Temperature", "Source"],
        newDeals.sort(p => p.file.ctime, "desc").map(p => [
            p.file.link,
            p.sector ?? "-",
            p.stage ?? "-",
            p.deal_temperature ?? "-",
            p.investors ?? "-"
        ])
    );
}
```

## Founder Calls This Week

```dataviewjs
const created   = dv.date(dv.current().date_created);
const dow       = created.weekday;
const weekStart = created.minus({ days: dow - 1 });
const weekEnd   = weekStart.plus({ days: 6 });

const calls = dv.pages('"100_Network/Meetings"')
    .where(p => (p.tags ?? []).includes("startup_intro") && p.startdate >= weekStart && p.startdate <= weekEnd);

if (calls.length === 0) {
    dv.paragraph("_No founder calls logged this week._");
} else {
    dv.table(
        ["Meeting", "Company", "Date", "Outcome"],
        calls.sort(p => p.startdate).map(p => [
            p.file.link,
            p.company ?? "-",
            p.startdate,
            p.status ?? "-"
        ])
    );
}
```

---

# 2. Pipeline Status

## Current Pipeline Snapshot

```dataviewjs
const startups = dv.pages('"100_Network/Startups"').where(p => p.file.name !== "Startup Template");
const by = s => startups.where(p => (p.deal_status ?? "").toLowerCase().includes(s));

const stages = [
    ["Screening",      by("screening").length,      "#a0a0a0"],
    ["Call Scheduled", by("call").length,            "#ffd700"],
    ["In DD",          by("in dd").length,           "#ff00ff"],
    ["Active/Ongoing", by("active").length + by("ongoing").length, "#00ffff"],
    ["Invested",       by("invested").length,        "#00ff78"],
    ["Paused",         by("paused").length,          "#888888"],
    ["Passed",         by("passed").length,          "#444444"],
];

const rows = stages.filter(([_, count]) => count > 0).map(([stage, count, color]) => {
    const bar = "█".repeat(Math.min(count * 2, 20));
    return [stage, count, `<span style="color:${color}">${bar}</span>`];
});

dv.table(["Stage", "Count", ""], rows);
```

## Submissions This Week

```dataviewjs
const created   = dv.date(dv.current().date_created);
const dow       = created.weekday;
const weekStart = created.minus({ days: dow - 1 });
const weekEnd   = weekStart.plus({ days: 6 });

const subs = dv.pages('"100_Network/Scout Submissions"')
    .where(p => p.file.cday >= weekStart && p.file.cday <= weekEnd);

if (subs.length === 0) {
    dv.paragraph("_No submissions created or updated this week._");
} else {
    dv.table(
        ["Submission", "Company", "Status", "Scores (T/VP/TR/M)"],
        subs.sort(p => p.file.mtime, "desc").map(p => [
            p.file.link,
            p.target_company ?? "-",
            p.submission_status ?? "-",
            `${p.team_score ?? "?"} / ${p.value_prop_score ?? "?"} / ${p.traction_score ?? "?"} / ${p.market_score ?? "?"}`
        ])
    );
}
```

## Deals Currently In DD

```dataview
TABLE sector AS "Sector", stage AS "Stage", deal_temperature AS "Temp", next_action AS "Next Action", next_action_date AS "Due"
FROM "100_Network/Startups"
WHERE deal_status = "In DD" OR deal_status = "Active" OR deal_status = "Ongoing"
SORT next_action_date ASC
```

---

# 3. Sector Analysis

```dataviewjs
// Sector heat map: deal count + temperature breakdown for the full pipeline
const startups = dv.pages('"100_Network/Startups"').where(p => p.file.name !== "Startup Template" && p.deal_status && p.deal_status !== "Passed");
const created   = dv.date(dv.current().date_created);
const dow       = created.weekday;
const weekStart = created.minus({ days: dow - 1 });

const sectorMap = {};
startups.forEach(p => {
    const s = p.sector ?? "Unknown";
    if (!sectorMap[s]) sectorMap[s] = { total: 0, hot: 0, warm: 0, cold: 0, new: 0 };
    sectorMap[s].total += 1;
    const temp = (p.deal_temperature ?? "").toLowerCase();
    if (temp === "hot")  sectorMap[s].hot  += 1;
    if (temp === "warm") sectorMap[s].warm += 1;
    if (temp === "cold") sectorMap[s].cold += 1;
    if (p.file.cday >= weekStart) sectorMap[s].new += 1;
});

const rows = Object.entries(sectorMap)
    .sort((a, b) => b[1].hot - a[1].hot || b[1].total - a[1].total)
    .map(([sector, c]) => [
        sector,
        c.total,
        c.hot  > 0 ? `🔴 ${c.hot}`  : "-",
        c.warm > 0 ? `🟡 ${c.warm}` : "-",
        c.new  > 0 ? `+${c.new}`    : "-"
    ]);

dv.table(["Sector", "Pipeline", "Hot", "Warm", "New This Week"], rows);
```

**Heating up:**
_Which sectors had new deals or temperature increases this week? Write observations._

-

**Cooling down / gaps:**
_Sectors that are quiet or underrepresented in the thesis._

-

---

# 4. Relationship Building

## New Contacts This Week

```dataviewjs
const created   = dv.date(dv.current().date_created);
const dow       = created.weekday;
const weekStart = created.minus({ days: dow - 1 });
const weekEnd   = weekStart.plus({ days: 6 });

const people = dv.pages('"100_Network/People"')
    .where(p => p.file.cday >= weekStart && p.file.cday <= weekEnd);

if (people.length === 0) {
    dv.paragraph("_No new contacts added this week._");
} else {
    dv.table(
        ["Person", "Role", "Org", "Relationship Status"],
        people.sort(p => p.file.ctime, "desc").map(p => [
            p.file.link,
            p.role ?? "-",
            p.company ?? "-",
            p.relationship_status ?? "Prospect"
        ])
    );
}
```

## All Meetings This Week

```dataviewjs
const created   = dv.date(dv.current().date_created);
const dow       = created.weekday;
const weekStart = created.minus({ days: dow - 1 });
const weekEnd   = weekStart.plus({ days: 6 });

const meetings = dv.pages('"100_Network/Meetings"')
    .where(p => p.file.name !== "Meeting Template" && p.file.name !== "First Meeting Template")
    .where(p => p.startdate >= weekStart && p.startdate <= weekEnd);

if (meetings.length === 0) {
    dv.paragraph("_No meetings logged this week._");
} else {
    dv.table(
        ["Meeting", "Company / Entity", "Type", "Date", "Status"],
        meetings.sort(p => p.startdate).map(p => {
            const isIntro = (p.tags ?? []).includes("startup_intro");
            return [
                p.file.link,
                p.company ?? "-",
                isIntro ? "Founder Call" : "General",
                p.startdate,
                p.status ?? "-"
            ];
        })
    );
}
```

**Warm intros facilitated:**
_Did you connect anyone in your network this week?_

-

**Relationships to nurture next week:**
_Who haven't you heard from in a while?_

-

---

# 5. Lessons Learned

## What Worked

> _Sourcing strategies, conversation approaches, research methods that paid off._

-

-

## What to Improve

> _Friction points, missed signals, process gaps._

-

-

## One Insight

> _The most interesting thing you learned this week — about a founder, a sector, or the market._

---

# 6. Next Week Focus

_3–5 specific, actionable goals. Not vague — name the company, person, or task._

- [ ]
- [ ]
- [ ]
- [ ]
- [ ]

## Carries Over From This Week

_Tasks that were planned but didn't happen — decide: do them next week or drop them._

```dataview
TASK
WHERE !completed AND due <= date(today) AND due >= date(today) - dur(7 days)
SORT due ASC
```

---

# Vault Activity Log

_Everything created or significantly edited this week — for your records._

```dataviewjs
const created   = dv.date(dv.current().date_created);
const dow       = created.weekday;
const weekStart = created.minus({ days: dow - 1 });
const weekEnd   = weekStart.plus({ days: 6 });

const allNotes = dv.pages("")
    .where(p =>
        !p.file.path.includes("Templates/") &&
        !p.file.path.includes(".obsidian/") &&
        !p.file.path.includes("200_MOCs/") &&
        (p.file.cday >= weekStart && p.file.cday <= weekEnd)
    )
    .sort(p => p.file.ctime, "desc");

if (allNotes.length === 0) {
    dv.paragraph("_No notes created this week outside of system files._");
} else {
    dv.table(
        ["Note", "Folder", "Created"],
        allNotes.map(p => [
            p.file.link,
            p.file.folder,
            p.file.ctime
        ])
    );
}
```
