# AGENTS.md — Coding Agent Playbook

This document defines how coding agents (Codex, etc.) should operate in this repository. It provides a safe, repeatable workflow tailored to this monorepo and the project’s conventions.

## Purpose
- Ensure consistent, minimal, and correct changes.
- Use the repo’s existing scripts for build, test, lint, and formatting.
- Respect safety, approvals, and sandbox settings.

## Quick Principles
- Prefer surgical diffs; don’t refactor unrelated code.
- Keep changes aligned with the current style and architecture.
- Fix root causes, not just symptoms, where scope allows.
- Do not commit or change versions unless explicitly requested.
- Never paste credentials, keys, or secrets. Do not fetch from network unless approved.

## Repo Snapshot
- Monorepo using npm workspaces under `packages/*`.
- Primary entry point for the CLI: `packages/cli/index.ts` → `packages/cli/src`.
- Tests use Vitest; integration tests live in `integration-tests/`.
- Node.js 20+ required.

## Allowed vs Disallowed
- Allowed:
  - Read files, analyze code, propose and apply patches within the workspace.
  - Run local builds/tests/linters via npm scripts (see below) when permitted.
  - Add focused tests alongside changed code, following existing patterns.
- Disallowed (unless explicitly approved):
  - Network access (package installs, external API calls, downloads).
  - Destructive actions (removing large folders, rewriting history).
  - Editing generated artifacts (e.g., `bundle/gemini.js`); edit sources instead.

## Workflow
1. Plan
   - For multi-step work, maintain a short, high-signal plan and update as you progress.
   - Group related shell actions under a single brief preamble.

2. Discover
   - Prefer ripgrep for search: `rg` or fallback as needed.
   - Read files in chunks (≤250 lines) to avoid truncated output.

3. Edit
   - Use `apply_patch` to add/update files; avoid noisy formatting-only diffs.
   - Follow local patterns; don’t add license headers unless asked.
   - Keep variable names descriptive; avoid single-letter names.

4. Validate
   - Use the npm scripts below. Start narrow (unit for changed package), then broader (workspace). Don’t fix unrelated failing tests.
   - Only add tests if a clear adjacent pattern exists. Keep tests co-located.

5. Hand-off
   - Summarize what changed, why, and any follow-ups. Reference files with clickable paths.

## Commands (root)
- Install deps: `npm ci`
- Build all: `npm run build`
- Bundle CLI: `npm run bundle`
- Test all workspaces: `npm test`
- CI test suite: `npm run test:ci`
- Integration tests (no sandbox): `npm run test:integration:sandbox:none`
- Lint (CI): `npm run lint:ci`
- Lint fix: `npm run lint:fix`
- Format: `npm run format`
- Typecheck: `npm run typecheck`
- Full gate (recommended before hand-off): `npm run preflight`

Notes:
- Do not edit `bundle/` directly; it’s built output.
- Some integration tests require sandbox tooling (Docker/Podman). Run only when permitted.

## File Referencing in Responses
- Use clickable repo-relative paths, optionally with 1-based line numbers:
  - `packages/cli/src/gemini.ts`
  - `packages/core/src/foo.ts:42`
- Avoid ranges and external URIs.

## Testing Guidance
- Framework: Vitest (`describe`, `it`, `expect`, `vi`).
- Placement: Co-locate test files with sources (`*.test.ts`, `*.test.tsx`).
- Mocks: Use `vi.mock(...)` at top when affecting module-level imports; use `vi.hoisted` if needed.
- React (Ink) components: Use `ink-testing-library` and assert using `lastFrame()`.
- Async: Prefer `async/await`; for timers, use `vi.useFakeTimers()` helpers.

## Code Style
- TypeScript-first where applicable.
- Prefer plain objects + TS types over classes unless the surrounding code uses classes.
- Keep effects and side-effects isolated; follow existing patterns in this repo.
- Use hyphenated CLI flags (e.g., `--my-flag`).

## Paths of Interest
- CLI entry: `packages/cli/index.ts`
- CLI UI/commands: `packages/cli/src/ui/commands/*`
- Core services/tools: `packages/core/src/*`
- Integration tests: `integration-tests/*`
- Docs: `docs/*`
- Repo-wide README: `README.md`
- Project context file: `GEMINI.md`

## Environment & Sandbox
- Node: `>=20` (see `package.json:engines`).
- Sandbox: `GEMINI_SANDBOX` can be `false`, `docker`, or `podman` (see `docs/sandbox.md`).
- Avoid building/pulling sandbox images unless approved.

## Validation Checklist (before finishing)
- Changes are minimal, scoped, and consistent with code style.
- New/updated tests added if valuable and patterned.
- `npm run build` passes for changed packages.
- `npm test` (or narrower) passes for changed area; `npm run preflight` when feasible.
- No edits to generated outputs or unrelated files.
- Clear hand-off summary with file references and next steps.

