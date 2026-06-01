# Expert Outreach Tool

A browser-based LinkedIn expert outreach tool for Cartier Digital Innovation & AI. No installation, no backend, no build step — open `index.html` in any browser and go.

---

## Demo

[Watch how the system works →](https://www.loom.com/share/e2cb2a8c762d446c8a30d57826498110)

---

## How to run

1. Clone or download this repo
2. Open `index.html` directly in Chrome, Safari, or Firefox (`file://` works fine)
3. Enter your API key on the first screen — it is saved in your browser and never sent anywhere except the AI provider you choose

---

## Option A — Start a new campaign

Use this when you are reaching out to a new set of experts on a topic. The campaign runs through four phases:

### Phase 1 — Candidate Discovery
Find the people you want to reach out to. Two ways to do this:
- **Search for experts** — AI generates targeted search queries and returns a list of relevant candidates with names, titles, LinkedIn URLs, and a signal for why they're relevant. You select who to keep.
- **Add manually** — paste in names and links yourself if you already know who you want to contact.

Both can be used together — run a search then add extra people manually.

### Phase 2 — Profiles
For each selected candidate, paste their LinkedIn profile text (or company page, blog post, or any other relevant context). You can also skip anyone at this stage so they are excluded from brief generation.

### Phase 3 — Briefs
AI reads each person's profile context and generates a brief covering who they are, why they're relevant to the campaign, a relevance score, and the best opening hook for outreach. Briefs are displayed as bullet points for quick review. You can skip anyone before moving to messages, or delete them entirely.

### Phase 4 — Messages
AI drafts a personalised outreach message for each non-skipped person using their brief and profile context as the only source. Two format options before drafting:
- **Long** — 3 to 5 short paragraphs with a sign-off
- **Short** — under 200 characters, no sign-off, for sending a LinkedIn connection request message without Premium

Each message card has Copy, Edit, Revise with note, and Regenerate options. All messages can be exported as a markdown file.

---

## Option B — Quick Add to an existing campaign

Use this when a campaign is already running and you want to add more people without restarting the full flow. Select an existing campaign, paste in one or more profiles, choose Long or Short message format, and generate — briefs and messages are created and appended to that campaign in one step.

---

## AI providers

| Provider | Web search | Notes |
|----------|-----------|-------|
| Anthropic (Claude) | Yes | |
| OpenAI (GPT) | Yes | |
| DeepSeek | No | Queries shown for manual search |
| Qwen (Alibaba) | No | Queries shown for manual search |

API keys are stored in your browser. They are never sent anywhere except the provider's own API endpoint.

---

## Style reference

`examples/outreach_examples.txt` contains real outreach messages that worked. The file is embedded in `index.html` as two constants:

- `OUTREACH_EXAMPLES_LONG` — 3 multi-paragraph messages used as the style reference for long drafts
- `OUTREACH_EXAMPLES_SHORT` — 4 short messages used as the style reference for under-200-character drafts

To update the style reference, edit these constants directly in `index.html`.

---

## Modifying the tool

All logic is in `index.html`. Common things to change:

| What | Where in the file |
|------|------------------|
| Sign-off name / team | Search for `Nitya Negi` in the `<script>` section |
| Message style examples | `OUTREACH_EXAMPLES_LONG` and `OUTREACH_EXAMPLES_SHORT` constants |
| AI model | `PROVIDERS` object at the top of the `<script>` section |
| Message prompt rules | `msgSystemPrompt` inside `generateAllMessages()` |
