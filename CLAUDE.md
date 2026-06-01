# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

---

# Expert Outreach — Claude Code Instructions

This is a LinkedIn expert outreach tool for Nitya Negi, Digital Innovation & AI, Cartier APAC.

**When this project is opened:**

1. Check for existing campaign folders under `outreach/`. If any are found, list them and ask: "I found existing campaigns: [list]. Would you like to continue one of these, or start a new session?"
   - If continuing: read the existing campaign's files (`candidates.md`, `profiles.md`, `briefs.md`, `outreach.md`) to understand where the campaign left off, summarise current state, and ask what the user wants to do next.
   - If starting new (or no existing campaigns): proceed to step 2.

2. Ask the user these three questions to set up the new session:
   - What topic are you researching for this outreach session?
   - What type of expert are you looking for? (role, seniority, any signals — e.g. "practitioners who've shipped X", "conference speakers on Y", "founders in Z space")
   - What is the outreach goal? (e.g. understand a trend, benchmark against industry, learn from practitioners)

Once answered, create a campaign folder at `outreach/{topic-slug}-{YYYYMMDD}/` and proceed with Phase 1 below.

---

## Workflow

### Phase 1 — Candidate Discovery → `candidates.md`
- Generate 5–8 targeted search queries using `site:linkedin.com/in` + topic keywords + role/seniority signals
- Run them via web search
- Write `outreach/{slug}/candidates.md` as a numbered list:

```
## 1. Name
**Title:** Role, Company
**LinkedIn:** URL
**Why relevant:** 1–2 sentences on fit
**Signal:** specific post, talk, article, or role signal
```

- Show the user the list and ask: "Which candidates do you want to pursue? Reply with the numbers, e.g. keep 1, 3, 5"

---

### Phase 2 — Profile Ingestion → `profiles.md`
- Create `outreach/{slug}/profiles.md` with a section per approved candidate (using their names and LinkedIn URLs from candidates.md as headers)
- Each section has a comment prompt: `<!-- Paste [name]'s LinkedIn profile text below -->`
- Include a clearly marked section at the bottom for the user to add their own people:

```
---
## ➕ Add your own people below
<!-- Add a name and any context you have — role, company, why relevant, any signal you want referenced -->
```

- Tell the user: "Open profiles.md, paste each person's LinkedIn text under their section, add anyone else you'd like to reach out to at the bottom, then tell me 'profiles are ready'."

---

### Phase 3 — Briefs → `briefs.md`
When the user says profiles are ready:
- Read `outreach/{slug}/profiles.md` — process every person (search-found and manually added equally)
- For each person, run web searches: `"{name}" personal website`, `"{name}" blog`, `"{name}" {company}`
- Write `outreach/{slug}/briefs.md` with this structure per person:

```
## Name
**Title:** Role, Company
**LinkedIn:** URL
**Personal website:** URL — or "No personal website found"
**Who they are:** 2–3 sentence summary
**Why relevant:** specific connection to the campaign topic
**Relevance score:** X/10 — one-line reasoning
**Best hook:** the specific thing to reference in the outreach message
```

- After writing, tell the user: "Briefs are ready — review briefs.md and let me know if you want to skip anyone or adjust anything before I draft the messages."

---

### Phase 4 — Message Drafting → `outreach.md`
When the user approves or gives adjustments:
- Read `outreach/{slug}/profiles.md` and `outreach/{slug}/briefs.md` for context on each person
- Read `examples/outreach_examples.txt` and mimic the tone, length, and structure of those messages closely
- Draft a personalised message for each person using the brief's best hook and the campaign topic/goal from Phase 1
- Sign off consistently as: Nitya Negi / Digital Innovation & AI / Cartier-HK APAC

Write `outreach/{slug}/outreach.md` with one entry per person:

```
## Name
**LinkedIn:** URL
**Personal website:** URL (or "No personal website found")

**Message:**
[drafted message]
```

---

### Phase 5 — Quick Add → appends to `briefs.md` and `outreach.md`
For adding new people to an existing campaign at any time — no need to restart the full flow.

**Setup:** When a campaign folder is created, also create `outreach/{slug}/quick_add.md` with the template below. The user pastes LinkedIn profiles into it whenever they find new people to reach out to.

**Trigger:** When the user says "run quick_add" (or "run quick_add for [campaign-slug]"):
- Read `outreach/{slug}/quick_add.md`
- Read the existing `outreach/{slug}/briefs.md` and `outreach/{slug}/outreach.md` for campaign context (topic, context, previous entries)
- For each person in quick_add.md: run web searches for their personal website, generate a brief, draft an outreach message using the same Phase 4 rules
- Append new briefs to `briefs.md` and new messages to `outreach.md`
- Tell the user which people were processed

**quick_add.md template:**
```
# Quick Add — [campaign-slug]

Paste LinkedIn profiles below, one section per person.
Tell Claude Code "run quick_add" when ready.

---

## [Name]
<!-- Paste LinkedIn profile text below -->

---

## [Name]
<!-- Paste LinkedIn profile text below -->

---
```

---

### Phase 6 — Revision (ongoing)
- If the user says "revise [name]'s message — [note]", edit that person's entry in `outreach.md` in place
- If the user says "regenerate for [name]", rewrite that entry from scratch using the brief
- If the user wants to add another person mid-session, use quick_add.md

---

## File Structure

```
Expert-Outreach/
├── CLAUDE.md                        ← this file
├── examples/
│   └── outreach_examples.txt        ← style reference (read in Phase 4)
└── outreach/
    └── {campaign-slug}/
        ├── candidates.md            ← Phase 1 output
        ├── profiles.md              ← Phase 2: user fills in
        ├── briefs.md                ← Phase 3 output
        └── outreach.md              ← Phase 4 output
        └── quick_add.md             ← Phase 5: drop profiles in anytime, run on demand
```
