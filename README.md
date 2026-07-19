# TierList Maker

A portable Agent Skills plugin that helps a user author a **TierVibe** tier list and produce a `.tiervibe.json` they import at `https://tiervibe.com/t/import`.

The agent does all the work (interview, tiers, cards, explanations) and auto-opens the result in your browser. Nothing touches the TierVibe server until you click **Publish** in the editor — login only comes up at that final step.

> 中文版见 [README.zh-CN.md](README.zh-CN.md).

## About

**TierList Maker** helps you build a high-quality tier list on [TierVibe](https://tiervibe.com) — through a step-by-step conversation with an AI agent, not by hand.

- **Per-card commentary.** Every card carries its own markdown explanation — switch cards and the commentary changes. Readers get real reasoning, not just a ranked row.
- **Ask-then-generate.** The agent asks what you want (topic, tier titles, color style) one question at a time, then builds it. No fixed templates — your choices drive the design.
- **Auto-opens the result.** When it's done, the agent opens the finished board in your browser. You log in (only then), drag to sort, publish.
- **Works with the agents you already use** — Claude Code, ChatGPT, Codex, and any agentskills.io-compatible tool.
- **Local until you publish.** Nothing touches the TierVibe server until you click Publish.

## Demo

![TierList Maker demo](docs/tierlist-demo.png)

**One prompt → a publish-ready TierList, with commentary on every card.**

Ask the agent something like *"make a tier list for AI coding models"* and it runs the whole workflow for you:

1. Interviews the topic and tier count (henz / love / custom presets).
2. Sets up the tiers — names, title-bar colors, the board's global brightness.
3. Drafts every card (text cards with coordinated colors, or image placeholders).
4. Writes a markdown **commentary** on each card — the part that makes a tier list worth reading.
5. Emits a single `.tiervibe.json` and opens `https://tiervibe.com/t/import` with the board loaded.
6. You drag the cards into your final order and hit Publish. Login only happens here, at the last step.

No image hunting, no manual card entry, no blank cards. The agent does the labor; you keep the judgment.

## Repo layout (dual marketplace)

This repo ships **two** marketplace catalogs so the same plugin installs on both Claude Code and Codex/ChatGPT-style agent tools:

```
TierList-Maker/
├── .claude-plugin/
│   └── marketplace.json            # Claude Code marketplace catalog
├── .agents/plugins/
│   └── marketplace.json            # Codex / ChatGPT-style marketplace catalog
├── plugins/
│   └── tierlist-maker/
│       ├── .claude-plugin/
│       │   └── plugin.json         # Claude Code plugin manifest
│       ├── .codex-plugin/
│       │   └── plugin.json         # Codex plugin manifest (with logo)
│       └── skills/
│           └── tierlist-maker/
│               ├── SKILL.md        # the skill (entry point)
│               ├── references/    # data-schema, tier-config, text-cards, explanations, import-flow
│               ├── templates/     # blank / henz-5tier / text-only skeletons
│               ├── examples/      # worked AI-models example
│               └── assets/
│                   └── logo.svg   # TierVibe logo (icon asset)
└── README.md
```

## Install — Claude Code

Add this repo as a marketplace, then install the plugin:

```
/plugin marketplace add edison7009/TierList-Maker
/plugin install tierlist-maker@tiervibe-com
```

Skill auto-loads on triggers like "make a tier list for ...", or invoke as `/tiervibe-com:tierlist-maker`.

## Install — ChatGPT

1. Open ChatGPT → **Plugins**.
2. Click the **⬇️** icon (top-right).
3. Choose **Add plugin marketplace**.
4. Paste the repo URL: `https://github.com/edison7009/TierList-Maker.git`
5. Confirm; `tierlist-maker` appears in the plugin list — enable it.

Then say "make a tier list for X" in chat to trigger it. The AI asks you step by step (titles / style / items), then auto-opens tiervibe.com with the board loaded — you drag to sort and publish.

## Install — Codex (CLI)

The repo ships `.agents/plugins/marketplace.json` (Codex schema). Add and enable:

```
codex plugin marketplace add edison7009/TierList-Maker
```

## How it works (the bridge on the TierVibe side)

The TierVibe editor has a local reader at `/t/import` (no server port, no backend, no data-model change). It parses the `.tiervibe.json` client-side, flattens it into the editor's existing load format, reuses `TierDataService.loadCompleteData` (card sizing, text-card protocol, markdown explanations), and hands off to the editor via the existing `location.state.seed` injection path. The agent can also `start`/`open` a `https://tiervibe.com/t/import#data=<base64>` link so the board auto-loads with no file drag. The user then drags to sort and publishes.

## Icons

Neither marketplace manifest schema has an icon field (verified against the official Claude Code plugin reference and the Codex/Kimi-style marketplace samples). The TierVibe logo ships at `plugins/tierlist-maker/skills/tierlist-maker/assets/logo.svg` as a skill asset, and the Codex `.codex-plugin/plugin.json` points `interface.logo` at it. For the marketplace list UI, set the GitHub/GitCode repo's social preview image to the same logo.

## Scope / non-goals
- Only **creates new** lists via import. Does not edit/delete existing posts.
- No per-tier background — only the single global `bgBrightness` (0..100).
- No image uploading — image URLs in the file are placeholders; the user swaps them in the editor if they want.
