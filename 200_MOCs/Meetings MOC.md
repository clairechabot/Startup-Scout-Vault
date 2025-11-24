---
mapofcontent: yes
title: Meetings MOC
moc_type: index
tags: [MOC, meetings, archive]
---

[[ellipsis dashboard]]

%%**Template:** [[Meeting Template]]%%

---

```ad-quote
_"The faint ink is better than the best memory."_
```

%% Meetings are timestamped events with other people, where information is exchanged and collected. These can also be linked to [[Network MOC]]. %% 

<br>


> [!col]
>
> > [!col-md]
> >
> > ```meta-bind-button
> > label: General Meeting
> > icon: "calendar"
> > hidden: false
> > class: ""
> > tooltip: "Standard internal or networking meeting"
> > id: "btn-general-meeting"
> > style: primary
> > actions:
> >   - type: templaterCreateNote
> >     templateFile: Templates/Meeting Template.md
> >     folderPath: 100_Network/Meetings
> >     fileName: Meeting 
> >     openNote: true
> > ```
>
> > [!col-md]
> >
> > ```meta-bind-button
> > label: Startup Intro
> > icon: "rocket"
> > hidden: false
> > class: ""
> > tooltip: "First call with a founder"
> > id: "btn-startup-meeting"
> > style: primary
> > actions:
> >   - type: templaterCreateNote
> >     templateFile: Templates/First Meeting Template.md
> >     folderPath: 100_Network/Meetings
> >     fileName: Intro Call
> >     openNote: true
> > ```

```dataviewjs

// --- DATA COLLECTION ---
const pages = dv.pages('"100_Network/Meetings"').where(p => p.file.name !== "Meeting Template" && p.file.name !== "Startup Meeting Template");

// Segregate the Archive
const planned = pages.where(p => p.status == "planned");
const startup_intros = pages.where(p => (p.tags ?? []).includes("startup_intro"));
const general = pages.where(p => !(p.tags ?? []).includes("startup_intro"));

// --- RENDER INVENTORY DASHBOARD ---
const html = `
<div style="display: flex; gap: 14px; margin-bottom: 24px; flex-wrap: wrap; align-items: stretch;">

  <div style="flex: 1; min-width: 120px; padding: 20px; border-radius: 12px; border: 1px solid rgba(0, 255, 255, 0.5); background: rgba(0, 255, 255, 0.05); text-align: center; box-shadow: 0 0 10px rgba(0, 255, 255, 0.1);">
    <div style="font-size: 28px; font-weight: 800; color: #00ffff; line-height: 1; margin-bottom: 8px;">${planned.length}</div>
    <div style="font-size: 11px; text-transform: uppercase; letter-spacing: 2px; opacity: 0.8; color: #fff;">Scheduled</div>
  </div>

  <div style="flex: 1; min-width: 120px; padding: 20px; border-radius: 12px; border: 1px solid rgba(255, 0, 255, 0.5); background: rgba(255, 0, 255, 0.05); text-align: center;">
    <div style="font-size: 28px; font-weight: 800; color: #ff00ff; line-height: 1; margin-bottom: 8px;">${startup_intros.length}</div>
    <div style="font-size: 11px; text-transform: uppercase; letter-spacing: 2px; opacity: 0.8; color: #fff;">Founder Calls</div>
  </div>

  <div style="flex: 1; min-width: 120px; padding: 20px; border-radius: 12px; border: 1px solid rgba(160, 160, 160, 0.5); background: rgba(160, 160, 160, 0.05); text-align: center;">
    <div style="font-size: 28px; font-weight: 800; color: #a0a0a0; line-height: 1; margin-bottom: 8px;">${general.length}</div>
    <div style="font-size: 11px; text-transform: uppercase; letter-spacing: 2px; opacity: 0.8; color: #fff;">General / Misc</div>
  </div>

</div>
`;

dv.paragraph(html);

```


<br>

---

# Upcoming Meetings

<br>

```dataview
TABLE startdate AS "Date", start_time AS "Time", location AS "Location", summary AS "Topic"
FROM "100_Network/Meetings"
WHERE status = "planned"
SORT startdate ASC, start_time ASC
```

<br>
<br>

---

# Startup Screening Archieve

<br>

```dataview
TABLE company AS "Startup", startdate AS "Date", summary AS "Notes", deal_status AS "Status"
FROM "100_Network/Meetings"
WHERE contains(tags, "startup_intro")
SORT startdate DESC
```

<br>
<br>

---

# General Meeting Log

<br>

```dataview
TABLE startdate AS "Date", summary AS "Topic", company AS "Related Entity"
FROM "100_Network/Meetings"
WHERE !contains(tags, "startup_intro") AND status != "planned"
SORT startdate DESC
LIMIT 25
```
