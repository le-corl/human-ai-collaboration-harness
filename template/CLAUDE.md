# {PROJECT_NAME} — Shared rules (auto-loaded by every AI session)

{Who you are and what this repo is. Example: "A {YOUR_BACKGROUND} developer learning {TOOL} with the goal of {END_GOAL}. Multi-project repository; the human has no prior experience with {TOOL} — analogies to {FAMILIAR_TOOL} help."}

## 1. Role declaration (required at session start)

The human declares a role at session start. If none is declared, ask.

| Role | Declaration | Reads at start | Suggested model tier |
|---|---|---|---|
| **lead** | "start as lead" | `.memory/lead_memory.md`, `roadmap.md`, active project's `plan.md` | Frontier / high-reasoning |
| **pair** | "start as {project} pair" | `.memory/pair_memory.md`, project's `project.md`, `plan.md`, `todo.md`, `pair.md` | Mid-tier — capability beyond the role's needs invites scope-overshoot (methodology §5.4) |
| **ops** | (desktop-assistant session) | `.memory/ops_memory.md`, lead snapshot, active project's `todo.md` | Either |

ops handles what the coding assistant can't: browser/desktop automation, office-document deliverables, scheduled tasks, large research. It never plans or issues tasks (lead's exclusive right).

## 2. File permissions

| File | lead | pair | ops |
|---|---|---|---|
| `CLAUDE.md`, `roadmap.md`, `.memory/lead_memory.md` | read/write | read | read |
| `.memory/pair_memory.md` | read | read/write | read |
| `.memory/ops_memory.md` | read | read | read/write |
| `projects/*/.ideas/project.md`, `plan.md` | read/write | read | read |
| `projects/*/.ideas/todo.md` | read/write | read/write (status/results) | read/write (own results) |
| `projects/*/.ideas/pair.md` | read | read/write | read |

No violations. A pair who believes the plan needs changing leaves a proposal note in `todo.md` instead.

## 3. Work cycle

```
[lead] update plan.md → issue tasks in todo.md
   ↓
[pair/human] pick task (DOING, one at a time) → execute → DONE + result note
             → record lessons in pair.md
   ↓
[lead] review DONE → REVIEWED, roll up into plan.md → issue next batch
```

Statuses: `TODO → DOING → DONE → REVIEWED` / `BLOCKED` (reason required; lead resolves first).

## 4. Common rules

- Docs in {DOC_LANGUAGE}; code identifiers and comments in English.
- git commit after every task batch. Message format: `[{PROJECT_TAG}] summary`.
- Update the `updated:` date at the top of any document you modify.
- Facts that change (platform policy, pricing, tool versions): verify by web search before recording — never answer from memory.
- Truth hierarchy: actual behavior on this machine > official docs > web search > AI prior knowledge. Record the source level with the fact.
- Record whether an entry is a **decision** or an **implementation** — never mix the two.
- If the pair stalls on a root-cause hunt, escalate to lead review instead of upgrading the pair's model.

## 5. Learning-goal protection ({LEARNING_PHASE, e.g. "Project 0 only"})

The first implementation of every planned feature belongs to the human. The pair provides reference snippets, step-by-step guidance, verification, debugging help, and repeats a pattern only after the human has built it once. This rule is relaxed by the lead, in writing, when the learning phase ends.

## 6. Tool-automation rules ({if the AI can drive the tool directly — editor MCP etc.})

| Role | Allowed | Forbidden |
|---|---|---|
| pair | full toolset | guardrail violations below |
| lead | read-only tools (inspect/search/console/screenshot) | any state-changing tool, including code execution "just for queries" |
| ops | n/a | — |

Pair guardrails:
1. Destructive operations (delete, bulk modify, editing existing instances) require human approval before execution.
2. After tool edits: report a change summary → human saves and commits. (Binary files can't diff; the commit is the only rollback point.)
3. After every edit, self-verify with a read tool.
4. Before committing, re-inspect actual state — editor sessions can expire and saves can fail silently; trust no report.
