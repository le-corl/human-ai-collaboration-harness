# {PROJECT_NAME} — Common project rules

updated: {YYYY-MM-DD}

This file is the single source of truth for provider-neutral project rules. Every automatically loaded entrypoint must read it first.

## 0. Harness state

- Status: `UNINITIALIZED`
- Initialized on: `{UNSET | YYYY-MM-DD}`

`UNINITIALIZED` means the entrypoint must invoke `init` before any other role. Change the status to `ACTIVE` only after the user confirms the required project contract. An active contract has resolved values for the current objective, collaboration profile, project-document language, code language, commit policy, and push policy. Optional unknowns use an explicit value such as `None declared`; they do not keep the project uninitialized.

## 1. Project contract

- User background: {BACKGROUND}
- Current objective: {CURRENT_OBJECTIVE}
- Explicit non-goals: {NON_GOALS}
- Collaboration profile: `{UNSET | learning | practical | mixed}`
- Profile selection date: {YYYY-MM-DD_OR_UNSET}
- Profile overrides: {NONE_OR_PROJECT_SPECIFIC_OVERRIDES}
- Deadline and constraints: {DEADLINE_AND_CONSTRAINTS}
- Project document language: {DOC_LANGUAGE}
- Code identifiers and comments: {CODE_LANGUAGE}

The user approves product scope and priorities. The AI may implement and document work within the approved scope.

If the collaboration profile is `UNSET`, do not infer it from the project. Route to `init`. In `mixed`, record each task's `task mode` as `learning` or `practical`. Keep the recorded selection until the user explicitly changes it.

The harness instructions remain in English. Downstream project documents and user-facing output may use the project document language selected above.

## 2. Source-of-truth documents

| Document | Meaning |
|---|---|
| `collaboration/brief.md` | Current product conditions and scope managed by the collaboration shell |
| `collaboration/plan.md` | Milestones and acceptance criteria managed by the collaboration shell |
| `collaboration/tasks.md` | Current task state managed by the collaboration shell |
| `collaboration/decisions.md` | Dated discussion outcomes, changes, and reversals |
| `collaboration/worklog.md` | Implementation, command, verification, and failure evidence |
| `collaboration/memory/` | Concise role continuity and reusable collaboration knowledge |
| `project/` | Actual development-project root: source, assets, tests, configuration, and project-owned documents |

Keep decisions separate from implementation. Record whether an item is a `decision`, `implementation`, `verification`, or `user approval`.

Location follows lifecycle, not subject matter or authorship. A file belongs inside `project/` only when it is still needed after the development project is detached from this harness. Project-specific but AI-managed discussion, planning, task, and evidence records remain in `collaboration/`. Distill accepted product truth into project-owned code, tests, configuration, ADRs, or documentation when the project must retain it independently.

## 3. Roles

`init` is a transient bootstrap skill, not an ongoing project role. It hands the original request to one of the roles below after activation.

| Role | Authority | Must not |
|---|---|---|
| `lead` | Maintain brief, plan, decisions, issue tracked tasks, and review `DONE` work | Change scope without approval or directly implement product features |
| `learn` | Explain, support the first implementation, debug, and verify `learning` work | Replace the user's protected first implementation or define new scope |
| `work` | Implement, verify, and report completion of `practical` work | Expand scope without approval or infer user approval |
| `ops` | Prepare external consoles, browser workflows, documents, and releases; record owned results | Issue product plans or perform irreversible submission for the user |

When one agent must take multiple roles, state the transition first and preserve each role's file authority. When practical, separate review from implementation across sessions or subagents.

## 4. Working method

- Continue the current user request or active task. Resolve an existing `BLOCKED` item first only when it blocks the current work.
- Follow the selected profile for tracking depth, parallelism, and explanation level.
- Ask the user only when unclear scope or acceptance criteria would materially change the result.
- After implementation, run tests or checks proportional to risk and change scope. Preserve enough evidence to reproduce the conclusion.
- If bounded investigation cannot establish the cause, record the evidence and mark the work `BLOCKED` instead of applying a guessed fix.
- Preserve user changes and unrelated dirty-worktree content.

## 5. Profile routing

- `learning` uses `collaboration/profiles/learning.md` and `learn`.
- `practical` uses `collaboration/profiles/practical.md` and `work`.
- `mixed` uses only the profile document and role assigned to the current task.
- Profile documents define only differing defaults; they do not repeat these common rules.

## 6. Facts and verification

- Evidence priority: actual target environment > official documentation > primary sources > search results > AI memory.
- Verify policies, prices, versions, console UI, and platform requirements against current official sources when they are used.
- Do not treat local browser, emulator, and real-device verification as interchangeable.
- Render visual work at the target size and compare it with the reference.
- Use deterministic tests or simulations for randomness, time, and boundary behavior.
- Before release, compare current source with artifact creation time, version, internal manifest, size, and hash.

## 7. External state and Git

- Confirm user authority and intent before irreversible external actions involving accounts, publication, submission, review requests, payment, or messages.
- Commit policy: {COMMIT_POLICY}
- Push policy: {PUSH_POLICY}
- Before destructive file operations, verify the exact target and recovery path.

## 8. Document lifecycle

- Keep only currently true conditions in `brief.md`. Move superseded choices to `decisions.md`.
- Keep only the current milestone and recent results in `tasks.md`. Move closed tables to `collaboration/archive/`.
- Keep evidence in `worklog.md` at the depth required by the selected profile; archive it by milestone when useful.
- Role memories contain only the next-session starting point and reusable principles. Do not duplicate the full worklog.
- After project completion, write a case using the source harness repository's `cases/_template.md` and promote only general lessons into the harness.
