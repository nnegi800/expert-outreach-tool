# Expert Outreach Tool

A browser-based LinkedIn expert outreach tool for Cartier Digital Innovation & AI. No installation, no backend, no build step — open `index.html` in any browser and go.

---

## Demo

[Watch how the system works →](https://www.loom.com/share/e2cb2a8c762d446c8a30d57826498110)

---

## What it does

The tool guides you through a structured expert outreach workflow:

1. **Discover candidates** — AI generates search queries and finds relevant experts via web search
2. **Collect profiles** — paste LinkedIn (or company / other) profile text for each person
3. **Generate briefs** — AI summarises who each person is, why they're relevant, and the best opening hook
4. **Draft messages** — AI writes personalised outreach messages in Nitya's voice, either long-form or under 200 characters
5. **Quick Add** — add new people to an existing campaign at any time without restarting

All data is saved in your browser's `localStorage` — no server, no account needed.

---

## How to run

1. Clone or download this repo
2. Open `index.html` directly in Chrome, Safari, or Firefox (`file://` works fine)
3. Enter your API key on the first screen — it is saved in `localStorage` for your browser session and never sent anywhere except the AI provider you choose

---

## `index.html` — structure guide

The entire application lives in a single file. Here is how it is organised:

```
index.html
│
├── <style>          CSS — all styles inline, no external stylesheet
│
├── Screens (HTML)   One <div id="screen-*"> per view:
│   ├── screen-key         API key entry
│   ├── screen-home        Campaign list + start new
│   ├── screen-setup       New campaign setup (topic, expert type, goal)
│   ├── screen-discovery   Candidate discovery (web search + manual add)
│   ├── screen-profiles    Paste LinkedIn profiles, set skip flags
│   ├── screen-briefs      Review generated briefs, skip people
│   ├── screen-messages    Outreach messages (edit, revise, regenerate, copy)
│   └── screen-quickadd    Add new people to an existing campaign
│
└── <script>         All JavaScript — no frameworks, no imports
    │
    ├── PROVIDERS            API config for Anthropic, OpenAI, DeepSeek, Qwen
    ├── OUTREACH_EXAMPLES_LONG    3 full-length example messages (style reference)
    ├── OUTREACH_EXAMPLES_SHORT   4 short (<200 char) example messages (style reference)
    ├── S {}                 Global state object (campaign, provider, api key)
    │
    ├── Persistence
    │   ├── save()           Writes S.campaigns to localStorage
    │   └── load on init     Reads campaigns + preferred provider from localStorage
    │
    ├── API layer
    │   └── callAI(system, user, { useWebSearch })
    │       Routes to the correct provider endpoint.
    │       Anthropic and OpenAI have native web search tools.
    │       DeepSeek and Qwen have no web search — queries are shown for manual use.
    │
    ├── Screen renderers
    │   ├── renderHome()
    │   ├── renderDiscovery()
    │   ├── renderProfiles()
    │   ├── renderBriefs(generateNew)
    │   ├── renderMessages(generateNew)
    │   └── renderQuickAdd()
    │
    ├── AI call functions (one section per phase)
    │   ├── runDiscovery()           Phase 1 — search queries + candidate search
    │   ├── generateAllBriefs()      Phase 3 — brief per person (skips profile-skipped)
    │   ├── generateAllMessages()    Phase 4 — message per person (long or short)
    │   ├── reviseMsg()              Revise a single message with a note
    │   ├── regenMsg()               Regenerate a single message from scratch
    │   └── runQuickAdd()            Quick Add — brief + message for new people
    │
    └── Utility helpers
        ├── esc(str)         HTML-encodes a string for safe use in onclick attributes
        ├── parseJSON(raw)   Strips markdown fences and parses JSON from AI responses
        ├── toId(name)       Converts a person's name to a safe DOM id
        ├── download(fn, md) Triggers a markdown file download
        └── toast(msg)       Shows a brief notification
```

---

## AI providers

| Provider | Web search | Notes |
|----------|-----------|-------|
| Anthropic (Claude) | Yes | Recommended — best brief and message quality |
| OpenAI (GPT) | Yes | Uses `web_search_preview` tool |
| DeepSeek | No | Queries shown for manual search |
| Qwen (Alibaba) | No | Queries shown for manual search |

API keys are stored in `localStorage` per browser. They are never sent anywhere except the provider's own API endpoint.

---

## Style reference

`examples/outreach_examples.txt` contains real outreach messages that worked. The file is embedded as two JavaScript constants at the top of `index.html`:

- `OUTREACH_EXAMPLES_LONG` — 3 multi-paragraph messages, used as the style reference when drafting long messages
- `OUTREACH_EXAMPLES_SHORT` — 4 under-200-character messages, used as the style reference when drafting short messages

To update the style reference, edit the constants directly in `index.html` (search for `OUTREACH_EXAMPLES_LONG`).

---

## Data model

Each campaign is stored as a JSON object in `localStorage` under the key `campaigns`:

```json
{
  "topic-slug-YYYYMMDD": {
    "id": "topic-slug-YYYYMMDD",
    "topic": "GEO / AI-driven discovery",
    "expertType": "practitioners who've shipped AI search tools",
    "goal": "understand how teams measure GEO impact",
    "phase": 4,
    "candidates": [
      { "name": "...", "title": "...", "linkedin": "...", "signal": "...", "selected": true }
    ],
    "profiles": { "Person Name": "pasted profile text" },
    "profileSkips": { "Person Name": false },
    "briefs": {
      "Person Name": {
        "title": "...", "linkedin": "...", "website": "...",
        "whoTheyAre": ["..."], "whyRelevant": ["..."],
        "score": 8, "skip": false
      }
    },
    "messages": {
      "Person Name": { "linkedin": "...", "website": "...", "message": "..." }
    }
  }
}
```

`phase` tracks how far a campaign has progressed (1 = Discovery, 2 = Profiles, 3 = Briefs, 4 = Messages). Opening a campaign navigates to its current phase.

---

## Modifying the tool

All logic is in `index.html`. Common things to change:

| What | Where in the file |
|------|------------------|
| Sign-off name / team | Search for `Nitya Negi` in the `<script>` section |
| Message style examples | `OUTREACH_EXAMPLES_LONG` and `OUTREACH_EXAMPLES_SHORT` constants |
| AI model | `PROVIDERS` object at the top of the `<script>` section |
| Brief JSON schema | `generateAllBriefs()` and `runQuickAdd()` functions |
| Message prompt rules | `msgSystemPrompt` inside `generateAllMessages()` |
