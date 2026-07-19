# TierList Maker

A portable Agent Skills plugin that helps a user author a **TierVibe** tier list and produce a `.tiervibe.json` they import at `https://tiervibe.com/t/import`.

The agent does all the work (interview, tiers, cards, explanations) and outputs one local file. Nothing touches the TierVibe server until the user clicks **发布 (Publish)** in the editor — login only comes up at that final import step.

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
/plugin marketplace add <your-github-org>/TierList-Maker
/plugin install tierlist-maker@tiervibe-tierlist
```

Skill is invoked as `/tiervibe-tierlist:tierlist-maker` or auto-loads on triggers like "make a tier list for ...".

## Install — Codex / ChatGPT-style

The repo contains `.agents/plugins/marketplace.json`. Add it per your tool's marketplace-add command, then enable the `tierlist-maker` plugin. The same `SKILL.md` drives the behavior.

## How it works (the bridge on the TierVibe side)

The TierVibe editor has a local reader at `/t/import` (no server port, no backend, no data-model change). It parses the `.tiervibe.json` client-side, flattens it into the editor's existing load format, reuses `TierDataService.loadCompleteData` (card sizing, text-card protocol, markdown explanations), and hands off to the editor via the existing `location.state.seed` injection path. The user then drags to sort and publishes.

## Icons

Neither marketplace manifest schema has an icon field (verified against the official Claude Code plugin reference and the Codex/Kimi-style marketplace samples). The TierVibe logo ships at `plugins/tierlist-maker/skills/tierlist-maker/assets/logo.svg` as a skill asset; for the marketplace list UI, set the GitHub repo's social preview image to the same logo.

## Scope / non-goals
- Only **creates new** lists via import. Does not edit/delete existing posts.
- No per-tier background — only the single global `bgBrightness` (0..100).
- No image uploading — image URLs in the file are placeholders; the user swaps them in the editor if they want.
