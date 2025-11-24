---
type: research_paper
title: "{{title}}"
authors: [{{authors}}]
year: {{date | format("YYYY")}}
publication: "{{publicationTitle}}"
url: {{url}}
zotero_link: {{pdfZoteroLink}}
tags: [research, paper]
date_created: <% tp.date.now("YYYY-MM-DD") %>
related_startup: 
related_people: 
---

[[500_Resources/Research/Paper Notes/Research MOC]]

# {{title}}

> [!abstract] Abstract
> {{abstractNote}}

---

## Key Concepts & Notes
_Summary of my findings._

- 

---

## Highlights & Annotations

{% for annotation in annotations -%}
{%- if annotation.annotatedText -%}
> [!quote] {{annotation.annotatedText}}
> [Page {{annotation.page}}](zotero://open-pdf/library/items/{{annotation.attachment.itemKey}}?page={{annotation.page}}&annotation={{annotation.id}})
{%- if annotation.comment %}
> **Note:** {{annotation.comment}}
{%- endif %}

{%- endif -%}
{%- if annotation.imageRelativePath -%}
![[{{annotation.imageRelativePath}}]]
> **Figure:** {{annotation.comment}}
{%- endif -%}
{% endfor -%}