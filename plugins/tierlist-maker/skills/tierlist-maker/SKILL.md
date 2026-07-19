---
name: tierlist-maker
description: Helps a user author a TierVibe tier list end-to-end and produce a .tiervibe.json they import at tiervibe.com/t/import. Use when the user wants to make/create/build a tier list, rank items, put things into tiers (S/A/B/C, 夯/顶级/.../拉完了, Love/Like/Okay/Meh/Dislike), or make a "从夯到拉" list. Interviews the user, sets up tiers, drafts cards (text or image), writes markdown explanations, validates the file, and tells the user how to import it. No server calls, no login until the final step.
when_to_use: ["make a tier list", "create a tier list", "build a tier list", "rank these items", "tier list maker", "S A B C ranking", "从夯到拉", "夯到拉", "tier list for ...", "rank ... into tiers"]
---

# TierList Maker

You help the user author a **TierVibe tier list** and produce a single `.tiervibe.json` file that they import into the TierVibe editor. The whole point is to remove the manual labor (finding images, adding cards, writing explanations) — the user only does the final drag-to-sort and publish.

## The principle (read this first)

- **Everything before "publish" is the user's local behavior.** Your job is to produce a *file*. You do NOT call any TierVibe API, do NOT upload anything, do NOT open a browser, do NOT require the user to log in until the very last step.
- **Keep it simple.** Your only output is one JSON file on the user's disk. Minimal permissions, no network, no browser automation.
- **Never mention login until Step 6.** Login happens naturally at import time (the TierVibe page asks if needed). Do not ask the user to log in, do not check auth, do not talk about accounts in Steps 1–5.

## The 6-step workflow

Run this as a conversation. Ask before assuming. Keep the user in the loop on the creative choices.

### Step 1 — Interview the topic
Find out:
- What is the list about? (e.g. "AI coding models", "best ramen in Tokyo", "Haruki Murakami novels")
- How many tiers (1–15)? If unsure, suggest 5.
- Which preset, if any: **henz** (夯→拉完了, Chinese), **love** (Love→Dislike, English), or **custom** (user names every tier). See [references/tier-config.md](references/tier-config.md) for the exact preset colors/names.
Tell the user: text cards are the most reliable; image cards are optional (they attach images themselves later if they want).

### Step 2 — Set up the tiers
For each tier, decide:
- `name` — the tier label (e.g. "夯", "S", "Love"). Keep ≤ 100 chars.
- `color` — the **title-bar** hex color (e.g. `#FF0000`). Pick colors that read well together; presets give you a ready palette.
- `fontSize` — optional. Default is 24; henz uses 72/56/36/48/36. Only set if the user wants custom sizing.

Then set the **global** board background:
- `bgBrightness` — a single number 0..100 for the WHOLE board (0 = near-black `#181818`, 100 = white). There is **no per-tier background** — do not invent one. This is the only background control. Default 0 if the user doesn't care.

See [references/tier-config.md](references/tier-config.md).

### Step 3 — Draft the cards
For each item the user wants to rank, create a card and assign it to a tier (or to the unranked `candidates` pool):

- **Text card** (most reliable, recommended): `{ "type": "text", "text": "GPT-4", "textColor": "#ffffff", "bgColor": "#e11d48" }`. Colors are optional — if you omit both, the editor auto-picks. You may coordinate card colors with the tier's title color for a cohesive look. See [references/text-cards.md](references/text-cards.md).
- **Image card** (optional): `{ "type": "image", "imageUrl": "https://...", "label": "Claude" }`. The URL is a *suggestion/placeholder* — the user attaches the real image in the editor if they want (their local image, uploaded through the editor's normal flow). If you can't find a good URL, **use a text card instead**. Do not download or upload anything. See [references/text-cards.md](references/text-cards.md) for the image caveat.

### Step 4 — Write explanations (the "讲解" mode)
Each card may carry a `detail` field — markdown the reader sees on the published post. This is what makes a tier list worth reading. For each card (and optionally a short board intro), write 1–4 short paragraphs:

- Plain text, headings (`#`/`##`/`###`), **bold**, *italic*, `code`, bullet/numbered lists, blockquotes, tables, links, horizontal rules.
- **No raw HTML** — it is escaped and shown as text. **No image syntax** for rendering (the post renders cards separately).
- Keep it tight and genuinely informative — why this item is in this tier.

Full rules in [references/explanations.md](references/explanations.md).

### Step 5 — Emit and self-check the file
Write one file, `<topic-slug>.tiervibe.json` (e.g. `ai-coding-models.tiervibe.json`), to the user's current working directory, following [references/data-schema.md](references/data-schema.md) exactly.

Before finishing, self-check:
- `title` is non-empty, ≤ 200 chars.
- `tiers` is a non-empty array, length 1–15.
- Every tier has `name` + hex `color`.
- `bgBrightness` (if present) is 0..100.
- Every text card has non-empty `text`; every image card has an http(s) `imageUrl`.
- No raw HTML inside any `detail`.

Print the absolute path of the file you wrote and a 3–5 line summary of the list (title, # tiers, # cards, # with explanations).

### Step 6 — Final step (NOW mention import + login)
Tell the user, in their language:

1. Open **https://tiervibe.com/t/import** in their browser.
2. Click the drop zone and choose the `.tiervibe.json` you just wrote (give the path).
3. **If not logged in, the site asks them to log in now** — that's expected and is the only time login comes up. (Google one-click or email.)
4. The list opens in the editor with all tiers, cards, and explanations filled in.
5. **Drag the cards to sort them** (this is the part only the user can do), then click **发布** (Publish).

Do NOT mention login, accounts, or sign-in anywhere before this step.

## Reference files (load on demand)
- [references/data-schema.md](references/data-schema.md) — the full `.tiervibe.json` format with all fields, types, limits, and validation rules. **Read this before Step 5.**
- [references/tier-config.md](references/tier-config.md) — the henz / love / default presets (tier names, colors, font sizes, bgBrightness).
- [references/text-cards.md](references/text-cards.md) — text-card color protocol and the image-card caveat.
- [references/explanations.md](references/explanations.md) — the allowed markdown subset and limits.
- [references/import-flow.md](references/import-flow.md) — exactly what the user sees at /t/import (for your Step 6 wording).

## Templates (copy and fill)
- [templates/blank.tiervibe.json](templates/blank.tiervibe.json) — minimal skeleton.
- [templates/henz-5tier.tiervibe.json](templates/henz-5tier.tiervibe.json) — 5-tier henz preset, text cards.
- [templates/text-only.tiervibe.json](templates/text-only.tiervibe.json) — pure text cards, the most reliable form.

## Worked example
- [examples/ai-models-tierlist.md](examples/ai-models-tierlist.md) — a full sample run (AI coding models) showing the interview and the resulting JSON.

## Hard rules
- Output exactly one `.tiervibe.json`. No other files unless the user asks.
- Do not call any URL, do not upload, do not open a browser, do not run code that touches the network — except reading the user's own files if they point you to images.
- Login is a Step-6 topic only. Never earlier.
- No per-tier background field exists. Use the single `bgBrightness` only.
- Keep SKILL scope: if the user asks for something this skill doesn't cover (e.g. editing an existing post, deleting, social features), say this skill only creates new lists via import.
