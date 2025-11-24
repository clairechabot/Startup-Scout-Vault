---
banner: "![[neon_definitions.jpg]]"
mapofcontent: yes
title: The Codex
moc_type: index
banner: "![[neon_definitions.jpg]]"
banner_y: 0.32
tags: [MOC, definitions, glossary]
---

[[ellipsis dashboard]]
%% [[Definition Template]] %% 

```ad-quote
_"The limits of my language mean the limits of my world."_

```


> [!col]
>
> > [!col-md]
> >
> > ```meta-bind-button
> > label: New Definition
> > icon: "book"
> > hidden: false
> > class: ""
> > tooltip: "Create a new term"
> > id: "btn-new-def"
> > style: primary
> > actions:
> >   - type: templaterCreateNote
> >     templateFile: "Templates/Definition Template.md"
> >     folderPath: "300_Knowledge Base/Definitions"
> >     fileName: New Definition
> >     openNote: true
> > ```




```dataviewjs

// --- CODEX STATS ---
const pages = dv.pages('"300_Knowledge Base/Definitions"');

// Categories
const vc = pages.where(p => (p.category ?? "").includes("VC")).length;
const ai = pages.where(p => (p.category ?? "").includes("AI")).length;
const finance = pages.where(p => (p.category ?? "").includes("Finance")).length;
const startup = pages.where(p => (p.category ?? "").includes("Startup")).length;
const total = pages.length;

// --- RANDOM WORD GENERATOR ---
// Pick a random page from the collection
let randomLink = "**No terms yet!**";
if (pages.length > 0) {
    const randomPage = pages[Math.floor(Math.random() * pages.length)];
    
    // ADDED CHECK: Ensure the page object and its file properties are valid
    if (randomPage && randomPage.file) { 
        // This is the correct way to construct an internal link in DataviewJS HTML
        randomLink = `<a class="internal-link" data-href="${randomPage.file.path}">${randomPage.file.basename}</a>`;
    } else {
        // Fallback for corrupted or invalid file entry
        randomLink = "**[Error Reading File]**";
    }
}

// --- RENDER DASHBOARD ---
const html = `
<div style="display: flex; gap: 14px; margin-bottom: 24px; flex-wrap: wrap; align-items: stretch;">

  <div style="flex: 1; min-width: 100px; padding: 15px; border-radius: 12px; border: 1px solid rgba(0, 255, 255, 0.5); background: rgba(0, 255, 255, 0.05); text-align: center; box-shadow: 0 0 10px rgba(0, 255, 255, 0.1);">
    <div style="font-size: 24px; font-weight: 800; color: #00ffff; line-height: 1; margin-bottom: 5px;">${vc}</div>
    <div style="font-size: 10px; text-transform: uppercase; letter-spacing: 1px; opacity: 0.8; color: #fff;">VC Lingo</div>
  </div>

  <div style="flex: 1; min-width: 100px; padding: 15px; border-radius: 12px; border: 1px solid rgba(255, 0, 255, 0.5); background: rgba(255, 0, 255, 0.05); text-align: center; box-shadow: 0 0 10px rgba(255, 0, 255, 0.1);">
    <div style="font-size: 24px; font-weight: 800; color: #ff00ff; line-height: 1; margin-bottom: 5px;">${ai}</div>
    <div style="font-size: 10px; text-transform: uppercase; letter-spacing: 1px; opacity: 0.8; color: #fff;">AI Tech</div>
  </div>

  <div style="flex: 1; min-width: 100px; padding: 15px; border-radius: 12px; border: 1px solid rgba(255, 215, 0, 0.5); background: rgba(255, 215, 0, 0.05); text-align: center;">
    <div style="font-size: 24px; font-weight: 800; color: #ffd700; line-height: 1; margin-bottom: 5px;">${finance}</div>
    <div style="font-size: 10px; text-transform: uppercase; letter-spacing: 1px; opacity: 0.8; color: #fff;">Finance</div>
  </div>

  <div style="flex: 1; min-width: 100px; padding: 15px; border-radius: 12px; border: 1px solid rgba(160, 160, 160, 0.5); background: rgba(160, 160, 160, 0.05); text-align: center;">
    <div style="font-size: 24px; font-weight: 800; color: #a0a0a0; line-height: 1; margin-bottom: 5px;">${total}</div>
    <div style="font-size: 10px; text-transform: uppercase; letter-spacing: 1px; opacity: 0.8; color: #fff;">Total Terms</div>
  </div>

</div>

<div style="padding: 15px; border-radius: 8px; border-left: 4px solid #00ffff; background: linear-gradient(90deg, rgba(0,255,255,0.1) 0%, transparent 100%); display: flex; align-items: center; justify-content: space-between;">
  <div>
    <span style="font-size: 12px; opacity: 0.7; text-transform: uppercase;">🎲 Random Term:</span>
    <span style="font-size: 18px; font-weight: bold; margin-left: 10px;">${randomLink}</span>
  </div>
</div>
`;

dv.paragraph(html);
```


---


# Glossary A-Z


> [!example]- 🏦 Venture Capital 
> ```dataview
> TABLE aliases AS "Aliases"
> FROM "300_Knowledge Base/Definitions"
> WHERE contains(category, "VC")
> SORT lower(term) ASC
> ```

> [!example]- 🤖 AI & Tech 
> ```dataview
> TABLE aliases AS "Aliases"
> FROM "300_Knowledge Base/Definitions"
> WHERE contains(category, "AI")
> SORT lower(term) ASC
> ```

> [!example]- 📈 Finance 
> ```dataview
> TABLE aliases AS "Aliases"
> FROM "300_Knowledge Base/Definitions"
> WHERE contains(category, "Finance")
> SORT lower(term) ASC
> ```

> [!example]- 🚀 Startup
> ```dataview
> TABLE aliases AS "Aliases"
> FROM "300_Knowledge Base/Definitions"
> WHERE contains(category, "Startup")
> SORT lower(term) ASC
> ```

> [!example]- 📂 Other / Uncategorized
> ```dataview
> TABLE category AS "Category"
> FROM "300_Knowledge Base/Definitions"
> WHERE contains(category, "Other") OR !category
> SORT lower(term) ASC
> ```

> [!example]- 📚 View All Terms 
> ```dataview
> TABLE category AS "Category", aliases AS "Aliases"
> FROM "300_Knowledge Base/Definitions"
> WHERE contains(tags, "definition")
> SORT lower(term) ASC
> ```