---
aliases: [Cockpit]
tags: 
cssclasses:
  - wide-page
title: The Cockpit
date_created: 2024-04-20-Saturday
date_modified: 2025-11-23
banner: "![[neon_wave.png]]"
banner_y: 0.68
---

```widgets
type: clock
```

> [!col]
>
> > [!col-md]
> >
> > ```meta-bind-button
> > label: Academic MOC
> > icon: "graduation-cap"
> > hidden: false
> > class: ""
> > tooltip: "Go to Course MOC"
> > id: "nav-academy"
> > style: primary
> > actions:
> >   - type: open
> >     link: "[[Academic MOC]]"
> >     newTab: true
> > ```
>
> > [!col-md]
> >
> > ```meta-bind-button
> > label: Daily Note
> > icon: "calendar-check"
> > hidden: false
> > class: ""
> > tooltip: "Create or open today's daily note"
> > id: "nav-daily"
> > style: primary
> > actions:
> >   - type: command
> >     command: daily-notes
> > ```
>
> > [!col-md]
> >
> > ```meta-bind-button
> > label: Frameworks
> > icon: "pen-tool"
> > hidden: false
> > class: ""
> > tooltip: "Go to Frameworks & Playbooks MOC"
> > id: "nav-frameworks"
> > style: primary
> > actions:
> >   - type: open
> >     link: "[[Frameworks & Playbooks MOC]]"
> >     newTab: true
> > ```
>
> > [!col-md]
> >
> > ```meta-bind-button
> > label: Network MOC
> > icon: "users"
> > hidden: false
> > class: ""
> > tooltip: "Go to People/Network MOC"
> > id: "nav-directory"
> > style: primary
> > actions:
> >   - type: open
> >     link: "[[Network MOC]]"
> >     newTab: true
> > ```
>
> > [!col-md]
> >
> > ```meta-bind-button
> > label: Startup MOC
> > icon: "rocket"
> > hidden: false
> > class: ""
> > tooltip: "Go to Startup MOC"
> > id: "nav-deals"
> > style: primary
> > actions:
> >   - type: open
> >     link: "[[Startup MOC]]"
> >     newTab: true
> > ```
>
> > [!col-md]
> >
> > ```meta-bind-button
> > label: Meeting MOC
> > icon: "calendar"
> > hidden: false
> > class: ""
> > tooltip: "Go to Meetings MOC"
> > id: "nav-meetings"
> > style: primary
> > actions:
> >   - type: open
> >     link: "[[Meetings MOC]]"
> >     newTab: true
> > ```
>
> > [!col-md]
> >
> > ```meta-bind-button
> > label: Scout Submission Form
> > icon: "file-send"
> > style: primary
> > class: ""
> > tooltip: "Direct link to the Ellipsis Airtable form"
> > id: "nav-submission"
> > hidden: false
> > actions:
> >   - type: open
> >     link: https://airtable.com/app8pOOPifhS6GORl/pagzEa0tdLFg1kAsc/form
> >     newTab: true
> > ```

---

