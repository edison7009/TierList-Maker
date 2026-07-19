# The Import Flow (Step 6 wording)

What the user sees at `https://tiervibe.com/t/import`, so your Step 6 message
matches reality.

## The page
- A centered drop zone: "点击选择或拖入文件" (click to choose or drop a file),
  accepting `.tiervibe.json`.
- Clicking opens a file picker; dragging the file onto the zone also works.
- While processing: a spinner, "正在导入…".

## Behavior
- If the user is **not logged in**: the route is protected, so they're bounced
  to `/login` first, then returned to `/t/import` after login. (Google one-click
  or email/password.) **This is the only time login comes up.**
- On a valid file: the page parses it, builds the editor state, and navigates to
  `/t/new` with everything filled in (tiers, cards, explanations, background).
- On an invalid file: a red error message appears and the page stays put. The
  user fixes the file and re-selects it. Nothing is sent to the server.

## In the editor (after import)
- Tiers and cards appear pre-filled; explanations are attached to their cards.
- The user's remaining job: **drag cards to sort them** into final positions
  (and swap any placeholder images for their own if they want).
- Then click **发布** (Publish) to post, or save as a draft.

## How to phrase Step 6 to the user (template)
In the user's language:

> 文件已生成:`<absolute path>`
> 接下来:
> 1. 打开 https://tiervibe.com/t/import
> 2. 点击中间区域,选择刚才的 `.tiervibe.json`(或直接拖进去)
> 3. 如果还没登录,网站会提示登录(Google 一键或邮箱)——这是唯一需要登录的步骤
> 4. 进入编辑器,层级区/卡片/讲解都已填好
> 5. 拖动卡片排好顺序,点「发布」即可

Adjust to English if the user is working in English:

> Your file is ready: `<absolute path>`
> 1. Open https://tiervibe.com/t/import
> 2. Click the drop zone and pick the `.tiervibe.json` (or drag it in)
> 3. If you're not logged in, the site will ask you to log in (Google one-click
>    or email) — that's the only login step
> 4. The editor opens with all tiers, cards, and explanations filled in
> 5. Drag the cards into your final order, then click 发布 (Publish)

Do not add steps about uploading images, calling APIs, or anything else — the
file is the deliverable and import is the only handoff.
