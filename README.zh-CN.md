# TierList Maker

一个可移植的 Agent Skills 插件,帮你用 AI **制作高质量的 TierVibe 榜单**,并产出一份 `.tiervibe.json`,在 `https://tiervibe.com/t/import` 一键导入。

agent 负责全部劳动(采访、层级、卡片、讲解),最后**自动打开浏览器**把榜单加载出来。在你点「发布」之前,所有数据都在本地,不碰 TierVibe 服务器;登录只在最后那一步出现。

> 英文版见 [README.md](README.md)。

## 关于

**TierList Maker** 通过与 AI agent **一问一答**的方式,帮你做出一个 TierVibe 榜单——不是手动拼。

- **逐卡讲解(讲解模式)。** 每张卡片都带自己的 markdown 讲解;切换不同卡片,讲解跟着变。读者看到的是真正的理由,而不只是一排排名。
- **先问再做。** agent 一步步问你(题材、层级标题、配色风格),再动手生成。没有固定模板——你的选择决定设计。
- **做完自动开浏览器。** 生成完毕,agent 直接打开榜单,你登录(只此一次)、拖拽排序、发布。
- **支持你常用的 Agent 工具** —— Claude Code、ChatGPT、Codex,以及任何 agentskills.io 兼容工具。
- **发布前全本地。** 不点发布,什么都不进 TierVibe 服务器。

## 演示

![TierList Maker 演示](docs/tierlist-demo.png)

**一句话 → 一份带讲解、可发布的榜单。**

跟 agent 说「给 AI 编程模型做个 tier list」,它会:

1. 问清题材和层数(henz / love / 自定义预设,默认 `T1..Tn`)。
2. 设好每个层级——标题、标题条颜色、全局背景明暗。
3. 建好每张卡片(文字卡可配色,或图片占位)。
4. 给每张卡写 markdown **讲解**——让榜单值得一读。
5. 产出一个 `.tiervibe.json`,自动打开 `https://tiervibe.com/t/import`。
6. 你把卡片拖到最终位置,点「发布」。登录只在最后这步。

不用找图、不用手动加卡、不会出现空卡。agent 干活,你做判断。

## 仓库结构(双市场)

本仓库同时提供**两份** marketplace 清单,同一插件可装在 Claude Code 和 Codex/ChatGPT 风格的 agent 工具上:

```
TierList-Maker/
├── .claude-plugin/
│   └── marketplace.json            # Claude Code 市场清单
├── .agents/plugins/
│   └── marketplace.json            # Codex / ChatGPT 风格市场清单
├── plugins/
│   └── tierlist-maker/
│       ├── .claude-plugin/
│       │   └── plugin.json         # Claude Code 插件清单
│       ├── .codex-plugin/
│       │   └── plugin.json         # Codex 插件清单(含 logo)
│       └── skills/
│           └── tierlist-maker/
│               ├── SKILL.md        # skill 入口
│               ├── references/    # data-schema / tier-config / text-cards / explanations / import-flow
│               ├── templates/     # blank / henz-5tier / text-only 骨架
│               ├── examples/      # AI 模型范例
│               └── assets/
│                   └── logo.svg   # TierVibe logo
└── README.md
```

## 安装 — Claude Code

把这个仓库加为 marketplace,再装插件:

```
/plugin marketplace add edison7009/TierList-Maker
/plugin install tierlist-maker@tiervibe-com
```

skill 在「给 X 做个 tier list」这类话触发自动加载,或用 `/tiervibe-com:tierlist-maker` 显式调用。

## 安装 — ChatGPT

1. 打开 ChatGPT → **插件**。
2. 右上角点 **「⬇️」**(下载图标)。
3. 选 **添加插件市场**。
4. 粘贴仓库地址:`https://github.com/edison7009/TierList-Maker.git`
5. 确认后,`tierlist-maker` 出现在插件列表,启用即可。

然后在对话里说「给 X 做个 tier list」触发;AI 一步步问你(标题/风格/项),最后自动打开 tiervibe.com 显示榜单,你拖拽排序、发布。

## 安装 — Codex (CLI)

仓库含 `.agents/plugins/marketplace.json`(Codex schema):

```
codex plugin marketplace add edison7009/TierList-Maker
```

## 工作原理(TierVibe 侧的衔接)

TierVibe 编辑器在 `/t/import` 有一个本地读取器(不开新端口、不动后端、不改数据契约)。它在浏览器里解析 `.tiervibe.json`,翻成编辑器内部加载格式,复用 `TierDataService.loadCompleteData`(卡片尺寸、文字卡协议、markdown 讲解),通过现有的 `location.state.seed` 注入路径交给编辑器。agent 还可以直接 `start`/`open` 一个 `https://tiervibe.com/t/import#data=<base64>` 链接,榜单自动加载,无需拖文件。

## 图标

两份 marketplace 清单都没有 icon 字段(已在官方 Claude Code plugin 文档和 Codex/Kimi 范例里确认)。TierVibe logo 作为 skill 资源放在 `plugins/tierlist-maker/skills/tierlist-maker/assets/logo.svg`,Codex 端的 `.codex-plugin/plugin.json` 用 `interface.logo` 指向它;列表里的图标则靠 GitHub/GitCode 仓库的社交预览图。

## 范围 / 不做
- 只**创建新榜单**(导入)。不编辑/删除已有帖子。
- 没有 per-tier 背景色——只有单个全局 `bgBrightness`(0..100)。
- 不上传图片——文件里的图片 URL 是占位,需要的话用户在编辑器里换。
