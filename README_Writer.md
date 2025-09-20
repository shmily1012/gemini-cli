# Novel Writing Assistant — Usage Guide

This guide explains how to use the Gemini CLI in this repository as a novel writing assistant. It covers setup, workflow, commands, and tips for multi‑genre web fiction.

## What You Get
- Opinionated project layout for fiction writing
- Long‑context model configuration for sustained world/character consistency
- Custom slash commands for planning, drafting, revising, and consistency checks

## Prerequisites
- Node.js 20+
- Gemini CLI available in your shell as `gemini`
- This repo contains ready‑made config and commands; you just run the CLI in this folder

## Quick Start
- Launch: `gemini`
- Use commands in the interactive prompt:
  - `/novel:plan logline` — generate hook options and update the outline’s “卖点” section
  - `/novel:plan outline` — build or refresh the three‑act outline
  - `/novel:plan beats` — propose next-arc beat plans without touching the outline directly
  - `/novel:write chapter ch01 [补充指令]` — draft or expand a chapter with optional guidance
  - `/novel:write revise ch01` — checklist-based polishing that overwrites the chapter after preview
  - `/novel:lore character 主角名` — create or refine a character card
  - `/novel:review consistency` — scan bible/outline/chapters for contradictions

## Project Layout
- `novel/WRITER.md:1` — Style, genre presets, output formats, quality checklists
- `novel/outline.md:1` — Logline, themes, three‑act plan, per‑chapter beats
- `novel/bible/characters.md:1` — Character cards (Goal/Motivation/Conflict)
- `novel/bible/world.md:1` — World rules, factions, timeline, systems
- `novel/bible/themes.md:1` — Themes, motifs, tone
- `novel/chapters/` — Chapter drafts `chNN.md` and their `_revN` revisions
- `novel/review/` — Consistency reports and QA notes

## Configuration
- `.gemini/settings.json:1`
  - `model.name`: default `gemini-2.5-pro` for long context
  - `context.fileName`: includes `GEMINI.md` and `novel/WRITER.md`
  - `context.includeDirectories`: includes `novel/`
  - You can switch models temporarily via the `-m` flag when launching: `gemini -m gemini-2.5-flash`

## Commands
- `/novel:plan <logline|outline|beats> [说明]`
  - `logline`: offers 1–3 hook options, tags their selling points, and updates the outline’s “卖点”小节。
  - `outline`: incrementally refreshes the three-act plan while preserving chapter numbering.
  - `beats`: drafts the next 6–10 chapter beats, writes to `novel/plan/beats_next.md`, and lists merge suggestions instead of editing the outline directly.
- `/novel:write <chapter|revise> <chNN> [补充指令]`
  - `chapter`: drafts or expands a chapter, auto-backups `_revN` files, and can call web search when real-world detail is needed.
  - `revise`: runs the full polish checklist, outputs a change summary, and overwrites the chapter after confirmation.
- `/novel:lore <character|world|glossary> [...]`
  - `character`: adds or updates character cards under `novel/bible/characters.md`.
  - `world`: grows `novel/bible/world.md` without losing hand-written sections, surfacing conflicts if found.
  - `glossary`: aggregates terminology into `novel/bible/glossary.md`, including a “待统一” list when naming drifts appear.
- `/novel:review <consistency|pov|timeline|summary|editorial>`
  - `consistency`: issues a contradiction report at `novel/review/consistency.md`.
  - `pov`: audits POV balance and recommendations in `novel/review/pov.md`.
  - `timeline`: normalizes chronology into `novel/bible/timeline.md` and flags time conflicts.
  - `summary`: captures story/state snapshots in `novel/review/summary.md`.
  - `editorial`: performs an editorial review and writes to `novel/review/editorial.md`.

## Suggested Workflow
- Seed: `/novel:plan logline` → `/novel:plan outline` → `/novel:plan beats`
- Enrich: `/novel:lore character 主角名`（视需要补充 `/novel:lore world` / `glossary`）
- Draft: `/novel:write chapter ch01`（按最新 beats 与补充指令写作，可触发网络调研）
- Polish: `/novel:write revise ch01`
- Guardrails: `/novel:review consistency` + `/novel:review timeline/pov` 按需巡检

## Multi‑Genre Tuning
- Put genre presets and do/don’t lists in `novel/WRITER.md`
- Pass high‑level guidance as arguments to commands (e.g., chapter pacing, target word count)
- Keep chapters 1500–3000 Chinese characters for web serialization, adjust per genre

## Safety and Backups
- Writes go through CLI’s confirm‑and‑diff flow
- Chapters auto‑backup to `*_revN.md` before overwrite
- Keep major structural changes in outline and bible under version control

## Tips
- Use `/compress` to keep long sessions responsive
- Split scenes with clear headers to improve revision quality
- Keep character G/M/C fields current; many consistency issues stem from drift there

## FAQ
- Do I need to change code? No — this assistant is fully configured via project files and custom commands
- Where are the commands? `.gemini/commands/novel/*.toml`
- Can I add new commands? Yes — add TOML files under `.gemini/commands/` and follow the existing patterns

## File Index
- `.gemini/settings.json:1`
- `.gemini/commands/novel/plan.toml:1`
- `.gemini/commands/novel/write.toml:1`
- `.gemini/commands/novel/lore.toml:1`
- `.gemini/commands/novel/review.toml:1`
- `novel/WRITER.md:1`
- `novel/outline.md:1`
- `novel/bible/characters.md:1`
- `novel/bible/world.md:1`
- `novel/bible/themes.md:1`
