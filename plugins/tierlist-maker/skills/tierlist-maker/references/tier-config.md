# Tier Presets & Colors

The editor ships three presets. They come up in **Step 2** (tiers) as options the
user may pick, rename, or replace — never as a default you push. Per `SKILL.md`
Step 2, lead with the language-neutral `T1..Tn` instead.

A preset only sets **tier names + title-bar colors (+ font sizes for henz)**. It
does **not** set the board background: `bgBrightness` is derived in Step 4 from
the card colors, never taken from a preset. See "Choosing `bgBrightness`" below.

## Default (no preset) — 15 tiers

Used when the user wants a plain `T1`..`T15` style list or a custom-named list
(you set every `name`). Title-bar colors run red→white:

| Tier | Color |
|---|---|
| T1 | `#FF7F7F` |
| T2 | `#FFBF7F` |
| T3 | `#FFDF7F` |
| T4 | `#FFFF7F` |
| T5 | `#BFFF7F` |
| T6 | `#7FFF7F` |
| T7 | `#7FFFBF` |
| T8 | `#7FFFFF` |
| T9 | `#7FBFFF` |
| T10 | `#7F7FFF` |
| T11 | `#BF7FFF` |
| T12 | `#FF7FBF` |
| T13 | `#FF7F7F` (lighter) |
| T14 | `#BFBFBF` |
| T15 | `#F7F7F7` |

Default font size 24. For a custom list, use `DEFAULT_TIER_CONFIG.slice(0, n)`
colors for the first n tiers, and let the user name them.

## henz preset (夯 → 拉完了) — 5 tiers, Chinese

A bold "从夯到拉" (best → worst) list. **henz is Chinese-only** for tier names.

| # | name (zh-Hans / en / ja / ko) | color | fontSize |
|---|---|---|---|
| 1 | 夯 | `#FF0000` | 72 |
| 2 | 顶级 | `#FFC000` | 56 |
| 3 | 人上人 | `#FFFF00` | 36 |
| 4 | NPC | `#FFF2CC` | 48 |
| 5 | 拉完了 | `#FFFFFF` | 36 |

Traditional Chinese (zh-Hant) names: 夯 / 頂級 / 人上人 / NPC / 拉完了.

The site's own henz *route* entry (`/t/chdl`) seeds a light board (brightness 50),
but that seed never reaches an imported board — the value in your `.tiervibe.json`
takes precedence. So derive `bgBrightness` from the card colors in Step 4 like any
other list; don't copy the 50.

## love preset (Love → Dislike) — 5 tiers, English

A "My Faves" style list.

| # | name | color | fontSize |
|---|---|---|---|
| 1 | Love | `#FF7F7F` | 24 |
| 2 | Like | `#FFBF7F` | 24 |
| 3 | Okay | `#FFFF7F` | 24 |
| 4 | Meh | `#7FFF7F` | 24 |
| 5 | Dislike | `#7FBFFF` | 24 |

Uses the default palette colors. Like henz, it carries no board brightness —
derive `bgBrightness` in Step 4.

## Choosing `bgBrightness`

**`bgBrightness` is derived, not chosen.** `SKILL.md` Step 4 rule 1 is the source
of truth: the board brightness is the **opposite** of the card backgrounds, so the
board frames the cards instead of fighting them.

- Cards are **light-tinted** (dark text on a light-tint bg) → board goes **dark**,
  `bgBrightness` 0-15.
- Cards are **dark-saturated** (light text on a dark bg) → board goes
  **light/near-white**, `bgBrightness` 90-97.

Pick the card scheme first, then set `bgBrightness` to the opposite end. Do not
ask the user for "light or dark board" as a separate question, and do not copy a
value from a preset or an example.

For reference, the scale itself: 0 → `#181818` (near-black), 100 → `#F7F7F7`
(near-white), linear gray in between; any integer 0..100 is valid.

## Color tips for cards

- **Always provide `textColor` AND `bgColor` on every text card.** Omitting them
  makes the editor auto-pick from its palette, which produces a flat, uniform row
  — `SKILL.md` Step 4 rule 3 rules this out. (Providing only *one* of the two is
  worse still: the reader drops both.)
- Give each card its own color identity so the board reads as varied. Any theme,
  any hue — warm, cool, neon, monochrome, brand colors, whatever fits the topic.
- **The one hard rule is readability**: `textColor` and `bgColor` must differ
  clearly in brightness — one around luminance 0.1-0.3, the other 0.7-0.95. Both
  "dark text on light bg" and "light text on dark bg" are fine; both-dark or
  both-light is not. If the two merge when you squint, re-pick.
- Coordinating a card's `bgColor` with its tier's title `color` is a nice touch
  for cohesion (e.g. tier `#FF0000`, card bg `#e11d48`) — but it is optional, and
  it never overrides the readability rule.
