---
name: first-watch
description: Build and cache a first-session repository briefing with architecture, conventions, test/lint/build workflows, and risk boundaries. Use when entering a repo for the first time, when repo context is missing, or when cached repo context is stale after HEAD/config changes.
---

# First Watch

Create a reliable repository briefing before implementation work.

## Workflow

### 1) Establish Repository Context

- Resolve repository root from current working directory.
- Capture:
  - active branch
  - HEAD commit
  - `git status --short` summary
  - remotes (names only)
- If not in a git repo, stop and report that this skill requires a git repository.

### 2) Freshness Check

Look for cached artifacts in `.codex/`:

- `.codex/repo-brief.json`
- `.codex/repo-brief.md`

If cache exists and all checks pass, load and use cache:

- cached `head_commit` equals current `HEAD`
- no changes to key files:
  - root `README*`
  - `AGENTS.md`
  - manifests (`go.mod`, `package.json`, `pyproject.toml`, `Cargo.toml`, `pom.xml`, `build.gradle*`, `WORKSPACE*`, `MODULE.bazel`)
  - CI files (`.github/workflows/*`, other pipeline config)

If any check fails, regenerate briefing.

### 3) Repository Survey (Thorough but Safe)

Gather facts without destructive commands.

- Structure:
  - top-level directories/files
  - likely app entrypoints (`cmd/`, `src/`, `app/`, `main.*`, `server.*`)
  - major modules/packages
- Stack/manifests:
  - detect languages/frameworks from manifest files
  - detect container/runtime files (`Dockerfile*`, compose files, k8s dirs)
- Quality workflow:
  - find test/lint/build scripts and commands from:
    - `Makefile`, `Taskfile*`, package scripts, README, CI workflows
  - list likely canonical commands
- Engineering patterns:
  - naming/layout conventions
  - layering boundaries (api/service/repo/etc.)
  - config patterns (env, flags, config files)
- Documentation sweep:
  - `README*`, `AGENTS.md`, design docs, contribution docs
- Safety:
  - never run destructive git commands
  - never change files during scan

### 4) Build Repo Briefing

Produce two outputs:

1. Human briefing: `.codex/repo-brief.md`
2. Machine facts: `.codex/repo-brief.json`

Both must include:

- repo_root
- branch
- head_commit
- generated_at (ISO-8601)
- scan_mode (`quick` or `deep`)
- confidence (`High` / `Medium` / `Low`)
- unknowns
- key assumptions (if any)

### 5) Required Briefing Sections (`repo-brief.md`)

Use this exact section order:

1. Repository Snapshot
2. Architecture Map
3. Build/Test/Lint/Run Commands
4. Coding & Layout Conventions
5. Dependency & Runtime Notes
6. Risk Boundaries (Do-not-break)
7. Open Unknowns
8. Suggested First Tasks

Keep concise and actionable.

### 6) Output Contract (Chat Response)

Return:

- `Action`: generated cache or reused cache
- `Scope`: repo root + branch + HEAD
- `Result`: key findings summary
- `Confidence`: High/Medium/Low
- `Next step`: what should happen before code edits (if anything)

## Scan Modes

### quick
Use when user asks for speed. Limit depth but still produce all required sections.

### deep
Default for first-time onboarding. Perform broad survey across code, scripts, docs, and CI for higher-confidence briefing.

## Guardrails

- Do not infer commands if discoverable commands exist.
- Mark unknowns explicitly instead of guessing.
- If multiple build systems coexist, list each and identify likely primary one.
- If monorepo detected, include subproject map and highlight likely target ambiguity.
- If ambiguity is critical for implementation, ask one focused disambiguation question after briefing.

## Minimal JSON Shape (`repo-brief.json`)

Use keys:

- `repo_root`
- `branch`
- `head_commit`
- `generated_at`
- `scan_mode`
- `confidence`
- `unknowns` (array)
- `manifests` (array of objects)
- `commands` (object with `build|test|lint|run` arrays)
- `architecture` (object)
- `conventions` (array)
- `risk_boundaries` (array)
- `suggested_first_tasks` (array)
