# Worked Example — AI Coding Models Tier List

A full sample run showing the conversation and the resulting `.tiervibe.json`.
This topic (AI models/tools) overlaps highly with TierVibe's audience — a good
default example.

## The interview

> **Agent**: What's the list about?
> **User**: AI coding models.
> **Agent**: How many tiers, and which preset? henz (夯→拉完了, 5 tiers),
> love (Love→Dislike), or custom?
> **User**: henz, 5 tiers.
> **Agent**: Light or dark board? henz defaults to 50 (light gray).
> **User**: Dark — 0.
> **Agent**: Which models, and where do they rank? Give me the items and I'll
> draft tiers + explanations.
> **User**: GPT-4, Claude, Gemini, Copilot, Codex, Llama, Mistral, Qwen.
> GPT-4 and Claude top, Llama and Qwen lower.

The agent drafts tiers, assigns cards, writes explanations, and emits the file.

## The resulting `ai-coding-models.tiervibe.json`

```json
{
  "title": "AI 编程模型 — 从夯到拉",
  "bgBrightness": 0,
  "layoutMode": "horizontal",
  "tiers": [
    {
      "name": "夯",
      "color": "#FF0000",
      "fontSize": 72,
      "cards": [
        { "type": "text", "text": "Claude", "textColor": "#ffffff", "bgColor": "#e11d48",
          "detail": "## 夯\n长上下文推理和谨慎改代码最稳。多文件重构、边界情况都强。\n\n- 跨文件改动可靠\n- prose 与代码都好" },
        { "type": "text", "text": "GPT-4", "textColor": "#ffffff", "bgColor": "#dc2626",
          "detail": "全能型,贵但硬任务最可靠。一次性大改首选。" }
      ]
    },
    {
      "name": "顶级",
      "color": "#FFC000",
      "fontSize": 56,
      "cards": [
        { "type": "text", "text": "Gemini", "detail": "超长上下文是杀手锏,代码质量中上。" },
        { "type": "text", "text": "Codex", "detail": "命令行代理跑得顺,适合循环式改代码。" }
      ]
    },
    {
      "name": "人上人",
      "color": "#FFFF00",
      "fontSize": 36,
      "cards": [
        { "type": "text", "text": "Copilot", "detail": "IDE 内补全体验最好,但自主性弱。" }
      ]
    },
    {
      "name": "NPC",
      "color": "#FFF2CC",
      "fontSize": 48,
      "cards": [
        { "type": "text", "text": "Mistral", "detail": "开源里性价比不错,代码偶有翻车。" }
      ]
    },
    {
      "name": "拉完了",
      "color": "#FFFFFF",
      "fontSize": 36,
      "cards": [
        { "type": "text", "text": "Llama", "textColor": "#1f1f1f", "bgColor": "#9ca3af", "detail": "本地跑可以,代码任务明显落后。" },
        { "type": "text", "text": "Qwen", "textColor": "#1f1f1f", "bgColor": "#6b7280", "detail": "中文场景还行,代码深度不够。" }
      ]
    }
  ],
  "candidates": []
}
```

## Step 6 message (Chinese)

> 文件已生成:`/path/to/ai-coding-models.tiervibe.json`
> 1. 打开 https://tiervibe.com/t/import
> 2. 点击中间区域,选择 `ai-coding-models.tiervibe.json`(或拖进去)
> 3. 如果还没登录,网站会提示登录(Google 一键或邮箱)——这是唯一需要登录的步骤
> 4. 进入编辑器,5 个层级区 + 8 张卡片 + 讲解都已填好
> 5. 拖动卡片排好顺序,点「发布」即可

## Notes
- All cards are text cards — zero network/CORS risk, import is instant.
- `bgBrightness: 0` overrides the henz default of 50 (user wanted dark).
- No `candidates` — the user ranked everything; an empty array is fine (or omit it).
