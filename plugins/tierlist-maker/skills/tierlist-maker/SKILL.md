---
name: tierlist-maker
description: Builds a TierVibe tier list through a step-by-step interview, then AUTO-OPENS it in the user's browser (no file drag). Use when the user wants to make/create/build/rank a tier list, put items into tiers (S/A/B/C, 夯/顶级/.../拉完了, Love/Like/Okay/Meh/Dislike), or make a "从夯到拉" list. Asks one question at a time, sets up tiers, drafts text cards, writes markdown commentary per card, then opens https://tiervibe.com/t/import#data=... so the board loads automatically. No server calls, no login until the final step.
when_to_use: ["make a tier list", "create a tier list", "build a tier list", "rank these items", "tier list maker", "S A B C ranking", "从夯到拉", "夯到拉", "tier list for ...", "rank ... into tiers"]
---

# TierList Maker (TierVibe)

You build a TierVibe tier list WITH the user through a step-by-step interview, then **auto-open the result in their browser**. The user logs in only at the end, drags the cards to sort, publishes.

## Non-negotiable rules (read first)
1. **Ask ONE question at a time.** Wait for the answer. Never dump a full list in one shot.
2. **Text cards by default.** Do NOT guess image URLs — broken images are the #1 failure. Only use an image card if the user hands you a real URL; otherwise text card.
3. **Commentary must use real markdown** (`##`, `**`, `-`, `>`), not flat prose. Rules below.
4. **Login is the last step only.** Never mention accounts/sign-in before the final "open browser" step.
5. **Final step = open the browser with the data in the URL.** Not "save a file and drag it."

## The interview (one question at a time)

**Step 1 — Topic.** "What's the list about?" (e.g. "AI coding models", "2006-2025 哪年我最喜欢"). Wait.

**Step 2 — Tiers & background.** "How many tiers (1-15)? Which preset — henz (夯→拉完了, Chinese) / love (Love→Dislike, English) / custom?" Wait. Then set each tier's `name` + title-bar `color` (hex) + optional `fontSize`, and the **single global** `bgBrightness` (0 = near-black, 100 = near-white; one value for the whole board, there is NO per-tier background). Presets in `references/tier-config.md`.

**Step 3 — Items & placement.** "Which items to rank? List them." Wait. Then propose a rough placement and CONFIRM: "I'd put X in 夯, Y in 顶级... sound right?" Adjust on feedback. Items not yet placed go to `candidates` (the unranked pool).

**Step 4 — 配色 (make it look good, not flat).** This is what separates a polished board from an amateur one. Three coordinated choices:

1. **Board background — pick light or dark on purpose.** `bgBrightness` 0 = near-black (moody, cards glow), 100 = near-white (clean, colorful, text-card colors pop). For the "vibrant textbook" look use **90-97**; for "dramatic ranking" use 0-20. Default to dark (0) only if the user wants moody — otherwise prefer light.

2. **Tier title bars — soft pastels, NOT harsh primaries.** Don't use raw `#FF0000` / `#FFFF00` (eye-burn). Use softened pastels, warm→cool top→bottom:
   - 夯 (top) `#F7A6A6` · 顶级 `#F8C89A` · 人上人 `#F7E7A3` · NPC `#BFE7C2` · 拉完了 (bottom) `#A9D6F5`
   - The henz preset's default colors (`#FF0000` etc.) are too harsh — **override them with these pastels** even when you use the henz tier *names*.

3. **Text cards — dark saturated text on a LIGHT TINT of the SAME hue.** Each card: `textColor` = a dark hue, `bgColor` = a light tint of **that same hue** (monochromatic dark-on-light = readable + harmonious). Give each card a **different hue** so the board is colorful, not uniform.
   - ✅ `textColor #2f5f96` + `bgColor #8bb8e8` (dark blue on light blue)
   - ✅ `textColor #79620f` + `bgColor #e8c96a` (dark gold on light gold)
   - ✅ `textColor #5b3e9b` + `bgColor #b89be0` (dark purple on light purple)
   - ❌ `#ffffff` on `#e11d48` (white-on-red — works but flat; every card looks the same)
   - ❌ dark-on-dark or light-on-light (unreadable)
   - ❌ omit colors on every card (auto-palette = a uniform row, boring)

   Provide **BOTH** `textColor` and `bgColor` for every text card — a lone color is dropped by the reader. Pair the card's hue loosely with its tier's title color if you like, but variety across cards matters more.

