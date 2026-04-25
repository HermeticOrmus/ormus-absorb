---
name: absorb
description: Distill session knowledge into persistent memory — project intent, domain understanding, architectural decisions, and vision. The learning ritual.
user_invocable: true
version: 1.0.0
tags:
  - memory
  - learning
  - distillation
---

# /absorb — Session Knowledge Distillation

> "What did this session teach us that no file in the repo can?"

Extract project understanding from the current session and persist it to memory. Not code patterns — those live in the code. This captures **intent, direction, vision, domain knowledge, and decisions** that would otherwise evaporate when context clears.

## Memory Location

By default, this skill writes to `~/.claude/memory/`. To use a different location, set the `CLAUDE_MEMORY_DIR` environment variable:

```bash
export CLAUDE_MEMORY_DIR="$HOME/my-claude-memory"
```

Throughout this skill, `$MEMORY` refers to that directory (`${CLAUDE_MEMORY_DIR:-$HOME/.claude/memory}`).

## When to Use

- End of a deep session where you learned things about a project's purpose or direction
- After domain modeling discussions (data schemas, taxonomies, business rules)
- After architectural decisions that have a *why* behind them
- When the user shared context about stakeholders, timelines, or vision
- Before `/compact` or session end to preserve understanding

## Arguments

```
/absorb                    — Full session distillation
/absorb @project:name      — Scope to a specific project
/absorb @dry               — Show what would be saved without writing
```

---

## Phase 1: NIGREDO — Extract Raw Knowledge

Scan the entire conversation for knowledge that is NOT derivable from code, git history, or existing docs.

### 1.1 Classify Findings

For each insight, classify into exactly one type:

| Type | What it captures | Persists to |
|------|-----------------|-------------|
| **intent** | Why a project exists, who it serves, what problem it solves | `project_{name}.md` |
| **direction** | Where the project is heading, roadmap, phases, priorities | `project_{name}.md` |
| **domain** | Business/domain concepts, taxonomies, terminology, rules | `project_{name}.md` or dedicated topic file |
| **decision** | Architectural choices with rationale (the *why*) | `project_{name}.md` |
| **vision** | Long-term aspiration, philosophy, design principles | `project_{name}.md` |
| **person** | Stakeholder context, roles, preferences, relationships | `project_{name}.md` or `user_*.md` |
| **feedback** | How the user wants to work, corrections, validated approaches | `feedback_*.md` |
| **reference** | External resources, APIs, accounts, credentials context | Existing memory or new `reference_*.md` |
| **infrastructure** | Services, ports, configs, deployment topology | `MEMORY.md` (infrastructure section) |

### 1.2 Build the Prima Materia

For each finding, capture:

```yaml
- insight: [one-line statement]
  type: [from table above]
  evidence: [what in the session proves this — quote or reference]
  novelty: [new | extends | contradicts | duplicate]
  weight: [critical | important | useful | noise]
```

**Drop anything classified as `noise` or `duplicate`.**

---

## Phase 2: ALBEDO — Purify Against Existing Memory

Read the current memory state:

```bash
cat $MEMORY/MEMORY.md
```

Then check each relevant topic file:

```bash
# If project-scoped, read the project memory
cat $MEMORY/project_{name}.md 2>/dev/null
```

For each insight:

| Novelty | Action |
|---------|--------|
| **new** | Create new entry |
| **extends** | Merge into existing entry (append detail, don't duplicate) |
| **contradicts** | Replace old entry with new truth + rationale |
| **duplicate** | Skip entirely |

**Key rule:** Real observations from this session beat prior assumptions. If we learned something contradicts an old memory, the new learning wins.

---

## Phase 3: CITRINITAS — Structure for Persistence

### 3.1 Project Memory Files

For project-scoped insights, organize into a `project_{name}.md` file with this structure:

```markdown
---
name: Project Name
description: One-line — what this project is
type: project
---

## Intent
[Why this project exists. The problem it solves. Who it serves.]

## Direction
[Current phase. What's being built now. Near-term priorities.]

## Vision
[Long-term aspiration. What it becomes when "done".]

## Domain Knowledge
[Business concepts, taxonomies, terminology, rules that aren't in the code.]

## Key Decisions
[Architectural choices with WHY. Format: "Decision — because Reason."]

## Stakeholders
[Who's involved, their role, their preferences.]
```

Only include sections that have content. Don't create empty sections.

### 3.2 Feedback Files

For workflow preferences, create or update `feedback_{topic}.md`:

```markdown
---
name: Feedback topic
description: One-line rule
type: feedback
---

[Rule statement]

**Why:** [reason from session]
**How to apply:** [when/where this kicks in]
```

### 3.3 MEMORY.md Index

After creating/updating topic files, update the `MEMORY.md` index:
- Add one-line pointer for any new file: `- [Title](file.md) -- one-line hook`
- Keep entries under 150 chars
- Don't duplicate content — the index points, it doesn't store

### 3.4 Project CLAUDE.md

If insights are **project-specific instructions** (not observations but directives), consider updating the project's `CLAUDE.md` instead:
- Terminology rules ("never use X, always say Y")
- Build commands
- Architecture constraints
- Domain-specific coding rules

**CLAUDE.md = instructions for Claude. Memory = observations about the world.**

---

## Phase 4: RUBEDO — Write and Verify

### 4.1 Write Files

Use Write/Edit tools to persist each insight to the correct file. Create new topic files when a project doesn't have one yet.

### 4.2 Coherence Check

After writing:
- [ ] No duplicate entries across files
- [ ] No secrets or ephemeral data (task IDs, temp paths) in memory
- [ ] Each memory file has correct frontmatter (name, description, type)
- [ ] MEMORY.md index has pointers to all new files
- [ ] Insights are written as durable facts, not session narration
  - Bad: "Today we discovered that the energies table has misclassified entries"
  - Good: "The energies table contains 36 form types — not all are energies. Items with () syntax (eg `flight()`, `Clearing()`) are functions/commands, misclassified under form='energy'"

### 4.3 Potency Report

```
ABSORBED
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Project: {project name or "cross-project"}

Insights captured:
  + {insight 1} [{type}]
  + {insight 2} [{type}]
  ~ {merged with existing} [{type}]
  - {skipped — duplicate/noise}

Files updated:
  {path}  [{N} changes]
  {path}  [created]

Understanding depth: {shallow | moderate | deep}
  shallow  = facts only, no vision/intent captured
  moderate = intent + direction + some domain knowledge
  deep     = full picture: intent, vision, domain, decisions, stakeholders
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## What NOT to Absorb

- Code patterns, file paths, function signatures — read the code
- Git history, who changed what — use git log
- Debugging solutions — the fix is in the commit
- Anything already in CLAUDE.md files
- Temporary state (current task, in-progress work)
- Speculative conclusions from a single data point

**The test:** "Would a future session benefit from knowing this, AND can't discover it from the codebase?" If both yes, absorb it.

---

## Integration with Other Skills

| Skill | Relationship |
|-------|-------------|
| `/handoff` | `/absorb` captures understanding; `/handoff` captures task state. Complementary. |
| `/pickup` | Future sessions benefit from what `/absorb` persisted |
| `/compact` | Run `/absorb` before `/compact` to preserve understanding before context compression |
