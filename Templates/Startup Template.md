---
company:
sector:
subsector:
location:
founded:
stage:
funding:
last_round:
deal_status: Screening
deal_temperature:
scout_form: false
summary:
investment_thesis:
date_created: <% tp.date.now("YYYY-MM-DD") %>
next_action:
next_action_date:
related_people: []
related_firms: []
investor_types: []
sector_tags: []
competitors: []
investors:
tags:
  - company
  - deal
---

[[ellipsis dashboard]]

```ad-note
title: Status Progression Reference
collapse: closed
**deal_status:** Screening → Call Scheduled → In DD → Active → Ongoing → Invested | Paused | Passed
**deal_temperature:** Hot (act now) | Warm (good fit, timing TBD) | Cold (monitor)

Temperature is independent of status — a Screening deal can be Hot.
See [[Status Standards]] for definitions of every value.
```

# Deal Overview

> [!NOTE] Quick Actions
> **Status:** `INPUT[inlineSelect(option(Screening), option(Call Scheduled), option(In DD), option(Active), option(Ongoing), option(Invested), option(Paused), option(Passed)):deal_status]`
> **Temperature:** `INPUT[inlineSelect(option(Hot), option(Warm), option(Cold)):deal_temperature]`
> **Scout Form Sent:** `INPUT[toggle:scout_form]`
> **Investor Types:** `INPUT[multiSelect(option(VC), option(Angel), option(Grant), option(Corporate)):investor_types]`

## Scout Submission

```dataview
TABLE submission_status as "Status", date_created as "Drafted"
FROM "100_Network/Scout Submissions"
WHERE contains(target_company, this.file.link) OR contains(target_company, this.file.name)
```

<br>

---

# 1. Summary

> [!abstract] One-Liner _(Paste the one-sentence pitch here)_

**Thesis / My Take:** _Why is this exciting? (or why is it confusing?)_

<br>

---

# 2. Team (Key Priority)

> _"Bet on the jockey, not the horse."_

- **Founders:** (Backgrounds, Exits, Technical/Sales split)

- **Key Hires:**

- **Advisors:**

## Related Contacts

```dataviewjs
// Show people linked in related_people field, with role lookup from People/
const linked = (dv.current().related_people ?? []);
const people = dv.pages('"100_Network/People"');

if (linked.length === 0) {
    dv.paragraph("_No linked contacts yet — add [[Person Name]] links to the `related_people` frontmatter field._");
} else {
    const rows = linked.map(ref => {
        const name = ref.path ? ref.path.split("/").pop() : ref.toString();
        const person = people.where(p => p.file.name === name).first();
        if (!person) return [ref, "-", "-", "-"];
        return [
            person.file.link,
            person.role ?? "-",
            person.company ?? "-",
            person.relationship_status ?? "New"
        ];
    });
    dv.table(["Person", "Role", "Org", "Relationship"], rows);
}
```

_Also queried from People notes that list this company:_

```dataview
TABLE role AS "Role", relationship_status AS "Status"
FROM "100_Network/People"
WHERE contains(company, this.file.link) OR contains(company, this.file.name)
```

<br>

---

# 3. Traction & Product

**Metrics**

- **Revenue:** (ARR / MRR)

- **Growth:** (MoM / YoY)

- **Users/Pilots:**

**Product**

- **Core Problem:**

- **Solution:**

- **Moat/Defensibility:**

<br>

---

# 4. Market & Landscape

## Market Size

- TAM/SAM/SOM:

## Sector Context

```dataviewjs
// Surface relevant Knowledge Base entries by sector_tags and sector name
const tags    = (dv.current().sector_tags ?? []).map(t => t.toLowerCase());
const sector  = (dv.current().sector ?? "").toLowerCase();

const defs     = dv.pages('"300_Knowledge Base/Definitions"');
const summaries = dv.pages('"300_Knowledge Base/Sector summaries"');

const matchDef = defs.where(d => {
    const dTags = (d.tags ?? []).map(t => t.toLowerCase());
    return tags.some(t => dTags.includes(t)) || (sector && d.file.name.toLowerCase().includes(sector));
});

const matchSum = summaries.where(s =>
    tags.some(t => s.file.name.toLowerCase().includes(t)) ||
    (sector && s.file.name.toLowerCase().includes(sector))
);

const allRows = [
    ...matchDef.map(d => [d.file.link, "Definition", (d.tags ?? []).join(", ")]),
    ...matchSum.map(s => [s.file.link, "Sector Summary", ""])
];

if (allRows.length === 0) {
    dv.paragraph("_No matching Knowledge Base entries. Add tags to `sector_tags` (e.g. `[ai, infrastructure]`) or check spellings._");
} else {
    dv.table(["Entry", "Type", "Tags"], allRows);
}
```

## Competitive Landscape

### Direct Competitors

```dataview
TABLE stage AS "Stage", funding AS "Funding", deal_status AS "Our Status", deal_temperature AS "Temp"
FROM "100_Network/Startups"
WHERE contains(this.competitors, file.link)
```

### Similar Deals in Vault _(same sector)_

```dataview
TABLE deal_status AS "Status", stage AS "Stage", deal_temperature AS "Temp", investor_types AS "Investor Types"
FROM "100_Network/Startups"
WHERE sector = this.sector AND file.name != this.file.name
SORT deal_status ASC
```

### Cross-References _(who lists this as a competitor)_

```dataview
TABLE stage AS "Stage", funding AS "Funding", deal_temperature AS "Temp"
FROM "100_Network/Startups"
WHERE contains(competitors, this.file.link) OR contains(competitors, this.file.name)
```

### Related Firms

```dataviewjs
// Show VC / investor firms linked in related_firms field
const linked = (dv.current().related_firms ?? []);
const people = dv.pages('"100_Network/People"');

if (linked.length === 0) {
    dv.paragraph("_No linked firms yet — add [[Firm Name]] links to the `related_firms` frontmatter field._");
} else {
    const rows = linked.map(ref => {
        const name = ref.path ? ref.path.split("/").pop() : ref.toString();
        const firm = people.where(p => p.file.name === name).first();
        if (!firm) return [ref, "-", "-"];
        return [
            firm.file.link,
            firm.role ?? firm.firm_type ?? "-",
            firm.stage_focus ?? "-"
        ];
    });
    dv.table(["Firm", "Type", "Stage Focus"], rows);
}
```

<br>

---

# 5. Deal Dynamics

- **Total Raised:**

- **Current Round:** (Target Amount)

- **Valuation:** (Cap / Pre-money)

- **Key Investors:**

> [!failure] Risks & Red Flags
> -
> -

<br>

---

# 6. Next Steps

**Next Action:** `INPUT[text:next_action]` **Due:** `INPUT[datePicker:next_action_date]`

- [ ]

<br>

---

# Reference Log

## Meetings

```dataview
TABLE startdate AS "Date", summary AS "Summary"
FROM "100_Network/Meetings"
WHERE contains(company, this.file.link) OR contains(company, this.file.name)
SORT startdate DESC
```
