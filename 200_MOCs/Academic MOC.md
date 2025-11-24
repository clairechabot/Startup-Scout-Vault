---
mapofcontent: yes
title: The Academy
moc_type: index
banner: "![[library.jpg]]"
banner_y: 0.544
tags: [MOC, knowledge, learning]
---

[[ellipsis dashboard]]
%% [[Course Hub Template]] %%

```ad-quote
_"The beautiful thing about learning is that no one can take it away from you."_


```

<br> 


> [!col]
>
>>[!col-md]
>>
>>```meta-bind-button
>>label: New Course / Subject
>>icon: "book-open"
>>hidden: false
>>class: ""
>>tooltip: "Create a new Course Hub"
>>id: "btn-new-course"
>>style: primary
>>actions:
>>  - type: templaterCreateNote
>>    templateFile: Templates/Course Hub Template.md
>>    folderPath: 300_Knowledge Base/Classes
>>    fileName: New-Course-Hub
>>    openNote: true
>>```
>
>>[!col-md]
>>
>>```meta-bind-button
>>label: New Sector Summary
>>icon: "bar-chart"
>>hidden: false
>>class: ""
>>tooltip: "Create a market analysis"
>>id: "btn-new-sector"
>>style: primary
>>actions:
>>  - type: templaterCreateNote
>>    templateFile: Templates/Sector Summary Template.md
>>    folderPath: 300_Knowledge Base/Sector summaries
>>    fileName: New-Sector-Summary
>>    openNote: true
>>```


<br> 
<br> 


```dataviewjs

// --- ACADEMY STATS ---
const pages = dv.pages('"300_Knowledge Base/Classes"');
const courses = pages.where(p => (p.tags ?? []).includes("course"));
const notes = pages.where(p => (p.tags ?? []).includes("study-note"));

// Filter Status
const active = courses.where(p => p.status == "ongoing").length;
const completed = courses.where(p => p.status == "completed").length;
const total_notes = notes.length;

// Tasks (Homework)
const tasks = pages.file.tasks.where(t => !t.completed).length;

// --- RENDER DASHBOARD ---
const html = `
<div style="display: flex; gap: 14px; margin-bottom: 24px; flex-wrap: wrap; align-items: stretch;">

  <div style="flex: 1; min-width: 100px; padding: 15px; border-radius: 12px; border: 1px solid rgba(0, 255, 255, 0.5); background: rgba(0, 255, 255, 0.05); text-align: center; box-shadow: 0 0 10px rgba(0, 255, 255, 0.1);">
    <div style="font-size: 24px; font-weight: 800; color: #00ffff; line-height: 1; margin-bottom: 5px;">${active}</div>
    <div style="font-size: 10px; text-transform: uppercase; letter-spacing: 1px; opacity: 0.8; color: #fff;">Active Courses</div>
  </div>

  <div style="flex: 1; min-width: 100px; padding: 15px; border-radius: 12px; border: 1px solid rgba(255, 0, 255, 0.5); background: rgba(255, 0, 255, 0.05); text-align: center; box-shadow: 0 0 10px rgba(255, 0, 255, 0.1);">
    <div style="font-size: 24px; font-weight: 800; color: #ff00ff; line-height: 1; margin-bottom: 5px;">${total_notes}</div>
    <div style="font-size: 10px; text-transform: uppercase; letter-spacing: 1px; opacity: 0.8; color: #fff;">Study Notes</div>
  </div>

  <div style="flex: 1; min-width: 100px; padding: 15px; border-radius: 12px; border: 1px solid rgba(160, 160, 160, 0.5); background: rgba(160, 160, 160, 0.05); text-align: center;">
    <div style="font-size: 24px; font-weight: 800; color: #a0a0a0; line-height: 1; margin-bottom: 5px;">${completed}</div>
    <div style="font-size: 10px; text-transform: uppercase; letter-spacing: 1px; opacity: 0.8; color: #fff;">Completed</div>
  </div>

  <div style="flex: 1; min-width: 100px; padding: 15px; border-radius: 12px; border: 1px solid rgba(255, 215, 0, 0.5); background: rgba(255, 215, 0, 0.05); text-align: center;">
    <div style="font-size: 24px; font-weight: 800; color: #ffd700; line-height: 1; margin-bottom: 5px;">${tasks}</div>
    <div style="font-size: 10px; text-transform: uppercase; letter-spacing: 1px; opacity: 0.8; color: #fff;">Assignments</div>
  </div>

</div>
`;

dv.paragraph(html);

```



# Active Curriculum 

```dataview 

TABLE semester AS "Term", professor AS "Professor", progress AS "Progress"
FROM "300_Knowledge Base/Classes" 
WHERE status = "ongoing" AND contains(tags, "course")
SORT file.name asc

```

<br>

---

# To-Do  

```dataview 

TASK
FROM "300_Knowledge Base/Classes"
WHERE !completed
GROUP BY file.link

```

<br> 

---

# Sector Summaries

> [!example]- 🏢 Enterprise & B2B _(Click to Expand)_
> ```dataview
> TABLE sentiment AS "Sentiment", maturity AS "Maturity", sub_sector AS "Focus"
> FROM "300_Knowledge Base/Sector summaries"
> WHERE contains(category, "Enterprise")
> SORT file.mtime DESC
> ```

> [!example]- 💸 Fintech _(Click to Expand)_
> ```dataview
> TABLE sentiment AS "Sentiment", maturity AS "Maturity", sub_sector AS "Focus"
> FROM "300_Knowledge Base/Sector summaries"
> WHERE contains(category, "Fintech")
> SORT file.mtime DESC
> ```

> [!example]- 🧠 AI & Data _(Click to Expand)_
> ```dataview
> TABLE sentiment AS "Sentiment", maturity AS "Maturity", sub_sector AS "Focus"
> FROM "300_Knowledge Base/Sector summaries"
> WHERE contains(category, "AI") OR contains(category, "Data")
> SORT file.mtime DESC
> ```

> [!example]- 🏥 Health & Bio _(Click to Expand)_
> ```dataview
> TABLE sentiment AS "Sentiment", maturity AS "Maturity", sub_sector AS "Focus"
> FROM "300_Knowledge Base/Sector summaries"
> WHERE contains(category, "Health")
> SORT file.mtime DESC
> ```

> [!example]- 🌍 Climate & Industrials _(Click to Expand)_
> ```dataview
> TABLE sentiment AS "Sentiment", maturity AS "Maturity", sub_sector AS "Focus"
> FROM "300_Knowledge Base/Sector summaries"
> WHERE contains(category, "Climate")
> SORT file.mtime DESC
> ```

> [!example]- 🛍️ Consumer _(Click to Expand)_
> ```dataview
> TABLE sentiment AS "Sentiment", maturity AS "Maturity", sub_sector AS "Focus"
> FROM "300_Knowledge Base/Sector summaries"
> WHERE contains(category, "Consumer")
> SORT file.mtime DESC
> ```

> [!example]- 🔭 All Research _(Click to Expand)_
> ```dataview
> TABLE category AS "Category", sentiment AS "Sentiment", file.mtime AS "Last Updated"
> FROM "300_Knowledge Base/Sector summaries"
> SORT file.mtime DESC
> ```

<br> 

---

# Course Archive 

```dataview 

TABLE semester AS "Term", professor AS "Professor"
FROM "300_Knowledge Base/Classes"
WHERE status = "completed" AND contains(tags, "course")
SORT file.ctime desc

```