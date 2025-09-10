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
  - `/novel:logline` — generate hook and update outline
  - `/novel:outline` — build/refresh three‑act outline and beats
  - `/novel:character 主角名` — create or refine a character card
  - `/novel:chapter ch01 [节奏/长度说明]` — draft or expand a chapter
  - `/novel:revise ch01` — checklist‑based polishing, writes `_revN`
  - `/novel:consistency` — cross‑check bible/outline/chapters

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
- `/novel:logline`
  - Generates 1–3 loglines and writes the chosen one to `novel/outline.md`
- `/novel:outline`
  - Creates or incrementally updates the three‑act outline and per‑chapter beats
  - Preserves existing chapter numbering (`chNN`)
- `/novel:character <角色名>`
  - Adds or refines a character section under `## <角色名>` in `novel/bible/characters.md`
- `/novel:chapter <chNN> [补充指令]`
  - Drafts/expands the chapter using outline + bible; auto‑backs up previous version to `*_revN.md` before overwriting `chNN.md`
  - Optional notes (e.g., “紧张节奏 2k字 双POV”) guide the draft
- `/novel:revise <chNN>`
  - Checklist‑based polish; writes refined text to `novel/chapters/chNN_revN.md`
- `/novel:consistency`
  - Scans bible/outline/chapters and writes `novel/review/consistency.md` with issues and fixes

## Suggested Workflow
- Seed: `/novel:logline` → `/novel:outline`
- Enrich: `/novel:character 主角名`（反复）+ 更新 `world.md`/`themes.md`
- Draft: `/novel:chapter ch01`（按 beats 写作）
- Polish: `/novel:revise ch01`（自动备份 `_revN`）
- Guardrails: `/novel:consistency`（定期跑，修正设定/时间线问题）

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
- `.gemini/commands/novel/logline.toml:1`
- `.gemini/commands/novel/outline.toml:1`
- `.gemini/commands/novel/character.toml:1`
- `.gemini/commands/novel/chapter.toml:1`
- `.gemini/commands/novel/revise.toml:1`
- `.gemini/commands/novel/consistency.toml:1`
- `novel/WRITER.md:1`
- `novel/outline.md:1`
- `novel/bible/characters.md:1`
- `novel/bible/world.md:1`
- `novel/bible/themes.md:1`
