# Explanations (`detail`) — Markdown Subset

Each card may carry a `detail` string — markdown the reader sees on the
published post. Good explanations are what make a tier list worth reading. They
also help SEO: the post page server-renders card explanations as real text.

## Allowed (GitHub-Flavored Markdown subset)
- Paragraphs (blank line between).
- Headings: `#`, `##`, `###` (deeper is rarely needed; keep to 1–3 levels).
- **bold**, *italic*, ***both***, `inline code`.
- Fenced code blocks: triple backticks.
- Bullet lists: `- ` or `* `; numbered lists: `1. `.
- Blockquotes: `> `.
- Tables: GFM pipe tables.
- Links: `[text](https://...)`.
- Horizontal rule: `---`.
- Strikethrough: `~~text~~`.

## NOT allowed / won't render
- **Raw HTML** (`<div>`, `<span>`, `<img>`, …) — escaped and shown as literal
  text. Do not use it.
- **Image markdown** `![alt](url)` for rendering inside `detail` — the post does
  not render images from `detail`; cards already show their own images. (A link
  to an external image as a URL is fine, but it won't be embedded.)
- Footnotes, definition lists, math, or any non-GFM extension.

## Style guidance
- 1–4 short paragraphs per card. Tight, not a wall of text.
- Lead with the verdict, then the why: "Top-tier for X because Y."
- Avoid repeating the card label; the reader can see it.
- Keep language to the post's language (match the user's tier names — if the
  list is Chinese, write explanations in Chinese; if English, English).
- Empty/whitespace-only `detail` is dropped by the reader (treated as
  "no explanation"). That's fine — not every card needs one.
- Trim trailing whitespace; the reader trims, but keep it clean.

## Example
```markdown
## Why this is 夯

Multi-file refactors and long-horizon planning are consistently strong.
Cost is high, but for hard tasks it's the most reliable pick.

- Great at cross-file changes
- Strong on edge cases
- Watch the price for throwaway tasks
```
