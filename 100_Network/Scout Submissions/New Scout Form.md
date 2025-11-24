---
type: scout_submission
target_company:
submission_status: Draft
tags:
  - submission
  - scout
date_created: 2025-11-24
team_score: 1
value_prop_score: 0
traction_score: 0
market_score: 0
sources: []
high_scoring_areas: []
raising: Yes
---

[[undefined]]
%% Links back to the main Company Page %%

# Submission Draft: New Scout Form

> [!NOTE] Actions
> **Status:** `INPUT[inlineSelect(option(Draft), option(Ready), option(Submitted)):submission_status]`
>  [Click here to open Airtable](https://airtable.com/app8pOOPifhS6GORl/pagzEa0tdLFg1kAsc/form)

<br> 

---

# 1. General Info

**Startup Name:**
`INPUT[text:target_company]`

**Location:**
`INPUT[text:location]`

**Link to Website:**
`INPUT[text:website]`

**Link to Pitch Deck / Docs:**
`INPUT[text:deck_link]`

**Funding Status:**
`INPUT[inlineSelect(option(Pre-Seed), option(Seed), option(Series A), option(Bootstrap), option(Undisclosed)):funding_status]`

**Sector:**
`INPUT[text:sector]`

**Origin of the Startup:**
> _Where does the idea come from? (University, previous job, hackathon?)_
```meta-bind
INPUT[textArea:origin_story]
```

**Where did you find this startup?** `INPUT[text:source_context]`

<br> 

---

# 2. Fundraising Details 

**Currently Raising?** `INPUT[inlineSelect(option(Yes), option(No)):raising]` **Round Type:** `INPUT[inlineSelect(option(Pre-seed), option(Seed), option(Series A), option(Bridge)):round_type]` **Amount:** `INPUT[text:raise_amount]` **Closing Date:** `INPUT[datePicker:round_close]`

**Details of the Round:**
 _Valuation, committed investors, cap table notes, use of funds._
> `INPUT[textArea:round_details]`

<br> 

---

# 3. Founding Team 

**Team Overview**
_Names, Roles, Backgrounds..._

`INPUT[textArea:team_overview]`

**Team Highlights**

`INPUT[textArea:team_highlights]`

**Contact Info:** `INPUT[text:founder_contact]`

**⭐ Team Score (1-5):** `INPUT[slider(minValue(1), maxValue(5)):team_score]` `VIEW[{team_score}]`/5

<br> 

--- 

# 4. Problem & Solution 

**Problem:**

`INPUT[textArea:problem]`

**Solution:**

`INPUT[textArea:solution]`


**Unique Selling Proposition (USP):**

`INPUT[textArea:usp]`


**⭐ Value Prop Score (1-5):** `INPUT[slider(minValue(1), maxValue(5)):value_prop_score]` `VIEW[{value_prop_score}]`/5

<br> 

--- 

# 5. Product & Traction

**Product Status:** `INPUT[inlineSelect(option(Idea), option(MVP), option(Live), option(Scaling)):product_status]`

**Current Traction:**

> _User numbers, revenue, pilots, LOIs, partnerships._

`INPUT[textArea:traction]`


**⭐ Traction Score (1-5):** `INPUT[slider(minValue(1), maxValue(5)):traction_score]` `VIEW[{traction_score}]`/5

<br> 

---

# 6. Market & Competition

**Target Market:**


`INPUT[textArea:target_market]`


**Market Size / Opportunity:**



`INPUT[textArea:market_size]`


**Key Competitors:**

`INPUT[textArea:competitors]`

**⭐ Market Score (1-5):** `INPUT[slider(minValue(1), maxValue(5)):market_score]` `VIEW[{market_score}]`/5

---

# 7. Why This Deal? (Analysis)

**Top 1-2 reasons to discuss this deal:**


`INPUT[textArea:top_reasons]`


**High-Scoring Areas (Add items):**

**High-Scoring Areas:**
```meta-bind
INPUT[list:high_scoring_areas]
```


_Comments on High Scores:_ `INPUT[textArea:high_score_comments]`

**🚩 Any red flags / concerns?**

`INPUT[textArea:red_flags]`

---

# 8. Scout's Hypothesis

**Investment Thesis:**

> _Why is this a strong opportunity for Ellipsis?_

`INPUT[textArea:thesis]`


**Would you invest?** `INPUT[inlineSelect(option(Yes), option(Maybe), option(No)):invest_verdict]`

**Personal Opinion:**

`INPUT[textArea:personal_opinion]`