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

## Modifying the tool

All logic is in `index.html`. Common things to change:

| What | Where in the file |
|------|------------------|
| Sign-off name / team | Search for `Nitya Negi` in the `<script>` section |
| Message style examples | `OUTREACH_EXAMPLES_LONG` and `OUTREACH_EXAMPLES_SHORT` constants |
| AI model | `PROVIDERS` object at the top of the `<script>` section |
| Message prompt rules | `msgSystemPrompt` inside `generateAllMessages()` |

### Style reference

`examples/outreach_examples.txt` contains real outreach messages that worked. The file is embedded in `index.html` as two constants:

- `OUTREACH_EXAMPLES_LONG` — 3 multi-paragraph messages used as the style reference for long drafts
- `OUTREACH_EXAMPLES_SHORT` — 4 short messages used as the style reference for under-200-character drafts

To update the style reference, edit these constants directly in `index.html`.

### API prompts

The tool makes six types of API calls. These are the exact instructions sent to the model at each step.

---

**1. Discovery — generate search queries**

> Generate 6–10 search queries to find the most relevant experts for this campaign. These queries will be run on any search platform — they are not limited to LinkedIn. Think about where practitioners in this space actually publish and get discovered.
>
> Mix query types: `site:linkedin.com/in`, podcast guest, conference speaker, newsletter author, case study author, company role blog.
>
> Return a JSON array of search query strings.

---

**2. Discovery — find candidates** *(web search enabled providers only)*

> Search the web using the generated queries and return up to 8 relevant experts. Prioritise practitioners who have actually shipped, written about, or spoken on the topic — not just people with the right job title. Leave the LinkedIn URL empty rather than guessing. The signal field is a one-sentence summary of why this person is relevant — used as context only, not as a message hook.
>
> Return a JSON array: `[{ name, title, linkedin, signal }]`

---

**3. Brief generation** *(one call per person)*

> Generate a brief for this person based on their profile context.
>
> Each bullet in `whoTheyAre` and `whyRelevant` must be a single short phrase or sentence — no long sentences, no paragraphs.
>
> Return JSON: `{ title, linkedin, website, whoTheyAre: [...], whyRelevant: [...], score }`

---

**4a. Message draft — Long**

> You draft personalised LinkedIn outreach messages for Nitya Negi, Digital Innovation & AI, Cartier APAC. Always sign off as: Nitya Negi / Digital Innovation & AI / Cartier-HK APAC.
>
> Message format: 3 to 5 short paragraphs. Open with a specific observation about their work sourced only from their profile context — name something concrete (a project, piece they wrote, talk, role, or experience) that clearly connects to the campaign goal. If nothing connects clearly, open with their overall background. Second paragraph: brief context on who Nitya is and what the team is building. Third paragraph: a clear, low-friction ask (20-min call, quick chat). Optional closing line. End with sign-off.
>
> Tone: casual and human, not formal or corporate. Personal pronouns. No em dashes. No filler sentences. No formal phrases.
>
> Match the tone, length, and structure of the embedded long examples exactly.

**4b. Message draft — Short**

> You draft short LinkedIn outreach messages for Nitya Negi, Digital Innovation & AI, Cartier APAC.
>
> Message format: under 200 characters total including the greeting. Structure: "Hi [First name], [one sentence referencing something specific from their profile context]. [One sentence stating what you want.]" No sign-off, no name, no title, no company. No em dashes. No filler.
>
> Match the tone, length, and structure of the embedded short examples exactly.

---

**5. Revise message**

> Revise the current message based on a note provided by the user. Keep the same casual, personal tone. Return only the revised message.

---

**6a. Quick Add — brief generation** *(one call per person)*

> Generate a brief for this person based on their profile context, scoped to the selected existing campaign.
>
> Each bullet in `whoTheyAre` and `whyRelevant` must be a single short phrase or sentence — no long sentences, no paragraphs.
>
> Return JSON: `{ title, linkedin, website, whoTheyAre: [...], whyRelevant: [...], score }`

---

**6b. Quick Add — message draft (Long)**

> You draft personalised LinkedIn outreach messages for Nitya Negi, Digital Innovation & AI, Cartier APAC. Always sign off as: Nitya Negi / Digital Innovation & AI / Cartier-HK APAC.
>
> Message format: 3 to 5 short paragraphs. Open with a specific observation about their work sourced only from their profile context — name something concrete (a project, piece they wrote, talk, role, or experience) that clearly connects to the campaign goal. If nothing connects clearly, open with their overall background. Second paragraph: brief context on who Nitya is and what the team is building. Third paragraph: a clear, low-friction ask (20-min call, quick chat). Optional closing line. End with sign-off.
>
> Tone: casual and human, not formal or corporate. Personal pronouns. No em dashes. No filler sentences. No formal phrases.
>
> Match the tone, length, and structure of the embedded long examples exactly.

**6c. Quick Add — message draft (Short)**

> You draft short LinkedIn outreach messages for Nitya Negi, Digital Innovation & AI, Cartier APAC.
>
> Hard limit: 200 characters total including the greeting. Count the characters before returning — do not return anything over 200 characters under any circumstances.
>
> Structure: "Hi [First name], [one sentence referencing something specific from their profile context]. [One sentence stating what you want.]" No sign-off, no name, no title, no company. No em dashes. No filler.
>
> Match the tone, length, and structure of the embedded short examples exactly.
