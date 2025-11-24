---
course_name: History of Scientific Revolutions
semester: Fall 2025
professor: Albert Einstein
status: ongoing
progress: 0
tags:
  - course
  - MOC
date_created: 2025-11-23
---

[[Course MOC]]

# Course Hub
> [!NOTE] Class Control
> **Status:** `INPUT[inlineSelect(option(ongoing), option(completed), option(planned)):status]` | **Progress:** `INPUT[slider(minValue(0), maxValue(100)):progress]` `VIEW[{progress}]`%

```meta-bind-button
label: 📝 New Note for this Class
icon: "plus"
tooltip: "Create a note linked to this course"
id: "new-class-note"
style: primary
actions:
  - type: templaterCreateNote
    templateFile: Templates/Study Note.md
    folderPath: 300_Knowledge Base/Classes
    fileName: History of Scientific Revolutions Note - <% tp.date.now("YYYY-MM-DD") %> 
    openNote: true
```

# Resources

```ad-note
title: Links
collapse: open
- Primary Textbook: *The Structure of Scientific Revolutions*  
- Supplemental Readings: Notes from Newton, Galileo, Curie archives
```

# Lecture Notes

```dataview
TABLE summary AS "Summary", Date AS "Lecture Date"
FROM "300_Knowledge Base/Classes"
WHERE course = this.file.link
SORT Date desc


```

---

# Assignments

```dataview
TASK 
FROM "300_Knowledge Base/Classes"
WHERE !completed AND contains(file.outlinks, [[]])
SORT due asc

```
