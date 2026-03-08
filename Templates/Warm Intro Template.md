---
type: warm_intro
date_created: <% tp.date.now("YYYY-MM-DD") %>
founder:
startup:
investor:
intro_status: Draft
outcome: Pending
follow_up_date:
tags:
  - intro
  - network
---

[[People MOC]]

```ad-note
title: How to use this template
collapse: closed
1. Fill in the three frontmatter links below — `founder`, `startup`, `investor`
2. All context blocks auto-populate once those links are set
3. Edit the generated email draft before sending
4. Update `intro_status` and `outcome` as the intro progresses
```

# Warm Intro

> [!NOTE] Logistics
> **Founder:** `INPUT[suggester(optionQuery("100_Network/People"), optionQuery("100_Network/Startups")):founder]`
> **Startup:** `INPUT[suggester(optionQuery("100_Network/Startups")):startup]`
> **Investor:** `INPUT[suggester(optionQuery("100_Network/People")):investor]`
> **Status:** `INPUT[inlineSelect(option(Draft), option(Sent), option(Connected), option(Outcome Known)):intro_status]`
> **Outcome:** `INPUT[inlineSelect(option(Pending), option(Meeting Scheduled), option(Passed), option(Investment)):outcome]`
> **Follow-up:** `INPUT[datePicker:follow_up_date]`

---

# 1. Context Brief

## Founder & Startup

```dataviewjs
const cur       = dv.current();
const fLink     = cur.founder;
const sLink     = cur.startup;
const founder   = fLink  ? dv.page(fLink.path  ?? fLink.toString())  : null;
const startup   = sLink  ? dv.page(sLink.path  ?? sLink.toString())  : null;

if (!founder && !startup) {
    dv.callout("info", "Not set", "Set `founder` and `startup` in frontmatter to auto-populate this section.");
    return;
}

if (founder) {
    dv.header(3, founder.name ?? founder.file.name);
    dv.table(["Field", "Value"], [
        ["Role",         founder.role             ?? "-"],
        ["Title",        founder.title            ?? "-"],
        ["Location",     founder.location         ?? "-"],
        ["Sector Focus", founder.sector_focus     ?? "-"],
        ["Relationship", founder.relationship_status ?? "-"],
        ["Email",        founder.email            ?? "-"],
        ["LinkedIn",     founder.linkedin         ?? "-"],
    ]);
}

if (startup) {
    dv.header(3, startup.company ?? startup.file.name);
    dv.table(["Field", "Value"], [
        ["Sector",       startup.sector           ?? "-"],
        ["Stage",        startup.stage            ?? "-"],
        ["Funding",      startup.funding          ?? "-"],
        ["Deal Status",  startup.deal_status      ?? "-"],
        ["Temperature",  startup.deal_temperature ?? "-"],
        ["One-Liner",    startup.summary          ?? "-"],
    ]);
    if (startup.investment_thesis) {
        dv.paragraph(`> **Investment Thesis:** ${startup.investment_thesis}`);
    }
}
```

## Investor

```dataviewjs
const cur      = dv.current();
const iLink    = cur.investor;
const investor = iLink ? dv.page(iLink.path ?? iLink.toString()) : null;

if (!investor) {
    dv.callout("info", "Not set", "Set `investor` in frontmatter to auto-populate this section.");
    return;
}

const name = investor.name ?? investor.firm_name ?? investor.file.name;
dv.header(3, name);
dv.table(["Field", "Value"], [
    ["Role / Type",   investor.role ?? investor.firm_type           ?? "-"],
    ["Org",           investor.company ?? investor.firm_name        ?? "-"],
    ["Sector Focus",  investor.sector_focus                         ?? "-"],
    ["Stage Focus",   investor.stage_focus                          ?? "-"],
    ["Location",      investor.location                             ?? "-"],
    ["Relationship",  investor.relationship_status                  ?? "-"],
    ["Email",         investor.email                                ?? "-"],
]);
```

---

# 2. Thesis Fit Analysis

```dataviewjs
const cur      = dv.current();
const startup  = cur.startup  ? dv.page(cur.startup.path  ?? cur.startup.toString())  : null;
const investor = cur.investor ? dv.page(cur.investor.path ?? cur.investor.toString()) : null;

if (!startup || !investor) {
    dv.callout("info", "Requires startup + investor", "Set both `startup` and `investor` in frontmatter.");
    return;
}

const startupSector  = (startup.sector  ?? "").toLowerCase();
const startupStage   = (startup.stage   ?? "").toLowerCase();
const invSectors     = (investor.sector_focus ?? "").toLowerCase();
const invStages      = (investor.stage_focus  ?? "").toLowerCase();

// Check for keyword overlap
const sectorOverlap  = startupSector && invSectors &&
    startupSector.split(/[\s,]+/).some(w => w.length > 2 && invSectors.includes(w));
const stageOverlap   = startupStage  && invStages  &&
    startupStage.split(/[\s,\-]+/).some(w => w.length > 2 && invStages.includes(w));

const icon = b => b ? "✅ Aligned" : "⚠️ Verify";

dv.table(
    ["Dimension", "Startup", "Investor Focus", "Signal"],
    [
        ["Sector", startup.sector ?? "-",   investor.sector_focus ?? "-", icon(sectorOverlap)],
        ["Stage",  startup.stage  ?? "-",   investor.stage_focus  ?? "-", icon(stageOverlap)],
        ["Geo",    startup.location ?? "-", investor.location     ?? "-", "-"],
    ]
);

// Urgency signal
const temp = (startup.deal_temperature ?? "").toLowerCase();
if (temp === "hot") {
    dv.paragraph("🔴 **Hot deal** — time-sensitive. Send this intro this week.");
} else if (temp === "warm") {
    dv.paragraph("🟡 **Warm deal** — good fit, no immediate pressure.");
} else if (temp === "cold") {
    dv.paragraph("🔵 **Cold deal** — file for later unless investor asks.");
}

// Overall fit verdict
const fit = sectorOverlap && stageOverlap;
dv.paragraph(fit
    ? "**Overall: Strong fit.** Both sector and stage align. This intro is worth making."
    : "**Overall: Partial fit.** Confirm thesis match before sending — add a note in Key Points explaining why this still works."
);
```

