---
name: tierlist-maker
description: Builds a TierVibe tier list through a step-by-step interview, then AUTO-OPENS it in the user's browser (no file drag). Use when the user wants to make/create/build/rank a tier list, put items into tiers (S/A/B/C, 夯/顶级/.../拉完了, Love/Like/Okay/Meh/Dislike), or make a "从夯到拉" list. Asks ONE question at a time — FIRST whether the user wants an image tier list or a text one. For image mode, priority is the user supplying images (public links or local files); AI image search/generation is the AI's own capability, not this skill's. Sets up tiers, drafts text or image cards, writes markdown commentary per card, then opens https://tiervibe.com/t/import#data=... so the board loads automatically. No server calls, no login until the final step.
when_to_use: ["make a tier list", "create a tier list", "build a tier list", "rank these items", "tier list maker", "S A B C ranking", "从夯到拉", "夯到拉", "tier list for ...", "rank ... into tiers"]
---

# TierList Maker (TierVibe)

You build a TierVibe tier list WITH the user through a step-by-step interview, then **auto-open the result in their browser**. The user logs in only at the end, drags the cards to sort, publishes.

## Non-negotiable rules (read first)
1. **Ask ONE question at a time.** Wait for the answer. Never dump a full list in one shot.
2. **Ask image-vs-text mode FIRST (Step 0).** Two card modes — image cards (a picture on each card) or text cards (a label on each card). Ask which the user wants before anything else; never assume text. For image cards use only a REAL `https://` URL the user supplied or you personally verified — never guess or fabricate a URL (broken images are the #1 failure). Local image files (`D:\…`, `file:`, `data:`, `blob:`) CANNOT be embedded in the `.tiervibe.json` — the reader rejects them — so the local-images flow uses text-card placeholders + a manifest mapping each file to its item/tier/detail (Step 3½); the user swaps the placeholder for their local file in the editor after import.
3. **Commentary must use real markdown** (`##`, `**`, `-`, `>`), not flat prose. Rules below.
4. **Login is the last step only.** Never mention accounts/sign-in before the final "open browser" step.
5. **Final step = open the browser with the data in the URL.** Not "save a file and drag it."
6. **Never railroad into a fixed 1/2 choice.** When you offer options (presets, styles, colors, anything), ALWAYS end with an open escape — "or tell me your own / 或者你说说你想要的别的". The user can describe freely in their own words; adapt to whatever they say. Do not force a pick from a numbered list.
7. **Open the USER'S DEFAULT system browser — never an agent's built-in/headless browser.** This URL is for a human to click around in: it needs the user's real browser (Google Chrome / Edge / Safari / Firefox — whatever they set as the OS default), so it carries their cookies, logins, bookmarks, and muscle memory. Do NOT open it via playwright/puppeteer/a headless instance/the agent tool's embedded browser — those have no login session and break the "log in at the last step" flow. Use the OS open command only (next section).

## The interview (one question at a time)

## Step 0 — Mode: image or text? (ask this first, before anything else)

Ask ONE question: **图片版还是文字版?(image tier list, or text?)** Wait for the answer.

- **Text version** — each card shows a label. Fastest, most reliable, no image hunting. Best for abstract items (model names, dish names, years, concepts). → skip Step 3½, go straight to Step 1.
- **Image version** — each card shows a picture. Looks better, but you need an image for every item. Best when the image IS the point (a specific character, logo, photo). → you'll gather images in Step 3½ after the items are known, then continue through Steps 1→2→4→5 like text mode.

State the trade-off, don't push one over the other. End with an open escape (rule 6): "或者你跟我说说你想怎么排 / or tell me what you have in mind". Whatever the user says, adapt.

**Step 1 — Topic.** "What's the list about?" (e.g. "AI coding models", "2006-2025 哪年我最喜欢"). Wait.

**Step 2 — Tiers: count, titles, background.** Ask, one at a time:
- "How many tiers (1-15)?"
- "What should each tier be **titled**?" Default is the universal, language-neutral **`T1, T2, … Tn`** — this is a global skill, so lead with that, not any locale-specific preset. Titles are the user's choice and not fixed. Keep titles **SHORT** — the title bar is narrow:
  - Best: a single token per tier — `T1`, `S`, `A`, `夯`, `Love`, `Best`, `Worst`.
  - If the user wants category-style titles, one short word each (e.g. `Fruit`, `Meat`, `Grain`) — not a phrase.
  - Never a long title like `当前人类长寿最科学的方案` on a tier bar — it wraps/clips. Save long descriptions for the post **title** field, not the tier bar.
  - Optional presets the user can pick/rename/replace: henz `夯/顶级/人上人/NPC/拉完了` (Chinese), love `Love/Like/Okay/Meh/Dislike` (English), `S/A/B/C/D`. Mention only as options — don't push a locale-specific default.

(Don't ask "light or dark board" here — `bgBrightness` is derived in Step 4 from the card colors, not chosen separately.)

Then set each tier's `name` (the user's chosen title) + title-bar `color` + optional `fontSize`. Presets in `references/tier-config.md`.

**Step 3 — Items & placement.** "Which items to rank? List them." Wait. Then propose a rough placement and CONFIRM: "I'd put X in 夯, Y in 顶级... sound right?" Adjust on feedback. Items not yet placed go to `candidates` (the unranked pool).

## Step 3½ — Images (image mode ONLY; skip entirely for text mode)

You need one image per item. **Priority: the user provides the images** — this is the only path fully inside this skill, and the most reliable. Image *finding/generation* (web search, AI image generation) is the AI's OWN capability and tooling, **NOT a feature of this skill** — be honest about that. If the AI can search/generate, it does so OUTSIDE this skill and returns here once images exist; if it can't, the user finds the images and comes back. Do NOT promise an image-search step as a skill feature.

Ask ONE question: **"图片你准备好了吗?是哪种情况?"** and branch:

**A. User has public https links** (图床 / Wikipedia / 官网图 etc.)
- Have the user paste one URL per item. Verify each starts with `https://`.
- Put each URL straight onto the card: `{ "type": "image", "imageUrl": "<url>", "label": "<item>", "detail": "..." }`.
- Warn: external image hosts may taint the publish-time cover canvas (CORS); treat these as placeholders the user can swap in the editor for a guaranteed-clean cover.

**B. User has the image files saved locally** (e.g. `1.png`, `2.png` on disk)
- Local file paths can't go in the JSON (schema rejects `file:`/`data:`/`blob:`). So you do NOT read, encode, or embed the files — you never need to visually inspect them.
- Instead, build a **manifest table** — your working source of truth. Ask the user, one item at a time, which file maps to which item/tier. Leave the `detail` column BLANK for now — it gets filled when Step 5 (commentary depth) runs later. Example:

  | 文件 file | 条目 item | 层级 tier | 讲解 detail |
  |---|---|---|---|
  | 1.png | 苹果 | 夯 | _(filled in Step 5)_ |
  | 2.png | 香蕉 | 顶级 | _(filled in Step 5)_ |

  The manifest is a working doc — it does NOT go in the JSON. It exists so you can build the board without ever looking at the images, and so the user has a swap cheat-sheet after import.
- Emit **text-card placeholders** in the JSON: each card `{ "type": "text", "text": "<item>", ... }` so the user can identify it in the editor by its label. (If the user also has a public URL for some items, use an image card for those and text placeholders for the rest.)
- Hand the manifest to the user at the end. After import, they swap each placeholder card's image for the matching local file in the editor (the editor uploads it to the platform CDN — the only clean path for local images). The manifest is their swap cheat-sheet: "the card labeled 苹果 → use 1.png".

**C. User doesn't have images yet / wants help finding them**
- Be honest: whether you can search the web for images or generate them depends on YOUR OWN tools and capabilities, not on this skill. Don't promise a search step as a skill feature.
- If you DO have web-search/vision tools: search per item (prefer stable, CORS-friendly hosts like Wikimedia Commons / official sites), show the user what you found, get confirmation — then it becomes branch A. Keep that search OUTSIDE these steps; the skill resumes once you have URLs.
- If you DON'T: say so plainly, suggest the user find the images themselves (then branch A or B), and keep building the board with text-card placeholders so nothing is blocked. The user can add images in the editor anytime.

In every image-mode branch, still run Steps 1, 2, 4, 5 (topic, tiers, colors, commentary). Image cards carry `detail` commentary exactly like text cards.

**Step 4 — 配色 (ask the style first, then generate — don't copy a fixed palette).** Colors are the user's design space. ASK: "What color style/feel do you want?" Offer options: pastel/soft, vibrant/saturated, dark & moody, warm, cool, monochrome, or their own description. The user picks — then you generate a scheme that FITS that style. The specific hues come from the user's chosen style, **never copied from an example**. Follow these PRINCIPLES, not a hex list:

1. **Board background CONTRASTS the card backgrounds (reverse linkage).** `bgBrightness` (0..100) is NOT a free choice — derive it from the cards' `bgColor` so the board frames the cards, not fights them:
   - Cards are **light-tinted** (the default: dark text on light-tint bg, e.g. `#2f5f96` text / `#8bb8e8` bg) → board goes **dark** (`bgBrightness` 0-15). Light cards pop against a dark board.
   - Cards are **dark-saturated** (light text on dark bg, e.g. `#8bb8e8` text / `#2f5f96` bg) → board goes **light/near-white** (`bgBrightness` 90-97). Dark cards pop against a light board.
   - i.e. card-bg brightness and board brightness are **opposite** — pick the card scheme first, then set `bgBrightness` to the opposite end. Don't ask the user separately for "light or dark board" in Step 2 — it's decided here by the card colors.
   - The user's Step 2 answer ("light/dark feel") is the STYLE hint (moody vs clean); the ACTUAL `bgBrightness` value follows the card-bg reverse rule above.

2. **Tier title bars form a gentle gradient top→bottom** (e.g. warm at top → cool at bottom for a best→worst feel). Saturation follows the style: pastel = low saturation (soft); vibrant = high saturation; dark/moody = deep jewel tones. Do NOT use harsh raw primaries (`#FF0000`, `#FFFF00`) unless the user explicitly asked for "loud".

3. **Text cards: freely themed colors — the only hard rule is READABILITY.** Any theme, any hue, any combination (warm, cool, festive, neon, monochrome, brand colors, whatever fits the topic). Give each card its own color identity so the board is varied. The ONE non-negotiable constraint: `textColor` and `bgColor` must have **enough brightness contrast** — one dark, the other light — so the text is legible.
   - heuristic: pick the pair so one is clearly dark (luminance ~0.1-0.3) and the other clearly light (luminance ~0.7-0.95). If you squint and the two merge, it's unreadable — re-pick.
   - this means BOTH "dark text on light bg" AND "light text on dark bg" are fine — pick whichever fits the theme. They just can't be both-dark or both-light.
   - the board `bgBrightness` follows the OPPOSITE of the card `bgColor` (rule 1): if your card bgs are light, board goes dark; if your card bgs are dark, board goes light.
   - never: dark-on-dark or light-on-light (unreadable); omitting colors on every card (auto-palette = a boring uniform row).
   - provide BOTH `textColor` and `bgColor` for every text card (a lone color is dropped by the reader).

Show the user ONE sample card's colors + one tier bar first, confirm the style reads right, then do all. If the user says "you pick / I don't care", default to a pastel light board.

**Step 5 — Commentary depth (ask first).** Each card's `detail` is the text shown on the right side when a viewer clicks the card. Ask the user how much commentary they want — never just write a title + one line (that's worse than none). Offer:

1. **None** — a pure ranking is enough; skip `detail` on every card.
2. **One line per card** — a single short sentence introducing each item.
3. **A few bullet points per card** — concise summary, not deep.
4. **Detailed per-card breakdown** — fuller reasoning per point, but keep each card's `detail` ≤ ~1000 chars (the platform caps the whole post's content at 2MB server-side; ~1000/card stays readable and safely under).
5. or tell me your own approach (length/style).

Wait for the pick. Then write each card's `detail` at the chosen depth, in markdown (next section). Show the user **ONE sample card's `detail` first**, confirm voice + length, then do all. Do NOT write flat prose — use the markdown formatting below. If you're in image mode + branch B (local files), also fill the `detail` column of the manifest table here — same text goes on the card's `detail` and the manifest row.

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
   - run ONE OS open command — this launches the user's **default system browser** (Chrome / Edge / Safari / Firefox), with their real cookies & login session:
     - Windows: `start "" "https://tiervibe.com/t/import#data=<urlencoded-base64>"`
     - macOS: `open "https://tiervibe.com/t/import#data=<urlencoded-base64>"`
     - Linux: `xdg-open "https://tiervibe.com/t/import#data=<urlencoded-base64>"`
   - **Do NOT** open it via playwright/puppeteer/a headless instance/the agent tool's embedded browser — those have no login session and break the "log in at the last step" flow. The OS `start`/`open`/`xdg-open` command is the only correct way (see rule 7).
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
- Finding/generating images → that's the AI's own capability, not this skill. The skill resumes once images are available (https URLs → image cards; local files → text-card placeholders + a manifest, swapped in the editor). The skill itself never downloads or uploads images.
