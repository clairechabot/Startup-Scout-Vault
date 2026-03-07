---
mapofcontent: yes
title: Network MOC
moc_type: index
tags: [MOC, network, overview]
---

[[ellipsis dashboard]]

```ad-quote
_"Your network is your net worth."_
```

> [!NOTE] Navigation
> This is the top-level network overview. For detailed people tracking, use [[People MOC]]. For meetings, use [[Meetings MOC]].

<br>

```dataviewjs
// --- NETWORK OVERVIEW STATS ---
const people   = dv.pages('"100_Network/People"');
const startups = dv.pages('"100_Network/Startups"');
const meetings = dv.pages('"100_Network/Meetings"')
    .where(p => p.file.name !== "Meeting Template" && p.file.name !== "First Meeting Template");
const events   = dv.pages('"100_Network/Events"');

const html = `
<div style="display: flex; gap: 14px; margin-bottom: 24px; flex-wrap: wrap; align-items: stretch;">

  <div style="flex: 1; min-width: 120px; padding: 20px; border-radius: 12px; border: 1px solid rgba(0, 255, 255, 0.5); background: rgba(0, 255, 255, 0.05); text-align: center; box-shadow: 0 0 10px rgba(0, 255, 255, 0.1);">
    <div style="font-size: 28px; font-weight: 800; color: #00ffff; line-height: 1; margin-bottom: 8px;">${people.length}</div>
    <div style="font-size: 11px; text-transform: uppercase; letter-spacing: 2px; opacity: 0.8; color: #fff;">People</div>
  </div>

  <div style="flex: 1; min-width: 120px; padding: 20px; border-radius: 12px; border: 1px solid rgba(255, 0, 255, 0.5); background: rgba(255, 0, 255, 0.05); text-align: center; box-shadow: 0 0 10px rgba(255, 0, 255, 0.1);">
    <div style="font-size: 28px; font-weight: 800; color: #ff00ff; line-height: 1; margin-bottom: 8px;">${startups.length}</div>
    <div style="font-size: 11px; text-transform: uppercase; letter-spacing: 2px; opacity: 0.8; color: #fff;">Startups</div>
  </div>

  <div style="flex: 1; min-width: 120px; padding: 20px; border-radius: 12px; border: 1px solid rgba(255, 215, 0, 0.5); background: rgba(255, 215, 0, 0.05); text-align: center; box-shadow: 0 0 10px rgba(255, 215, 0, 0.1);">
    <div style="font-size: 28px; font-weight: 800; color: #ffd700; line-height: 1; margin-bottom: 8px;">${meetings.length}</div>
    <div style="font-size: 11px; text-transform: uppercase; letter-spacing: 2px; opacity: 0.8; color: #fff;">Meetings</div>
  </div>

  <div style="flex: 1; min-width: 120px; padding: 20px; border-radius: 12px; border: 1px solid rgba(160, 160, 160, 0.5); background: rgba(160, 160, 160, 0.05); text-align: center; box-shadow: 0 0 10px rgba(160, 160, 160, 0.1);">
    <div style="font-size: 28px; font-weight: 800; color: #a0a0a0; line-height: 1; margin-bottom: 8px;">${events.length}</div>
    <div style="font-size: 11px; text-transform: uppercase; letter-spacing: 2px; opacity: 0.8; color: #fff;">Events</div>
  </div>

</div>
`;
dv.paragraph(html);
```

---

# People Directory

```dataview
TABLE role AS "Role", company AS "Org", relationship_status AS "Status", sector_focus AS "Sector"
FROM "100_Network/People"
SORT relationship_status ASC, file.name ASC
```

---

# Events & Conferences

```dataview
TABLE date AS "Date", location AS "Location", organizer AS "Host"
FROM "100_Network/Events"
SORT date DESC
```
