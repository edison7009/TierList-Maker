# Text & Image Cards

## Text cards

A text card is a small square showing a label. Most reliable — no network, no
CORS, no missing-image risk. **Which mode (text vs image) you use is decided in
Step 0 of the interview** (`SKILL.md`); this file covers the mechanics of each
card type once that choice is made. When text mode is chosen — or when an image
can't be cleanly obtained — use a text card.

### Color protocol
- Provide `textColor` AND `bgColor` **together**, or **omit both**.
- The reader encodes them into the card's stored `imageUrl` as
  `text:<encoded>#<fgHex>#<bgHex>` (hex without `#`).
- If you provide only one color, the reader drops both and the editor auto-picks
  from its palette. So: pair or neither.
- Hex colors: 3–8 hex digits, with or without leading `#`.

### Sizing the text
Keep `text` to a short label (a few words). The card auto-fits font size to the
text length; very long text gets shrunk and clipped. If a card needs a long
description, that belongs in `detail` (the explanation), not `text`.

## Image cards (optional, with a caveat)

```jsonc
{ "type": "image", "imageUrl": "https://...", "label": "Claude", "detail": "..." }
```

- `imageUrl` must be a real `http://` or `https://` URL. **No `data:`/`blob:`** —
  the reader rejects them.
- `label` is human-readable only and is **not stored** on the card. It's there
  so you and the user can keep track of which image is which. The user identifies
  the card in the editor by its image.
- `imageUrl` is a **placeholder/suggestion**. Do NOT download or upload. If you
  can't find a clean URL, switch the card to a text card.

### The cover-canvas caveat (why images are "user's local behavior")
At publish time the editor renders the whole board to a canvas for the cover
thumbnail. If a card image is a cross-origin URL whose host does **not** send
`Access-Control-Allow-Origin`, the canvas is "tainted" and the publish can fail.
The platform's own CDN images are CORS-clean; random external images often are
not. So:

- Treat the image URL you supply as a placeholder the user may swap.
- The user can replace any image in the editor with their own local file (the
  editor uploads it to the platform CDN → guaranteed clean).
- You do **not** handle any of this — it's the user's local editor action.
  Just supply a reasonable URL or use a text card.

## When to use which
- Label-style items (model names, dish names, book titles) → **text card**.
- Items where the image IS the point (a specific character, a logo, a photo) →
  image card, but warn the user they may want to swap it in the editor.
- Not sure → text card. The user can always add images themselves in the editor.
