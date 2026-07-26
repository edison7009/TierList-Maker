# The Import Flow (final-step wording)

What actually happens at `https://tiervibe.com/t/import`, so your final message
matches reality.

**The primary path is auto-open, not a file drop.** `SKILL.md` → "Emit + auto-open"
is the source of truth: you base64 the JSON into `#data=` and run one OS open
command. Do NOT tell the user a file was generated or ask them to drag anything —
the file drop below exists only as the oversized-URL fallback.

## Path A — auto-open via `#data=` (default)

You run the OS open command; the user's default browser lands on
`/t/import#data=<urlencoded-base64>`. Then:

- The page reads the hash on mount, base64-decodes it as UTF-8, validates it, and
  navigates straight to `/t/new` with tiers, cards, commentary, and background
  already filled in. No drop zone, no file picker.
- On a validation error it shows the message and stays put. Nothing is sent to
  the server.
- Past 2,000,000 characters of `#data=` it refuses with a "too large" error —
  but your shell's command-line limit will bind long before that. See the table
  in `SKILL.md`.

### The login detour

`/t/import` is auth-protected. If the user isn't logged in, they're bounced to
`/login`, and the pending board rides along in the router's location state.

- **Email login returns to the board.** The handler navigates back to the saved
  location, hash intact, and the import runs.
- **⚠️ Google login currently loses the board.** That handler navigates to `/`
  instead of the saved location, so the payload is dropped and the user lands on
  the homepage with nothing. This is a site-side bug, not something you can work
  around from the skill.

So when the user may not be logged in, say so *before* they click:

> 如果网站提示登录,**请用邮箱登录** —— Google 一键登录目前会跳回首页,榜单数据会丢。

> If it asks you to log in, **use email** — Google sign-in currently bounces to
> the homepage and loses the board.

If they hit it anyway, nothing is lost on your side: re-run the same open command
once they're logged in and the board loads.

## Path B — file drop (fallback only, when the URL won't fit)

Use this only when the URL exceeds your shell's ceiling (see `SKILL.md`). Then the
page works as a classic drop zone:

- A centered drop zone: "点击选择或拖入文件" (click to choose or drop a file),
  accepting `.tiervibe.json`.
- Clicking opens a file picker; dragging the file onto the zone also works.
- While processing: a spinner, "正在导入…".
- Valid file → same destination as Path A: `/t/new`, fully populated.
- Invalid file → red error message, page stays put, nothing sent to the server.

## In the editor (both paths)

- Tiers and cards appear pre-filled; commentary is attached to its cards.
- The user's remaining job: **drag cards to sort them** into final positions (and
  swap any placeholder images for their own if they want).
- Then click **发布** (Publish) to post, or save as a draft.

## How to phrase the final step (templates)

In the user's language. **Path A — what you'll use almost every time:**

> 榜单已经在浏览器里打开了。
> 1. 如果提示登录,**用邮箱登录**(Google 登录目前会丢数据)
> 2. 层级、卡片、讲解都已经填好了
> 3. 拖动卡片排好最终顺序
> 4. 点「发布」

> Your board is open in the browser.
> 1. If it asks you to log in, **use email** (Google sign-in currently loses the board)
> 2. Tiers, cards, and commentary are already filled in
> 3. Drag the cards into your final order
> 4. Click 发布 (Publish)

**Path B — only if you had to fall back to a file:**

> 榜单太大,URL 放不下,所以存成了文件:`<absolute path>`
> 1. 打开 https://tiervibe.com/t/import
> 2. 点击中间区域选择这个文件(或直接拖进去)
> 3. 如果提示登录,用邮箱登录
> 4. 拖动卡片排好顺序,点「发布」

Do not add steps about uploading images or calling APIs — opening the board is
the only handoff.
