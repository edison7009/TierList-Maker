# Tier Presets & Colors

The editor ships three presets. Pick one in Step 1, or go custom. A preset only
sets **tier names + title-bar colors (+ font sizes for henz)** — it does NOT set
the board background; you still set the single global `bgBrightness` yourself.

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
Default `bgBrightness` for henz is **50** (light gray-white) — set it unless the
user says otherwise.

## love preset (Love → Dislike) — 5 tiers, English

A "My Faves" style list.

| # | name | color | fontSize |
|---|---|---|---|
| 1 | Love | `#FF7F7F` | 24 |
| 2 | Like | `#FFBF7F` | 24 |
| 3 | Okay | `#FFFF7F` | 24 |
| 4 | Meh | `#7FFF7F` | 24 |
| 5 | Dislike | `#7FBFFF` | 24 |

Uses the default palette colors. `bgBrightness` defaults to 0 (dark) unless the
user says otherwise.

## Choosing `bgBrightness`

- 0 → `#181818` (near-black). Default for most lists; cards pop.
- 50 → mid-gray. henz uses this.
- 100 → `#F7F7F7` (near-white). Use for light-themed content.
- Any integer 0..100 in between is valid (linear gray).

## Color tips for cards

- Coordinate a text card's `bgColor` with its tier's title `color` for a
  cohesive look (e.g. tier `#FF0000`, card bg `#e11d48`).
- Keep text readable: pair a dark `textColor` with a light `bgColor` and vice
  versa. `#ffffff` text on a saturated bg works for most labels.
- If unsure, **omit both colors** and let the editor auto-pick from its palette.
