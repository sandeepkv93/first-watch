# First Watch

**Repository Onboarding Skill for AI Coding Agents**

First Watch is a cross-client skill that performs a thorough first-session repository survey and produces reusable briefing artifacts. It helps users and agents establish architecture awareness, test/build/lint workflows, coding conventions, and risk boundaries before implementation work begins.

## Overview

First Watch serves as an **onboarding and context bootstrap layer**. It scans a repository, summarizes current state, and caches findings so subsequent sessions can start with accurate context.

It produces:

1. **Human briefing**: `repo-brief.md`
2. **Machine facts**: `repo-brief.json`

Cache location is selected in this order:

1. `.codex/`
2. `.claude/`
3. `.agent-cache/` (fallback)

## Key Features

- **First-session deep survey**: structure, manifests, scripts, CI, and docs
- **Staleness-aware cache reuse**: regenerate when HEAD or key files change
- **Safe reconnaissance defaults**: non-destructive read-only analysis
- **Actionable output contract**: clear findings, confidence, unknowns, and next steps
- **Cross-client compatibility**: works for both Codex and Claude skill directories

## Installation

### Option A: Clone directly

#### Codex

```bash
git clone https://github.com/sandeepkv93/first-watch.git ~/.codex/skills/first-watch
```

#### Claude

```bash
git clone https://github.com/sandeepkv93/first-watch.git ~/.claude/skills/first-watch
```

### Option B: Install script (recommended)

```bash
git clone https://github.com/sandeepkv93/first-watch.git
cd first-watch
./scripts/install.sh both
```

Supported targets:

- `./scripts/install.sh codex`
- `./scripts/install.sh claude`
- `./scripts/install.sh both`

## Usage

Invoke the skill by name in your client:

- `first-watch <optional scope>`

Typical usage:

- "Run first-watch in deep mode for this repo"
- "Use first-watch and refresh the cached repo brief"

## Workflow Summary

1. **Establish repository context**
- repo root, branch, HEAD, git status, remotes

2. **Freshness check**
- reuse cache only if still valid

3. **Repository survey**
- structure, entrypoints, manifests, test/lint/build workflows, docs

4. **Generate briefing artifacts**
- `repo-brief.md` and `repo-brief.json`

5. **Return concise execution summary**
- action, scope, result, confidence, next step

## Output Expectations

`repo-brief.md` sections:

1. Repository Snapshot
2. Architecture Map
3. Build/Test/Lint/Run Commands
4. Coding & Layout Conventions
5. Dependency & Runtime Notes
6. Risk Boundaries (Do-not-break)
7. Open Unknowns
8. Suggested First Tasks

`repo-brief.json` includes structured keys for head commit, manifests, commands, architecture, conventions, and risk boundaries.

## File Structure

```text
first-watch/
├── SKILL.md
├── README.md
├── agents/
│   └── openai.yaml
└── scripts/
    └── install.sh
```

## Compatibility Notes

- Primary behavior is defined in `SKILL.md` and is client-agnostic.
- `agents/openai.yaml` provides Codex/OpenAI UI metadata and does not prevent Claude usage.

## License

MIT License