---

# 3. Your Relationship Context

```dataviewjs
const cur        = dv.current();
const fLink      = cur.founder;
const iLink      = cur.investor;
const meetings   = dv.pages('"100_Network/Meetings"')
    .where(p => p.file.name !== "Meeting Template" && p.file.name !== "First Meeting Template");

if (!fLink && !iLink) {
    dv.callout("info", "Not set", "Set `founder` and/or `investor` to pull relationship history.");
    return;
}

// Helper: extract a bare name from a link for fuzzy matching
const bareName = link => {
    if (!link) return "";
    const path = link.path ?? link.toString();
    return path.split("/").pop().replace(".md", "");
};

const matchesPerson = (meeting, name) => {
    if (!name) return false;
    const company = (meeting.company?.toString() ?? "").toLowerCase();
    const attendees = (meeting.attendees ?? []).map(a => (a.path ?? a.toString()).toLowerCase());
    return company.includes(name.toLowerCase()) || attendees.some(a => a.includes(name.toLowerCase()));
};

// Founder relationship
if (fLink) {
    const founderName = bareName(fLink);
    const fm = meetings.where(m => matchesPerson(m, founderName)).sort(m => m.startdate, "desc");
    dv.header(4, `History with ${founderName}`);
    if (fm.length === 0) {
        dv.paragraph(`_No logged meetings with ${founderName}. Add context manually below._`);
    } else {
        dv.table(
            ["Date", "Summary", "Status"],
            fm.map(m => [m.startdate ?? "-", m.summary ?? m.file.name, m.status ?? "-"])
        );
    }
}

// Investor relationship
if (iLink) {
    const investorName = bareName(iLink);
    const im = meetings.where(m => matchesPerson(m, investorName)).sort(m => m.startdate, "desc");
    dv.header(4, `History with ${investorName}`);
    if (im.length === 0) {
        dv.paragraph(`_No logged meetings with ${investorName}. Add context manually below._`);
    } else {
        dv.table(
            ["Date", "Summary", "Status"],
            im.map(m => [m.startdate ?? "-", m.summary ?? m.file.name, m.status ?? "-"])
        );
    }
}
```

**How I know the founder:**
-

**How I know the investor:**
-

**Why I trust this connection enough to make it:**
-

---

# 4. Introduction Email Draft

```dataviewjs
// Generate a live smart draft from vault data
const cur      = dv.current();
const founder  = cur.founder  ? dv.page(cur.founder.path  ?? cur.founder.toString())  : null;
const startup  = cur.startup  ? dv.page(cur.startup.path  ?? cur.startup.toString())  : null;
const investor = cur.investor ? dv.page(cur.investor.path ?? cur.investor.toString()) : null;

const f = {
    name:      founder?.name  ?? founder?.file.name ?? "[Founder Name]",
    role:      (founder?.role ?? "Founder").split(",")[0].trim(),
    email:     founder?.email ?? "[founder@email.com]",
};
const s = {
    name:      startup?.company ?? startup?.file.name ?? "[Startup]",
    stage:     startup?.stage    ?? "[stage]",
    funding:   startup?.funding  ?? "[amount raised]",
    summary:   startup?.summary  ?? "[one-liner]",
    temp:      (startup?.deal_temperature ?? "").toLowerCase(),
};
const i = {
    name:      investor?.name ?? investor?.firm_name ?? investor?.file.name ?? "[Investor Name]",
    firstName: (investor?.name ?? investor?.firm_name ?? investor?.file.name ?? "there").split(" ")[0],
    role:      investor?.role ?? investor?.firm_type ?? "investor",
    org:       investor?.company ?? investor?.firm_name ?? "[their firm]",
    sectors:   investor?.sector_focus ?? "[their focus areas]",
    email:     investor?.email ?? "[investor@email.com]",
};

const urgency = s.temp === "hot"
    ? ` They're actively in process right now, so timing is relevant.`
    : "";

const subjectLine = `Intro: ${f.name} (${s.name}) ↔ ${i.name}`;

