> **Note (2026-05-23) — Consolidated into LibreSessionFlow**
>
> This standalone has been folded into the umbrella repo
> [LibreSessionFlow-Claude-Code](https://github.com/HermeticOrmus/LibreSessionFlow-Claude-Code),
> alongside 9 sibling session-lifecycle plugins. The current version of this
> functionality lives at `plugins/absorb/` in the umbrella.
>
> This repo remains live as a landing page. New work happens in the umbrella.

---

<p align="center">
  <img src="https://ormus.solutions/mascot/golden_swan.gif" alt="ormus-absorb" width="128" style="image-rendering: pixelated;" />
</p>

<h1 align="center">ormus-absorb</h1>

<p align="center">
  <em>Distill what a Claude session taught you into persistent memory. The learning ritual.</em>
</p>

<p align="center">
  <a href="https://github.com/HermeticOrmus/ormus-absorb/stargazers"><img src="https://img.shields.io/github/stars/HermeticOrmus/ormus-absorb?style=flat-square&color=aa8142" alt="Stars" /></a>
  <a href="https://github.com/HermeticOrmus/ormus-absorb/blob/main/LICENSE"><img src="https://img.shields.io/github/license/HermeticOrmus/ormus-absorb?style=flat-square&color=aa8142" alt="License" /></a>
  <a href="https://github.com/HermeticOrmus/ormus-absorb/commits"><img src="https://img.shields.io/github/last-commit/HermeticOrmus/ormus-absorb?style=flat-square&color=aa8142" alt="Last Commit" /></a>
  <img src="https://img.shields.io/badge/Claude_Code-aa8142?style=flat-square&logo=anthropic&logoColor=white" alt="Claude Code" />
</p>

---

> **Distill what a Claude session taught you into persistent memory. The learning ritual.**

A Claude Code skill that extracts the non-obvious knowledge from a session — intent, vision, domain rules, architectural decisions, stakeholder context — and writes it to a structured memory store that future sessions can read.

## Why

Claude sessions teach you things the code can't. Why a project exists. Who the stakeholders are. The rule that governs an undocumented edge case. The reason a particular architectural choice was made. Without a place to put that knowledge, it evaporates the moment context clears.

`/absorb` is the place. It runs at the end of a deep session, classifies what you learned, and writes durable memory files that load into future sessions automatically.

## Install

```bash
git clone https://github.com/HermeticOrmus/ormus-absorb ~/.claude/skills/absorb
```

Or as a Claude Code plugin:

```bash
claude plugin marketplace add HermeticOrmus/ormus-absorb
```

Restart Claude Code so the skill registry picks it up.

### Memory location

By default, `/absorb` writes to `~/.claude/memory/`. Override with:

```bash
export CLAUDE_MEMORY_DIR="$HOME/my-claude-memory"
```

The directory is created on first use.

## Usage

```
/absorb                    — Full session distillation
/absorb @project:name      — Scope to a specific project
/absorb @dry               — Show what would be saved without writing
```

Best run before `/compact` or session end so understanding survives context compression.

## What gets absorbed

| Type | What it captures |
|---|---|
| **intent** | Why a project exists, who it serves |
| **direction** | Roadmap, phases, near-term priorities |
| **domain** | Business concepts, taxonomies, undocumented rules |
| **decision** | Architectural choices with the *why* |
| **vision** | Long-term aspiration, design philosophy |
| **person** | Stakeholder context, roles, preferences |
| **feedback** | How the user wants to work — validated approaches |
| **reference** | External resources, APIs, accounts |
| **infrastructure** | Services, ports, deployment topology |

## What doesn't get absorbed

- Code patterns or file paths (read the code)
- Git history (use `git log`)
- Debugging solutions (the fix is in the commit)
- Anything already in `CLAUDE.md`
- Ephemeral session state

The test: *"Would a future session benefit from knowing this, AND can't discover it from the codebase?"* If both yes, absorb it. Otherwise, no.

## Output structure

After absorbing, your memory directory looks like:

```
~/.claude/memory/
├── MEMORY.md                    ← index, one line per topic
├── project_<name>.md            ← per-project intent/vision/decisions
├── feedback_<topic>.md          ← workflow preferences ("Why" + "How to apply")
└── reference_<topic>.md         ← external resource pointers
```

Each file has YAML frontmatter (`name`, `description`, `type`) so future Claude sessions can find what's relevant.

## How it works (4-phase distillation)

1. **NIGREDO** — Scan the conversation, classify every insight, drop noise and duplicates
2. **ALBEDO** — Read existing memory, decide whether each insight is new / extends / contradicts / duplicate
3. **CITRINITAS** — Structure into the right file (project/feedback/reference) with the right schema
4. **RUBEDO** — Write, verify coherence, report

The naming follows the alchemical four-stage process — fitting, since the skill literally distills experience into a more refined form.

## Pairs with

- **[ormus-handoff](https://github.com/HermeticOrmus/ormus-handoff)** — captures task state for the next session. /absorb captures understanding. Run both at session end.
- **[ormus-pickup](https://github.com/HermeticOrmus/ormus-pickup)** — restores context from a HANDOFF file. Future sessions also load whatever /absorb persisted.
- **[ormus-explore](https://github.com/HermeticOrmus/ormus-explore)** — token-efficient AST-based code search.
- **[ormus-vibe-proof](https://github.com/HermeticOrmus/ormus-vibe-proof)** — security hardening for vibe-coded full-stack apps.
- **[ormus-meta-prompting](https://github.com/HermeticOrmus/ormus-meta-prompting)** — categorical foundations for AI prompt engineering.

Together they form the **ormus session lifecycle** — composable Claude Code skills for doing serious work across days, machines, and context resets.

## License

MIT. See [LICENSE](LICENSE).

## Origin

Distilled from real long-running Claude Code engagements where every session ended with valuable understanding evaporating. The nine-type classification taxonomy (intent / direction / domain / decision / vision / person / feedback / reference / infrastructure) emerged empirically — those types covered every "I wish I had written this down" moment.

---

## Part of the Libre Open-Source Stack for Claude Code

This repository is part of a growing family of open-source toolkits for Claude Code.

### Libre suite — comprehensive plugin bundles

- [LibreUIUX-Claude-Code](https://github.com/HermeticOrmus/LibreUIUX-Claude-Code) — UI/UX development (152 agents, 70 plugins, 76 commands, 74 skills)
- [LibreArch-Claude-Code](https://github.com/HermeticOrmus/LibreArch-Claude-Code) — Software architecture and system design
- [LibreCopy-Claude-Code](https://github.com/HermeticOrmus/LibreCopy-Claude-Code) — Technical writing and documentation engineering
- [LibreDevOps-Claude-Code](https://github.com/HermeticOrmus/LibreDevOps-Claude-Code) — DevOps engineering and infrastructure automation
- [LibreEmbed-Claude-Code](https://github.com/HermeticOrmus/LibreEmbed-Claude-Code) — Embedded systems, firmware, and IoT development
- [LibreFinTech-Claude-Code](https://github.com/HermeticOrmus/LibreFinTech-Claude-Code) — Financial technology development
- [LibreGEO-Claude-Code](https://github.com/HermeticOrmus/LibreGEO-Claude-Code) — AI-search optimization (ChatGPT, Perplexity, Gemini, Google AI Overviews)
- [LibreGameDev-Claude-Code](https://github.com/HermeticOrmus/LibreGameDev-Claude-Code) — Game development across Godot, Unity, Unreal
- [LibreMLOps-Claude-Code](https://github.com/HermeticOrmus/LibreMLOps-Claude-Code) — ML engineering and AI operations
- [LibreMobileDev-Claude-Code](https://github.com/HermeticOrmus/LibreMobileDev-Claude-Code) — Mobile app development (Flutter, React Native, native iOS, native Android)
- [LibreSecOps-Claude-Code](https://github.com/HermeticOrmus/LibreSecOps-Claude-Code) — Security operations

### Skills mini-repos — single CLAUDE.md drop-ins

- [vibe-engineer-skills](https://github.com/HermeticOrmus/vibe-engineer-skills) — Direct AI codegen well (hypothesis → scope → validate → reject working-but-wrong)
- [markdown-discipline-skills](https://github.com/HermeticOrmus/markdown-discipline-skills) — Strip AI-slop from markdown (no em dashes, no marketing fluff)
- [shell-safety-skills](https://github.com/HermeticOrmus/shell-safety-skills) — `set -euo pipefail` discipline + 15 failure-mode examples
- [commit-standard-skills](https://github.com/HermeticOrmus/commit-standard-skills) — Ormus Commit Standard v1.0 + commit-msg hook + commitlint
- [unwoke-skills](https://github.com/HermeticOrmus/unwoke-skills) — Strip AI theater (ten sins to eliminate, symmetric engagement)
- [python-conventions-skills](https://github.com/HermeticOrmus/python-conventions-skills) — Modern Python 3.11+ (types, pathlib, async, ruff, mypy, uv)
- [typescript-conventions-skills](https://github.com/HermeticOrmus/typescript-conventions-skills) — TypeScript strict mode, discriminated unions, Result types
- [hermetic-laws-skills](https://github.com/HermeticOrmus/hermetic-laws-skills) — Seven Hermetic Principles applied to engineering
- [riper-workflow-skills](https://github.com/HermeticOrmus/riper-workflow-skills) — Research / Innovate / Plan / Execute / Review systematic dev
- [six-day-cycle-skills](https://github.com/HermeticOrmus/six-day-cycle-skills) — Sustainable shipping cadence with mandatory rest
- [token-optimization-skills](https://github.com/HermeticOrmus/token-optimization-skills) — Claude Code token + context optimization
- [osint-skills](https://github.com/HermeticOrmus/osint-skills) — OSINT research methodology (multi-wave investigative spiral)
- [calcinate-skills](https://github.com/HermeticOrmus/calcinate-skills) — Stage 1 of the Magnum Opus (burn project bloat)
- [claude-md-overhaul-skills](https://github.com/HermeticOrmus/claude-md-overhaul-skills) — Audit CLAUDE.md and MEMORY.md against caps
- [session-handoff-skills](https://github.com/HermeticOrmus/session-handoff-skills) — Session handoff + pickup discipline
- [naming-skills](https://github.com/HermeticOrmus/naming-skills) — Product naming methodology (mine the brand's vocabulary)
- [magnum-opus-skills](https://github.com/HermeticOrmus/magnum-opus-skills) — Seven-stage alchemy applied to project transformation

### Template source

- [andrej-karpathy-skills](https://github.com/HermeticOrmus/andrej-karpathy-skills) — the canonical single-file CLAUDE.md pattern (fork of jiayuan_jy's original)

Star the family, not just one — that's how the suite stays coherent.
