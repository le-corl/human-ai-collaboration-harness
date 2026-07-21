# {PROJECT_NAME} — Shared rules (auto-loaded by every AI session)

{Who you are and what this repo is. Example: "A {YOUR_BACKGROUND} developer learning {TOOL} with the goal of {END_GOAL}. Multi-project repository; the human has no prior experience with {TOOL} — analogies to {FAMILIAR_TOOL} help."}

## 1. Role declaration (required at session start)

The human declares a role at session start. If none is declared, ask.

| Role | Declaration | Reads at start |
|---|---|---|
| **leader** | "start as leader" | `.memory/leader_memory.md`, `roadmap.md`, active project's `plan.md` |
| **worker** | "start as {project} worker" | `.memory/worker_memory.md`, project's `project.md`, `plan.md`, `todo.md`, `worker.md` |
| **ops** | (desktop-assistant session) | `.memory/ops_memory.md`, leader snapshot, active project's `todo.md` |

ops handles what the coding assistant can't: browser/desktop automation, office-document deliverables, scheduled tasks, large research. It never plans or issues tasks (leader's exclusive right).

## 2. File permissions

| File | leader | worker | ops |
|---|---|---|---|
| `CLAUDE.md`, `roadmap.md`, `.memory/leader_memory.md` | read/write | read | read |
| `.memory/worker_memory.md` | read | read/write | read |
| `.memory/ops_memory.md` | read | read | read/write |
| `projects/*/. ideas/project.md`, `plan.md` | read/write | read | read |
| `projects/*/.ideas/todo.md` | read/write | read/write (status/results) | read/write (own results) |
| `projects/*/.ideas/worker.md` | read | read/write | read |

No violations. A worker who believes the plan needs changing leaves a proposal note in `todo.md` instead.

## 3. Work cycle

```
[leader] update plan.md → issue tasks in todo.md
   ↓
[worker/human] pick task (DOING, one at a time) → execute → DONE + result note
               → record lessons in worker.md
   ↓
[leader] review DONE → REVIEWED, roll up into plan.md → issue next batch
```

Statuses: `TODO → DOING → DONE → REVIEWED` / `BLOCKED` (reason required; leader resolves first).

## 4. Common rules

- Docs in {DOC_LANGUAGE}; code identifiers and comments in English.
- git commit after every task batch. Message format: `[{PROJECT_TAG}] summary`.
- Update the `updated:` date at the top of any document you modify.
- Facts that change (platform policy, pricing, tool versions): verify by web search before recording — never answer from memory.
- Truth hierarchy: actual behavior on this machine > official docs > web search > AI prior knowledge. Record the source level with the fact.
- Record whether an entry is a **decision** or an **implementation** — never mix the two.

## 5. Learning-goal protection ({LEARNING_PHASE, e.g. "Project 0 only"})

The first implementation of every planned feature belongs to the human. The AI provides reference snippets, step-by-step guidance, verification, debugging help, and repeats a pattern only after the human has built it once. This rule is relaxed by the leader, in writing, when the learning phase ends.

## 6. Tool-automation rules ({if the AI can drive the tool directly — editor MCP etc.})

| Role | Allowed | Forbidden |
|---|---|---|
| worker | full toolset | guardrail violations below |
| leader | read-only tools (inspect/search/console/screenshot) | any state-changing tool, including code execution "just for queries" |
| ops | n/a | — |

Worker guardrails:
1. Destructive operations (delete, bulk modify, editing existing instances) require human approval before execution.
2. After tool edits: report a change summary → human saves and commits. (Binary files can't diff; the commit is the only rollback point.)
3. After every edit, self-verify with a read tool.
4. Before committing, re-inspect actual state — editor sessions can expire and saves can fail silently; trust no report.
