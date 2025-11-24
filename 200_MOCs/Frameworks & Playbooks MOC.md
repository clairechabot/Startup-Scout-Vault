---
mapofcontent: yes
title: Frameworks & Playbooks
moc_type: index
tags: [MOC, frameworks, strategy]
---

[[ellipsis dashboard]]

```ad-quote
_"A system beats inspiration. Frameworks are repeatable leverage."_

---

> [!col]
>
> > [!col-md]
> >
> > ```meta-bind-button
> > label: New Outreach Template
> > icon: ""
> > hidden: false
> > class: ""
> > tooltip: ""
> > id: ""
> > style: primary
> > actions:
> >   - type: command
> >     command: workspace:new-tab
> >   - type: templaterCreateNote
> >     templateFile: Templates/Framework Template.md
> >     folderPath: 000_Admin/Outreach Templates
> >     fileName: New Outreach Template
> >     openNote: true
> > ```
>
> > [!col-md]
> >
> > ```meta-bind-button
> > label: New Checklist
> > icon: ""
> > hidden: false
> > class: ""
> > tooltip: ""
> > id: ""
> > style: primary
> > actions:
> >   - type: command
> >     command: workspace:new-tab
> >   - type: templaterCreateNote
> >     templateFile: Templates/Framework Template.md
> >     folderPath: 000_Admin/Due Diligence Checklists
> >     fileName: New Checklist
> >     openNote: true
> > ```
>
> > [!col-md]
> >
> > ```meta-bind-button
> > label: New Pitch Framework
> > icon: ""
> > hidden: false
> > class: ""
> > tooltip: ""
> > id: ""
> > style: primary
> > actions:
> >   - type: command
> >     command: workspace:new-tab
> >   - type: templaterCreateNote
> >     templateFile: Templates/Framework Template.md
> >     folderPath: 000_Admin/Frameworks/Pitches
> >     fileName: New Pitch Framework
> >     openNote: true
> > ```
>
> > [!col-md]
> >
> > ```meta-bind-button
> > label: New Screening Framework
> > icon: ""
> > hidden: false
> > class: ""
> > tooltip: ""
> > id: ""
> > style: primary
> > actions:
> >   - type: command
> >     command: workspace:new-tab
> >   - type: templaterCreateNote
> >     templateFile: Templates/Framework Template.md
> >     folderPath: 000_Admin/Frameworks/Screening
> >     fileName: New Screening Framework
> >     openNote: true
> > ```
>
> > [!col-md]
> >
> > ```meta-bind-button
> > label: New Market Map
> > icon: ""
> > hidden: false
> > class: ""
> > tooltip: ""
> > id: ""
> > style: primary
> > actions:
> >   - type: command
> >     command: workspace:new-tab
> >   - type: templaterCreateNote
> >     templateFile: Templates/Market Map Template.md
> >     folderPath: 000_Admin/Market Maps
> >     fileName: New Market Map
> >     openNote: true
> > ```


```

```dataviewjs

// --- TOOLBOX STATS (FOLDER BASED) ---
// Now counting based on the actual folder location to ensure accuracy

const screening = dv.pages('"000_Admin/Frameworks/Screening"').length;
const diligence = dv.pages('"000_Admin/Due Diligence Checklists"').length;
const models = dv.pages('"000_Admin/Mental Models"').length;

// Outreach counts both Templates and Pitches
const outreachTemplates = dv.pages('"000_Admin/Outreach Templates"').length;
const pitchFrameworks = dv.pages('"000_Admin/Frameworks/Pitches"').length;
const outreach = outreachTemplates + pitchFrameworks;

// --- RENDER DASHBOARD ---
const html = `
<div style="display: flex; gap: 14px; margin-bottom: 24px; flex-wrap: wrap; align-items: stretch;">

  <div style="flex: 1; min-width: 100px; padding: 15px; border-radius: 12px; border: 1px solid rgba(0, 255, 255, 0.5); background: rgba(0, 255, 255, 0.05); text-align: center; box-shadow: 0 0 10px rgba(0, 255, 255, 0.1);">
    <div style="font-size: 24px; font-weight: 800; color: #00ffff; line-height: 1; margin-bottom: 5px;">${screening}</div>
    <div style="font-size: 10px; text-transform: uppercase; letter-spacing: 1px; opacity: 0.8; color: #fff;">Screening Tools</div>
  </div>

  <div style="flex: 1; min-width: 100px; padding: 15px; border-radius: 12px; border: 1px solid rgba(255, 0, 255, 0.5); background: rgba(255, 0, 255, 0.05); text-align: center; box-shadow: 0 0 10px rgba(255, 0, 255, 0.1);">
    <div style="font-size: 24px; font-weight: 800; color: #ff00ff; line-height: 1; margin-bottom: 5px;">${diligence}</div>
    <div style="font-size: 10px; text-transform: uppercase; letter-spacing: 1px; opacity: 0.8; color: #fff;">Diligence Checklists</div>
  </div>

  <div style="flex: 1; min-width: 100px; padding: 15px; border-radius: 12px; border: 1px solid rgba(255, 215, 0, 0.5); background: rgba(255, 215, 0, 0.05); text-align: center;">
    <div style="font-size: 24px; font-weight: 800; color: #ffd700; line-height: 1; margin-bottom: 5px;">${models}</div>
    <div style="font-size: 10px; text-transform: uppercase; letter-spacing: 1px; opacity: 0.8; color: #fff;">Mental Models</div>
  </div>

  <div style="flex: 1; min-width: 100px; padding: 15px; border-radius: 12px; border: 1px solid rgba(160, 160, 160, 0.5); background: rgba(160, 160, 160, 0.05); text-align: center;">
    <div style="font-size: 24px; font-weight: 800; color: #a0a0a0; line-height: 1; margin-bottom: 5px;">${outreach}</div>
    <div style="font-size: 10px; text-transform: uppercase; letter-spacing: 1px; opacity: 0.8; color: #fff;">Outreach Templates</div>
  </div>

</div>
`;

dv.paragraph(html);
```


---

# Market Maps 

```dataview 

TABLE WITHOUT ID file.link AS "File", sector AS "Sector", status AS "Status", date_created AS "Created"
FROM "000_Admin/Market Maps"
SORT date_created DESC
```


<br> 

---

# Screening & Selection 

```dataview

TABLE WITHOUT ID file.link AS "File", date_created AS "Added"
FROM "000_Admin/Frameworks/Screening"
SORT file.name ASC

```

<br> 

---

# Due Diligence 

```dataview

TABLE WITHOUT ID file.link AS "File", date_created AS "Added"
FROM "000_Admin/Due Diligence Checklists"
SORT file.name ASC

```

<br> 

---

# Mental Models 

```dataview

TABLE WITHOUT ID file.link AS "File", category AS "Type", date_created AS "Added"
FROM "000_Admin/Mental Models"
SORT file.name ASC


```

<br> 

---

# Outreach & Pitching 

```dataview

TABLE WITHOUT ID file.link AS "File", date_created AS "Added"
FROM "000_Admin/Outreach Templates" OR "000_Admin/Frameworks/Pitches"
SORT file.name ASC

```