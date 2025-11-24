---
location: 
Hours_Worked: 
tags:
  - work_note
Icon: 
pomodoros: 
excalidraw-open-md: true
created: <% tp.date.now("YYYY-MM-DD") %>
---

[[ellipsis dashboard]] | [[Work Notes MOC]]

---

<< [[100_Network/Work Notes/<%tp.date.now("YYYY", -1) %>/<% tp.date.now("MM-MMMM", -1) %>/<% tp.date.now("YYYY-MM-DD-dddd", -1) %> | Yesterday]] | [[100_Network/Work Notes/<%tp.date.now("YYYY", 1) %>/<% tp.date.now("MM-MMMM", 1) %>/<% tp.date.now("YYYY-MM-DD-dddd", 1) %> | Tomorrow]] >>

___

### This Day Last Year 

![[<% tp.date.now("YYYY-MM-DD", "P-1Y") %>]]

---


## To-Do List 
<br> 

- 



<br> 
<br>

________

# Notes
<br> 

```meta-bind-button
label: Scout Submission
icon: ""
style: primary
class: ""
cssStyle: ""
backgroundImage: ""
tooltip: ""
id: ""
hidden: false
actions:
  - type: open
    link: https://airtable.com/app8pOOPifhS6GORl/pagTfjPv5mh49WoGk/form
    newTab: true

```

## Today's Meetings



## New Contacts / Companies




## Daily Notes





<br> 
<br>

---


### Notes created today
```dataview
List 
FROM "" 
WHERE file.cday = this.file.day
SORT file.ctime asc
```

<br> 

### Notes last touched today
```dataview
List 
FROM "" 
WHERE file.mday = this.file.day
SORT file.mtime asc
```

