---
mapofcontent: yes
title: Meetings MOC
moc_type: index
tags: [MOC, meetings]
---

[[ellipsis dashboard]]

```ad-quote
_"The faint ink is better than the best memory."_
```

> [!col]
>
> > [!col-md]
> >
> > ```meta-bind-button
> > label: Startup Intro Call
> > icon: "rocket"
> > style: primary
> > id: "btn-startup-meeting"
> > actions:
> >   - type: templaterCreateNote
> >     templateFile: Templates/First Meeting Template.md
> >     folderPath: 100_Network/Meetings
> >     fileName: Intro Call
> >     openNote: true
> > ```
>
> > [!col-md]
> >
> > ```meta-bind-button
> > label: General Meeting
> > icon: "calendar"
> > style: primary
> > id: "btn-general-meeting"
> > actions:
> >   - type: templaterCreateNote
> >     templateFile: Templates/Meeting Template.md
> >     folderPath: 100_Network/Meetings
> >     fileName: Meeting
> >     openNote: true
> > ```

<br>

```dataviewjs
// --- MEETINGS STATS ---
const meetings = dv.pages('"100_Network/Meetings"')
    .where(p => p.file.name !== "Meeting Template" && p.file.name !== "First Meeting Template");

const now = dv.date("today");
const thisMonth = now.month;
const thisYear  = now.year;

const scheduled   = meetings.where(p => (p.status === "Scheduled" || p.status === "Follow-up Pending") && p.startdate >= now).length;
const founderCalls = meetings.where(p => (p.tags ?? []).includes("startup_intro")).length;
const doneThisMonth = meetings.where(p => {
    if (!p.startdate) return false;
    const d = p.startdate;
    return (p.status === "Completed" || p.status === "Follow-up Pending" || p.status === "Cancelled") && d.month === thisMonth && d.year === thisYear;
}).length;
const followupsDue = meetings.where(p => {
    return (p.file.tasks ?? []).filter(t => !t.completed && t.due && t.due <= now.plus({days: 7})).length > 0;
}).length;

const html = `
<div style="display: flex; gap: 14px; margin-bottom: 24px; flex-wrap: wrap; align-items: stretch;">

  <div style="flex: 1; min-width: 120px; padding: 20px; border-radius: 12px; border: 1px solid rgba(0, 255, 255, 0.5); background: rgba(0, 255, 255, 0.05); text-align: center; box-shadow: 0 0 10px rgba(0, 255, 255, 0.1);">
    <div style="font-size: 28px; font-weight: 800; color: #00ffff; line-height: 1; margin-bottom: 8px;">${scheduled}</div>
    <div style="font-size: 11px; text-transform: uppercase; letter-spacing: 2px; opacity: 0.8; color: #fff;">Scheduled</div>
  </div>

  <div style="flex: 1; min-width: 120px; padding: 20px; border-radius: 12px; border: 1px solid rgba(255, 0, 255, 0.5); background: rgba(255, 0, 255, 0.05); text-align: center; box-shadow: 0 0 10px rgba(255, 0, 255, 0.1);">
    <div style="font-size: 28px; font-weight: 800; color: #ff00ff; line-height: 1; margin-bottom: 8px;">${founderCalls}</div>
    <div style="font-size: 11px; text-transform: uppercase; letter-spacing: 2px; opacity: 0.8; color: #fff;">Founder Calls</div>
  </div>

  <div style="flex: 1; min-width: 120px; padding: 20px; border-radius: 12px; border: 1px solid rgba(255, 215, 0, 0.5); background: rgba(255, 215, 0, 0.05); text-align: center; box-shadow: 0 0 10px rgba(255, 215, 0, 0.1);">
    <div style="font-size: 28px; font-weight: 800; color: #ffd700; line-height: 1; margin-bottom: 8px;">${doneThisMonth}</div>
    <div style="font-size: 11px; text-transform: uppercase; letter-spacing: 2px; opacity: 0.8; color: #fff;">Done This Month</div>
  </div>

  <div style="flex: 1; min-width: 120px; padding: 20px; border-radius: 12px; border: 1px solid rgba(255, 80, 80, 0.5); background: rgba(255, 80, 80, 0.05); text-align: center; box-shadow: 0 0 10px rgba(255, 80, 80, 0.1);">
    <div style="font-size: 28px; font-weight: 800; color: #ff5050; line-height: 1; margin-bottom: 8px;">${followupsDue}</div>
    <div style="font-size: 11px; text-transform: uppercase; letter-spacing: 2px; opacity: 0.8; color: #fff;">Follow-ups Due</div>
  </div>

</div>
`;
dv.paragraph(html);
```

---

# Upcoming Schedule

```dataview
TABLE startdate AS "Date", start_time AS "Time", company AS "Company", summary AS "Topic", location AS "Location"
FROM "100_Network/Meetings"
WHERE (status = "Scheduled" OR status = "Follow-up Pending") AND startdate >= date(today)
SORT startdate ASC, start_time ASC
```

---

# Follow-ups Due (Next 7 Days)

```dataview
TASK
FROM "100_Network/Meetings"
WHERE !completed AND due AND due <= date(today) + dur(7 days)
SORT due ASC
GROUP BY file.link
```

---

# Startup Intro Calls

_All founder-facing meetings tagged `startup_intro`, newest first._

```dataview
TABLE startdate AS "Date", company AS "Startup", summary AS "Notes", status AS "Outcome"
FROM "100_Network/Meetings"
WHERE contains(tags, "startup_intro")
SORT startdate DESC
```

---

# Sector Breakdown

_How many founder calls per sector? Spot sourcing imbalances._

```dataviewjs
const meetings = dv.pages('"100_Network/Meetings"')
    .where(p => (p.tags ?? []).includes("startup_intro") && p.company);

// Try to join to startup for sector — fall back to meeting-level sector if present
const startups = dv.pages('"100_Network/Startups"');

const sectorMap = {};
meetings.forEach(m => {
    let sector = m.sector ?? "Unknown";
    // Try to look up sector from linked startup
    if (m.company) {
        const linked = startups.where(s => s.file.link.path === (m.company.path ?? "")).first();
        if (linked && linked.sector) sector = linked.sector;
    }
    sectorMap[sector] = (sectorMap[sector] ?? 0) + 1;
});

const rows = Object.entries(sectorMap).sort((a, b) => b[1] - a[1]);
dv.table(["Sector", "Intro Calls"], rows);
```

---

# General Meeting Log

_Internal calls, networking, partner meetings._

```dataview
TABLE startdate AS "Date", company AS "Related Entity", summary AS "Topic", status AS "Status"
FROM "100_Network/Meetings"
WHERE !contains(tags, "startup_intro") AND (status = "Completed" OR status = "Follow-up Pending" OR status = "Cancelled" OR status = "Rescheduled")
SORT startdate DESC
LIMIT 30
```

---

# Cancelled / Rescheduled

```dataview
TABLE startdate AS "Date", company AS "Company", summary AS "Topic", status AS "Status"
FROM "100_Network/Meetings"
WHERE status = "Cancelled" OR status = "Rescheduled"
SORT startdate DESC
```
