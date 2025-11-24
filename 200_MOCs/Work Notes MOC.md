---
banner: "![[neon_work_notes.jpg]]"
aliases: []
tags: [MOC]
title: Work Notes MOC
date_created: 2024-03-04-Monday
date_modified: 2024-06-17-Monday
banner: "![[neon_work_notes.jpg]]"
banner_y: 0.592
---

[[ellipsis dashboard]]

%%**Template:** [[Daily Note]]%%

---

This page conglomerates all of the Work Notes that are made on a daily basis for my Ph.D. These notes can be linked to [[Network MOC]] as well as [[Meetings MOC]]. 

<br>

```meta-bind-button
label: New Work Note
icon: ""
hidden: false
class: ""
tooltip: ""
id: ""
style: primary
actions:
  - type: command
    command: workspace:new-tab
  - type: command
    command: daily-notes

```



<br>

---


```tracker
searchType: frontmatter
searchTarget: Hours_Worked
folder: 100_Network/Work Notes
dateFormat: "YYYY-MM-DD-dddd"
summary:
    template: "I've recorded {{sum()}} hours of scouting!"
    style: "font-size:30px;color:garnet;margin-left: 50px;margin-top:00px;"
```


---

# List of Work Notes

<br>

## 2025

```dataview
TABLE location AS "Location", Hours_Worked AS "Hours Worked"
FROM "100_Network/Work Notes" and -#MOC
SORT created DESC
WHERE contains(file.name, "2025")
```


