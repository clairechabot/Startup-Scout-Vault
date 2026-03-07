---
aliases: [Cockpit]
tags: []
cssclasses:
  - wide-page
title: The Cockpit
date_modified: 2026-03-07
---

> [!col]
>
> > [!col-md]
> >
> > ```meta-bind-button
> > label: Brain Dump
> > icon: "zap"
> > style: primary
> > id: "brain-dump"
> > actions:
> >   - type: command
> >     command: quickadd:choice:Brain Dump
> > ```
>
> > [!col-md]
> >
> > ```meta-bind-button
> > label: New Startup
> > icon: "rocket"
> > style: primary
> > id: "new-startup"
> > actions:
> >   - type: templaterCreateNote
> >     templateFile: Templates/Startup Template.md
> >     folderPath: 100_Network/Startups
> >     fileName: New Startup
> >     openNote: true
> > ```
>
> > [!col-md]
> >
> > ```meta-bind-button
> > label: New Meeting
> > icon: "calendar"
> > style: primary
> > id: "new-meeting"
> > actions:
> >   - type: templaterCreateNote
> >     templateFile: Templates/First Meeting Template.md
> >     folderPath: 100_Network/Meetings
> >     fileName: New Meeting
> >     openNote: true
> > ```
>
> > [!col-md]
> >
> > ```meta-bind-button
> > label: Daily Note
> > icon: "calendar-check"
> > style: primary
> > id: "nav-daily"
> > actions:
> >   - type: command
> >     command: daily-notes
> > ```

---

## Overdue

```dataview
TASK
WHERE !completed AND due < date(today)
SORT due ASC
```

---

## Inbox — Unscheduled

```dataview
TASK
FROM "00_Inbox"
WHERE !completed
SORT file.ctime DESC
LIMIT 15
```

---

## Upcoming Meetings

```dataview
TABLE startdate AS "Date", start_time AS "Time", company AS "Company", summary AS "Topic"
FROM "100_Network/Meetings"
WHERE status = "planned" AND startdate >= date(today)
SORT startdate ASC
LIMIT 7
```

---

## Active Pipeline

```dataview
TABLE deal_status AS "Status", sector AS "Sector", stage AS "Stage"
FROM "100_Network/Startups"
WHERE contains(lower(deal_status), "screening") OR contains(lower(deal_status), "call") OR contains(lower(deal_status), "in dd") OR contains(lower(deal_status), "ongoing") OR contains(lower(deal_status), "active")
SORT file.mtime DESC
```

---

## Follow-ups Due (14 days)

```dataview
TASK
FROM "100_Network"
WHERE !completed AND due AND due <= date(today) + dur(14 days) AND contains(tags, "followup")
SORT due ASC
```