```dataviewjs
// --- PULLING KEY METRICS FROM VAULT ---
const startups = dv.pages('"100_Network/Startups"');
const contacts = dv.pages('"100_Network/Contacts"');
const submissions = dv.pages('"100_Network/Startups/Submissions"');
const meetings = dv.pages('"100_Network/Meetings"');
const now = dv.date('today');

// **NEW:** Calculate ALL tasks due within the next 7 days (or overdue)
const totalTasksDue = dv.pages("") // Search entire vault for tasks
    .file.tasks
    .where(t => !t.completed && t.due && t.due <= now.plus({days: 7}))
    .length;

// OLD Logic (retained)
const activeDeals = startups.where(p => p.deal_status && p.deal_status.toLowerCase().includes("ongoing") || p.deal_status.toLowerCase().includes("in dd")).length;
const pendingSubmissions = submissions.where(p => p.submission_status == "Draft" || p.submission_status == "Ready").length;
const plannedMeetings = meetings.where(p => p.status == "planned" && p.startdate >= now).length;


const html = `
<div style="display: flex; gap: 14px; margin-bottom: 24px; flex-wrap: wrap; align-items: stretch;">

  <div style="flex: 1; min-width: 100px; padding: 15px; border-radius: 12px; border: 1px solid rgba(0, 255, 255, 0.5); background: rgba(0, 255, 255, 0.05); text-align: center; box-shadow: 0 0 10px rgba(0, 255, 255, 0.1);">
    <div style="font-size: 24px; font-weight: 800; color: #00ffff; line-height: 1; margin-bottom: 5px;">${activeDeals}</div>
    <div style="font-size: 10px; text-transform: uppercase; letter-spacing: 1px; opacity: 0.8; color: #fff;">Active Deals</div>
  </div>

  <div style="flex: 1; min-width: 100px; padding: 15px; border-radius: 12px; border: 1px solid rgba(255, 0, 255, 0.5); background: rgba(255, 0, 255, 0.05); text-align: center; box-shadow: 0 0 10px rgba(255, 0, 255, 0.1);">
    <div style="font-size: 24px; font-weight: 800; color: #ff00ff; line-height: 1; margin-bottom: 5px;">${pendingSubmissions}</div>
    <div style="font-size: 10px; text-transform: uppercase; letter-spacing: 1px; opacity: 0.8; color: #fff;">Pending Submissions</div>
  </div>

  <div style="flex: 1; min-width: 100px; padding: 15px; border-radius: 12px; border: 1px solid rgba(255, 215, 0, 0.5); background: rgba(255, 215, 0, 0.05); text-align: center; box-shadow: 0 0 10px rgba(255, 215, 0, 0.1);">
    <div style="font-size: 24px; font-weight: 800; color: #ffd700; line-height: 1; margin-bottom: 5px;">${totalTasksDue}</div>
    <div style="font-size: 10px; text-transform: uppercase; letter-spacing: 1px; opacity: 0.8; color: #fff;">Actionable Tasks</div>
  </div>

  <div style="flex: 1; min-width: 100px; padding: 15px; border-radius: 12px; border: 1px solid rgba(160, 160, 160, 0.5); background: rgba(160, 160, 160, 0.05); text-align: center; box-shadow: 0 0 10px rgba(160, 160, 160, 0.1);">
    <div style="font-size: 24px; font-weight: 800; color: #a0a0a0; line-height: 1; margin-bottom: 5px;">${plannedMeetings}</div>
    <div style="font-size: 10px; text-transform: uppercase; letter-spacing: 1px; opacity: 0.8; color: #fff;">Planned Calls</div>
  </div>

</div>
`;
dv.paragraph(html);
```

# Action Items

## Network Follow-ups 

```dataview 
TASK
FROM "100_Network"
WHERE due != null AND !completed AND contains(tags, "followup") AND due <= date(today) + dur(14 days)
GROUP BY file.link
```

## Overdue Tasks


````ad-important
collapse: closed
```dataview
TASK  
WHERE due < date(today) AND !completed
SORT due asc
````

## Upcoming Meetings

```dataview
TABLE startdate AS "Date", start_time AS "Time", location AS "Location", summary AS "Topic"
FROM "100_Network/Meetings"
WHERE status = "planned"
SORT startdate ASC, start_time ASC
LIMIT 5
```

---

# Tasks
 

```ad-important
title: This Week
collapse: closed
```dataview 

TASK 
WHERE !completed AND due.week = date(today).week 
SORT due ASC 
GROUP BY due

```



```ad-tldr
title: Inbox
collapse: closed
```dataview

TASK
WHERE !completed AND !scheduled AND !due
SORT file.name asc
LIMIT 10

```


```ad-tldr
title: This Month
collapse: closed
```dataview 
TASK 
WHERE !completed AND due.month = date(today).month  
SORT due asc
GROUP BY due
```


---

# Completed Submissions


```dataview
TABLE target_company as "Startup", date_created as "Submitted Date"
FROM "100_Network/Scout Submissions"
WHERE submission_status = "Submitted"
SORT date_created DESC
LIMIT 5
```

```ad-check
title: Last 10 Completed Tasks
collapse: closed
```dataview
TASK 
WHERE completed AND due.year = date(today).year
LIMIT 10
SORT file.cdate asc
````