const draft = [
    `**Subject:** ${subjectLine}`,
    `**To:** ${f.email}, ${i.email}`,
    ``,
    `---`,
    ``,
    `Hi ${i.firstName},`,
    ``,
    `I wanted to connect you with ${f.name}, the ${f.role} of ${s.name} — ${s.summary}`,
    ``,
    `They're at ${s.stage}, have raised ${s.funding}, and are building in a space that maps closely to your focus on ${i.sectors}.${urgency}`,
    ``,
    `${f.name}, meet ${i.name} — ${i.role} at ${i.org}. [One sentence on why they're a strong fit for the startup.]`,
    ``,
    `I'll let you both take it from here, but happy to join a call if that's useful.`,
    ``,
    `Best,`,
    `[Your name]`,
].join("\n");

dv.paragraph(draft);
```

> [!warning] Edit before sending
> The draft above is auto-generated from your vault. Review it for accuracy, add the bracketed placeholders, and personalise the tone before copying to email.

---

# 5. Key Points to Highlight

_The 3 things that make this intro compelling for this specific investor._

**On the founder:**
-

**On the product / traction:**
-

**On the thesis fit:**
-

---

# 6. Potential Objections to Address

```dataviewjs
// Surface the startup's known risks from deal note as a starting point
const cur     = dv.current();
const startup = cur.startup ? dv.page(cur.startup.path ?? cur.startup.toString()) : null;

if (!startup) {
    dv.paragraph("_Set `startup` to see known risks from the deal note._");
    return;
}

// Try to surface investment thesis context
if (startup.investment_thesis) {
    dv.paragraph(`**Your thesis on this deal:** ${startup.investment_thesis}`);
}
dv.paragraph("_Add the likely investor objections in the table below. Check the deal note's Risks & Red Flags section for starting points._");
```

| Objection | Your Response |
|---|---|
| _Too early / no revenue yet_ | |
| _Sector too niche_ | |
| _Team missing X experience_ | |
| | |

---

# 7. Success Metrics

- [ ] **Sent** — Intro email delivered to both parties
- [ ] **Acknowledged** — Both parties reply within 48 h
- [ ] **Meeting scheduled** — Call or coffee booked between founder and investor
- [ ] **Feedback received** — Even a pass with reasoning counts
- [ ] **Outcome logged** — Update `outcome` field above

**What success looks like for this specific intro:**
_E.g. "Even if they don't invest, I want [investor] to be on [founder]'s radar for future rounds."_

---

# 8. Follow-up Timeline

| Day | Action | Done? |
|---|---|---|
| 0 | Send intro email | ☐ |
| 2 | Nudge if no acknowledgement from either side | ☐ |
| 7 | Check in — did a call get scheduled? | ☐ |
| 14 | If no meeting yet — offer to facilitate directly | ☐ |
| 30 | Log final `outcome` and close this note | ☐ |

> Follow-up date set: `VIEW[{follow_up_date}]`

---

# Activity Log

_All meetings and submissions involving either party, for full context._

```dataviewjs
const cur       = dv.current();
const fLink     = cur.founder;
const iLink     = cur.investor;
const sLink     = cur.startup;

const meetings  = dv.pages('"100_Network/Meetings"')
    .where(p => p.file.name !== "Meeting Template" && p.file.name !== "First Meeting Template");
const subs      = dv.pages('"100_Network/Scout Submissions"');

const bareName  = link => link ? (link.path ?? link.toString()).split("/").pop().replace(".md", "") : "";
const fName     = bareName(fLink);
const iName     = bareName(iLink);
const sName     = bareName(sLink);

const nameSet   = [fName, iName, sName].filter(Boolean);

if (nameSet.length === 0) {
    dv.paragraph("_Set frontmatter links to see activity log._");
    return;
}

const matches = m => nameSet.some(n => {
    const company   = (m.company?.toString() ?? "").toLowerCase();
    const attendees = (m.attendees ?? []).map(a => (a.path ?? a.toString()).toLowerCase());
    return company.includes(n.toLowerCase()) || attendees.some(a => a.includes(n.toLowerCase()));
});

const relevantMeetings = meetings.where(matches).sort(m => m.startdate, "desc");

if (relevantMeetings.length > 0) {
    dv.header(3, "Meetings");
    dv.table(
        ["Date", "Meeting", "Company", "Status"],
        relevantMeetings.map(m => [m.startdate ?? "-", m.file.link, m.company ?? "-", m.status ?? "-"])
    );
}

// Submissions related to startup
if (sName) {
    const relatedSubs = subs.where(s =>
        (s.target_company?.toString() ?? "").toLowerCase().includes(sName.toLowerCase())
    );
    if (relatedSubs.length > 0) {
        dv.header(3, "Scout Submissions");
        dv.table(
            ["Submission", "Status", "Score"],
            relatedSubs.map(s => [
                s.file.link,
                s.submission_status ?? "-",
                `${s.team_score ?? "?"}/${s.value_prop_score ?? "?"}/${s.traction_score ?? "?"}/${s.market_score ?? "?"}`
            ])
        );
    }
}
```
