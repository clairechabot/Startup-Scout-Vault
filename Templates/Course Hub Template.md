---
course_name:
semester:
professor:
status: ongoing
progress: 0
tags:
  - course
  - MOC
date_created: <% tp.date.now("YYYY-MM-DD") %>
---

[[Course MOC]]

# Course Hub
> [!NOTE] Class Control
> **Status:** `INPUT[inlineSelect(option(ongoing), option(completed), option(planned)):status]` | **Progress:** `INPUT[slider(minValue(0), maxValue(100)):progress]` `VIEW[{progress}]`%

```meta-bind-button
label: 📝 New Note for this Class
icon: "plus"
hidden: false
class: ""
tooltip: "Create a note linked to this course"
id: "new-class-note"
style: primary
actions:
  - type: templaterCreateNote
    templateFile: Templates/Study Note.md
    folderPath: 300_Knowledge Base/Classes
    fileName: <% tp.file.title %> Note - <% tp.date.now("YYYY-MM-DD") %> 
    openNote: true

```

# Resources 

```ad-note
title: Links 
collapse: open



```

# Lecture Notes 

```dataview 
TABLE summary AS "Summary", date AS "Lecture Date"
FROM "300_Knowledge Base/Classes"
WHERE course = this.file.link
SORT date desc
```

---

# Assignments 

```dataview 

TASK 
FROM "300_Knowledge Base/Classes"
WHERE !completed AND contains(file.outlinks, [[]])
SORT due asc

```