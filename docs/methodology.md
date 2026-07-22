# Methodology

The harness is a set of conventions over plain files. This document is the full system; the [cases](cases/) show it running — start with [Unity → Roblox](cases/unity-to-roblox.md).

## 1. Why sessions need roles

A single "do everything" AI session drifts: it plans, then implements its own plan, then grades its own work. Splitting by role creates the review boundary that a solo learner otherwise lacks:

| Role | Does | Never does | Suggested model tier |
|---|---|---|---|
| **lead** | Maintains the roadmap and per-project plans, issues task batches, reviews finished work, rolls up lessons | Implements; touches the tool directly (read-only inspection at most) | Frontier / high-reasoning |
| **pair** | Assists execution: snippets, step-by-step guides, verification, debugging, repetitive batch work | First implementations during a learning phase (§5.1); plan changes (may only propose) | Mid-tier (see §5.4) |
| **ops** | Jobs the coding assistant can't do: browser/desktop automation, office-document deliverables, scheduled tasks, large research reports | Planning or issuing tasks (lead's exclusive right) | Either |

The pair is named after pair programming deliberately: it works alongside you and never takes the keyboard away — which is the entire philosophy of §5.1 in one word.

Roles are enforced socially, not technically — by a permissions table in the rules file that every session auto-loads. In practice this works: a session that knows it is `pair` will decline to edit the plan and will leave a proposal note instead. The human is the escalation path for violations.

## 2. The files

| File | Owner (write) | Purpose |
|---|---|---|
| `CLAUDE.md` (repo root) | lead | The harness entry point. Auto-loaded by every session: roles, permissions, cycle, guardrails |
| `roadmap.md` | lead | Cross-project strategy, pipeline, research backlog, rolled-up lessons |
| `.memory/lead_memory.md` | lead | Decisions log + **project-state snapshot** (the handoff mechanism, §4) |
| `.memory/pair_memory.md` | pair | Tool/engine knowledge that transfers across projects |
| `.memory/ops_memory.md` | ops | Ops-specific know-how and environment notes |
| `projects/<P>/.ideas/project.md` | lead | Spec: concept + a **feature ↔ learning-goal mapping** (§5.1) |
| `projects/<P>/.ideas/plan.md` | lead | Milestones, completion criteria, per-milestone retrospective rollups |
| `projects/<P>/.ideas/todo.md` | lead, pair, ops | The task table. Single source of truth for work state |
| `projects/<P>/.ideas/pair.md` | pair | Dated work log: lessons, decisions, traps (§6) |

## 3. The cycle

```
[lead]  update plan.md → issue task batch in todo.md
    ↓
[pair + human]  pick ONE task (DOING) → execute → DONE + result note
                → record lessons in pair.md
    ↓
[lead]  review DONE → REVIEWED, roll lessons up into plan.md → issue next batch
```

Statuses: `TODO → DOING → DONE → REVIEWED`, plus `BLOCKED` (reason required; lead resolves first). One DOING per pair at a time. Every task batch ends with a git commit.

The rollup step is what makes this a learning system rather than a task tracker: the lead's milestone retrospective distills the pair log into principles, carried forward into the next batch and the next project.

## 4. Memory protocol

- **Start:** every session reads its role's files (listed in the rules file) before doing anything.
- **End:** every session writes back what changed. The lead additionally overwrites a **project-state snapshot** section in its memory file — current milestone, pending confirmations, what to check first next time.
- **Handoff = snapshot, not transcript.** A new session resumes from the snapshot. Anything not written down is deliberately lost; this forces honest, complete records.
- **Promotion rule:** project-specific lessons live in the project's `pair.md`; anything reusable across projects gets promoted to `.memory/pair_memory.md`. The promotion decision itself is a useful review act.

## 5. Guardrails

### 5.1 Learning-goal protection (the core rule)

During a declared learning phase, **the first implementation of every feature belongs to the human.** The spec encodes this: each feature maps to a learning goal ("checkpoint system → server scripts, touch events"), so skipping the human implementation visibly breaks the spec's purpose, not just a preference.

The pair's share: reference snippets, step-by-step editor guidance, post-hoc verification, debugging help, and repetitive application of a pattern the human already built once (e.g. human builds stage 1's checkpoint; pair replicates it for stages 2–6).

**Sunset clause:** the rule names its own end. When the learning phase closes, the lead relaxes it in writing and productivity becomes the priority. A guardrail without a sunset becomes ritual.

### 5.2 Tool-automation guardrails (editor MCP or similar)

If the AI can drive the tool directly (execute code in the editor, insert assets, capture screens), split capabilities by role:

- **pair:** full toolset, under rules: destructive operations (delete, bulk modify) require human approval first; after any edit, self-verify with a read tool; report a change summary so the human saves and commits.
- **lead:** strictly read-only tools (inspect, search, console output, screenshots) — and **not** the code-execution tool even "for queries," because arbitrary execution cannot be guaranteed side-effect-free. Draw the line at tool level, not intent level: tool-level boundaries are auditable.
- **Binary artifacts have no diff.** If project files are opaque (e.g. engine scene files), the commit is the only rollback point — commit cadence is a safety mechanism, not hygiene.
- **Don't trust "saved."** Editor sessions expire, saves fail silently. Before committing, re-inspect actual state with a read tool rather than trusting anyone's report — including the human's.

### 5.3 Truth hierarchy

`actual behavior on the machine > official docs > web search > AI prior knowledge.`

Record which level a fact came from. Two corollaries from the field: keyboard shortcuts and UI layouts drift faster than tutorials (verify on device); platform policy (age verification, licensing, monetization terms) is perishable — re-search every time, never cache.

### 5.4 Model choice is a guardrail

Give the lead a frontier model and the pair a mid-tier one — and not (only) for cost.

- **Economics:** lead sessions are short and judgment-dense (review, rollup, planning); pair sessions are long and token-heavy, with scope already narrowed by the todo table. Put the expensive model where judgment concentrates and the efficient one where volume concentrates.
- **Behavior:** stronger models have a stronger tendency to helpfully overshoot scope and "just do it all" — which is precisely the anti-pattern §5.1 exists to prevent. During a learning phase, capability beyond the role's needs is a liability, not a bonus. A mid-tier pair following explicit guardrails protects your learning better than a frontier pair resisting its own initiative.
- **Escalation, not upgrade:** when the pair stalls on a root-cause hunt, don't reach for a bigger model — push the problem to lead review. That boundary is what the cycle is for. (In the founding case, every deep-debugging episode was resolved by a mid-tier pair under these rules.)

## 6. Logging conventions

- Format: `date / task-id / lesson·decision·trap`, one line each, in the project's `pair.md`.
- **Decision ≠ implementation.** Recording "decided to add X" and "implemented X" identically caused our one real process failure (a feature everyone believed existed). State which one it is.
- Log failures with their root cause, not just the fix. The founding case's most valuable entries are the three bugs that shared one cause.

## 7. Beyond game engines

Nothing above is engine-specific. The harness fits any domain where (a) you are deliberately learning, (b) work spans many sessions, and (c) an AI could do the work for you — which is exactly when it shouldn't. Swap "engine" for a 3D modeling suite, a DAW, an infrastructure stack; the roles, files, cycle, and guardrails carry over unchanged. When your project ends, write it up with the [case template](cases/_template.md).
