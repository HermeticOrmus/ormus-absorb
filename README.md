# ormus-absorb

> Distill what a Claude session taught you into persistent memory. The learning ritual.

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