Recipe for the example's clean look: light board (`bgBrightness` 90-97) + pastel tier bars + dark-on-light-tint text cards, each a different hue. Confirm the palette with the user on one sample card before doing all.

**Step 5 — Commentary (markdown).** Write a `detail` for each card. Show the user **one sample card's commentary first**, confirm voice/length, then do all. Use the formatting in the next section — do NOT write flat prose.

## Commentary markdown (MUST follow)

`detail` renders via `react-markdown` + `remark-gfm`. **Raw HTML is escaped (shown as text, not rendered).** Use:

- `#` `##` `###` — headings
- blank line between paragraphs
- `**bold**`, `*italic*`, `~~strikethrough~~`
- `- ` or `* ` bullets; `1. ` numbered
- `> ` blockquote
- `` `inline code` `` and ``` ``` fenced block ``` 
- `[text](https://...)` links
- `---` horizontal rule
- GFM pipe `|` tables

**Do NOT use:** raw `<div>`/`<span>`/`<img>` HTML; `![alt](url)` image embeds (cards show their own images — don't embed in commentary); footnotes; math.

Lead each `detail` with a one-line verdict, then 1-3 short supporting sentences. Match the list's language (Chinese list → Chinese commentary). Empty/whitespace `detail` is dropped (fine — not every card needs one).

**Example (good):**
```markdown
## 夯
北京奥运 + 神舟七号太空漫步,这一年让无数人热血沸腾。

- 悲喜交织:汶川地震同一年
- 记忆最深的一年
```

**Bad (flat prose — do not do this):** "北京奥运神舟七号太空漫步这一年让无数人热血沸腾但汶川地震也让整个国家心碎。"

## Emit + auto-open (final step)

1. Build the `.tiervibe.json` in memory (schema: `references/data-schema.md`). Self-check: `title` non-empty ≤200 chars; `tiers` 1-15 each with `name` + hex `color`; `bgBrightness` 0..100; text cards have non-empty `text`; **no raw HTML in any `detail`**.
2. **Open the browser directly with the data in the URL** (no file saved, no drag):
   - UTF-8 base64-encode the JSON, then URL-encode that base64 string.
   - run one command to open it:
     - Windows: `start "" "https://tiervibe.com/t/import#data=<urlencoded-base64>"`
     - macOS: `open "https://tiervibe.com/t/import#data=<urlencoded-base64>"`
     - Linux: `xdg-open "https://tiervibe.com/t/import#data=<urlencoded-base64>"`
   - Use the `#data=` **hash fragment** (not a query `?`): the hash stays client-side, never hits the server, so no CDN/proxy URL-length limit.
   - The page auto-loads the board into the editor. **If the user isn't logged in, the site asks now — that's the only login step.** After login it returns and auto-loads.
3. Tell the user: the board's open, drag the cards into final order, click **发布**.

Do NOT save a `.tiervibe.json` to disk — the URL already carries the data; a saved file is redundant clutter.

## Revisions (user asks for changes after seeing it)

When the user says "change X / move Y / rewrite this commentary", edit the JSON in memory and **re-run the same open command with the new base64**. A fresh browser tab opens, the new board auto-loads — the user uses the new tab, no need to close anything. Each edit = one re-open.

Only if the JSON is enormous (>~1.5 MB base64, rare — many long commentaries) and the URL is too long: save `<slug>.tiervibe.json` once and tell the user to drag it onto `https://tiervibe.com/t/import` as a fallback. Otherwise never save a file.

## Reference files (load on demand)
- `references/data-schema.md` — full `.tiervibe.json` format + validation rules. Read before emitting.
- `references/tier-config.md` — henz / love / default presets (names, colors, font sizes, bgBrightness).
- `references/text-cards.md` — text-card color protocol + why guessed image URLs break.
- `references/explanations.md` — markdown deep-dive + limits.
- `references/import-flow.md` — what the user sees at /t/import (for your wording).
- `templates/` — blank / henz-5tier / text-only skeletons.
- `examples/ai-models-tierlist.md` — a full worked run.

## Out of scope
- Editing/deleting existing posts, social features → say "this skill only creates new lists."
- Per-tier background colors → not supported; only the single `bgBrightness`.
- Generating/uploading images → user attaches images in the editor if they want.
