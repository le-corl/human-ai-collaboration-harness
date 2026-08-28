# Human-AI Collaboration Harness Methodology

updated: 2026-08-29

This harness is a plain-text protocol for managing **collaboration profiles, roles, file ownership, evidence, and handoff**, independent of any AI product. Its goal is to preserve decisions and state across sessions while synchronizing the user's and AI's observable thinking, expression, judgment, and execution in both directions. AI thinking here means visible assumptions, problem decomposition, decision rationale, constraints, and options—not hidden chain-of-thought.

## 1. Provider-neutral core and adapters

`PROJECT_RULES.md` is the single source of truth for the current project contract and common rules. Automatically loaded files such as `AGENTS.md` and `CLAUDE.md` are thin adapters that check initialization state, then load only the required profile document and provider-specific role skill.

Changing providers must not require converting the product plan or work records. Only the entrypoint and skill location change.

### Initialization routing

`PROJECT_RULES.md` begins as `UNINITIALIZED`. Before any normal role is selected, the entrypoint invokes `init` when the file is missing, the status is not `ACTIVE`, or a required contract value remains blank or contains a template placeholder. Required values are the current objective, collaboration profile, project-document language, code language, commit policy, and push policy.

`init` inspects existing project evidence first, asks once for decisions it cannot safely derive, writes the confirmed contract, and changes the status to `ACTIVE`. It then resumes the user's original request through the appropriate profile and role. An active, coherent contract is not reinitialized merely because an optional field is blank.

## 2. Roles

| Role | Owns | Does not do | Recommended model traits |
|---|---|---|---|
| `lead` | Objectives, scope, milestones, decision documents, tracked task issuance, completion review | Change scope without approval or directly implement product features | Strong judgment and review |
| `learn` | Explanation, protected first-implementation support, debugging, verification, and pattern repetition for learning work | Replace the protected first implementation or define product scope | Strong explanation and staged debugging |
| `work` | Implementation, verification, and completion reporting for practical work | Expand scope without approval or infer user approval | Strong autonomous execution and tool use |
| `ops` | Browser, console, document, release preparation, and other operational work | Issue product plans or perform irreversible external submission for the user | Appropriate tool access |

Prefer assigning roles to separate sessions or agents when the project can support that overhead. Select model capability per role: judgment and review for `lead`, explanation and staged debugging for `learn`, autonomous execution for `work`, and suitable tool access for `ops`. Model performance is part of role assignment, but role boundaries still describe **who decides what and who proves what**.

One agent may take multiple roles when shared context or project size makes separation inefficient. State each transition and preserve file ownership and review boundaries. Every substantive assistant response has one accountable `response owner role`; a later feedback signal applies to that role unless the user explicitly names another. Feedback processing is a meta-operation and does not become the response owner.

## 3. Role-scoped collaboration confidence

`Collaboration confidence` is a percentage-like control value for how independently one role should act with the current user in the current project. It is not a probability that the role is factually correct, a model capability score, or a value that transfers to another role or project.

Each role memory starts once at `50%` with zero feedback samples. Later sessions inherit that role's stored value. Record the current executor when known. If the project objective or assigned provider/model changes materially, move the value halfway toward `50%`, round to a whole number, and record the reason. Do not transfer a value into a newly copied harness.

Confidence changes interaction cost and reversible autonomy:

| Band | Default behavior |
|---|---|
| `20–39` cautious | Surface more assumptions, recheck relevant local state, use smaller reversible steps, and add checkpoints where a mismatch would change the result |
| `40–60` balanced | Follow the selected profile and normal risk-proportional workflow |
| `61–80` trusted | Reduce repeated confirmations and explanations, reuse recently verified stable context, act directly within approved reversible scope, and make one additional evidence-based recommendation when disagreement remains |

No confidence value weakens role boundaries, learning guardrails, evidence requirements, or authority for irreversible and external actions. Low confidence increases verification and dialogue; it does not require agreement with an unsupported user claim. High confidence may skip redundant interaction, not evidence required by risk.

The user changes confidence only through an explicit `feedback-up` or `feedback-down` invocation. Ordinary praise or criticism does not change it.

- `feedback-up`: add `5`, capped at `80`.
- `feedback-down`: subtract `10`, floored at `20`.
- Increment the role's feedback sample count once per non-duplicate event.

Apply feedback to the immediately preceding substantive assistant response and its observed result. If that response's owner role is ambiguous, ask the user to identify the role before changing any file. The user may name the target role directly in a mixed-role session.

Keep the current score, sample count, executor, and distilled interaction adjustments in the role memory, which normal role sessions already read. Append the event evidence only to that role's file under `collaboration/feedback/`; normal role work must not load feedback logs. An event records the target, observed result or user judgment, likely cause, promoted adjustment, and resulting score without copying the full response. Do not reinterpret user approval as proof of technical correctness or rewrite verified evidence merely to match the feedback direction.

Promote a feedback lesson into `PROJECT_RULES.md` only when the user declares it project-wide or repeated evidence shows that it applies across roles. Project completion may distill general lessons into a case and the source harness.

## 4. Collaboration profiles

At project start, the AI must not infer the profile. It asks the user to choose one of the following and records the answer in `PROJECT_RULES.md`. It does not ask again unless the user changes the selection.

- `learning`: The human owns the first implementation mapped to the learning goal. `learn` provides explanation, examples, verification, debugging, and repeated application.
- `practical`: `work` directly implements the approved scope while synchronizing the user's and AI's working methods in both directions.
- `mixed`: Each task declares `learning` or `practical` and loads only the corresponding profile document and role.

Do not duplicate common rules in each profile. `collaboration/profiles/learning.md` and `collaboration/profiles/practical.md` contain only differing defaults. If deadlines or circumstances change, `lead` records the proposed profile change and the user approves it. Every learning guardrail needs a release condition; otherwise it becomes permanent ceremony.

## 5. Files and ownership

The distributed unit is the entire `template/` directory. After copying it, `project/` is the sole development-project root and `collaboration/` is the surrounding AI-collaboration state.

Location follows lifecycle, not topic or author. Ask: **Would the development project still need this file if detached from the harness?** If yes, it belongs in `project/`. If not, it belongs in `collaboration/`. Discussion history stays outside; accepted product truth is distilled into project-owned code, tests, configuration, ADRs, or documentation when needed.

| File | Primary owner | Purpose |
|---|---|---|
| `PROJECT_RULES.md` | lead | Common rules, authority, selected collaboration profile, and overrides |
| `collaboration/profiles/learning.md` | harness | Defaults added only for learning work |
| `collaboration/profiles/practical.md` | harness | Defaults added only for practical work |
| `collaboration/brief.md` | lead | Current product contract and scope |
| `collaboration/plan.md` | lead | Milestones and acceptance criteria |
| `collaboration/tasks.md` | lead + execution role | Single source of truth for tracked task state |
| `collaboration/decisions.md` | lead | Dated discussion outcomes, changes, and reversals |
| `collaboration/worklog.md` | learn + work + ops | Implementation, commands, verification, failures, and causes |
| `collaboration/memory/lead_memory.md` | lead | Concise starting snapshot for the next lead session |
| `collaboration/memory/learn_memory.md` | learn | Reusable technical knowledge and environment traps from learning work |
| `collaboration/memory/work_memory.md` | work | Practical continuity, technical knowledge, and collaboration fit |
| `collaboration/memory/ops_memory.md` | ops | Reusable operational environment and external-procedure knowledge |
| `collaboration/feedback/{role}.md` | feedback skills | Role-local evidence for confidence changes; not part of normal role startup context |
| `project/` | product | Actual source, assets, tests, configuration, and project-owned documents |

Keep only currently true conditions in `brief.md`; move the history of past choices to `decisions.md`. Archive task tables and logs at milestone close. Memory files must not duplicate the full history.

## 6. Work cycles

```text
[learning]
[lead] issue a task with a learning goal, first attempt, and release condition
   ↓
[human + learn] first implementation → debugging and verification → pattern repetition → DONE

[practical]
[user or lead] request scope and outcome
   ↓
[work] implementation → risk-proportional verification → concise completion report
```

Tracked work uses `TODO → DOING → DONE → REVIEWED`; `BLOCKED` records the cause and required input. Learning work focuses on one task by default. Practical work may treat an explicit user request as the task, run non-conflicting subwork in parallel, and scale documentation to risk and handoff needs.

Do not record `decision`, `implementation`, `verification`, and `user approval` as the same state. "We decided to add it" is not evidence that it works.

## 7. Human authority

- The human makes final decisions about product concept, scope expansion, costs, accounts, publication, submission, and release.
- The AI may make reversible file changes and run verification within the approved scope.
- Before irreversible external work, messages affecting other people, or legal claims about qualifications, business status, or ratings, verify current evidence and authority.
- Set commit and push policy at project start. If unset, prepare the changes but confirm before performing those actions.
- Identify and preserve user changes. Do not clean unrelated dirty-worktree content.

## 8. Evidence priority

`Actual target-environment behavior > official documentation > reliable primary sources > search results > AI memory`.

- Recheck policies, prices, versions, and console UI whenever they are used.
- Local browser testing is not substitute evidence for native bridges or real devices.
- Verify visual work by rendering at the target size, not only by inspecting computed styles or the DOM.
- When deriving from an approved reference, preserve its relationships and values instead of recreating them by eye.
- Test randomness, time, and boundary behavior with simulation or deterministic checks in addition to code reading.

## 9. Artifact verification

Treat release artifacts as verification targets distinct from source code.

1. Confirm that the artifact was created after the latest source change.
2. Compare manifests, lockfiles, and versions embedded in the bundle.
3. Record test and production-build commands.
4. Record file size and hash.
5. Recheck the core flow in the actual upload or test environment.

A correct-looking settings screen does not prove that the binary signature, manifest, or bundled files are correct. Trust the artifact, not the setting.

## 10. External platforms and deadlines

`Development complete`, `test uploaded`, `review requested`, `released`, and `event submitted` are different states. Do not assume that information required by one state blocks another. For deadline work, break the official procedure into a staged checklist and separate the minimum submission path from full release preparation.

Identify console-dependent work early. Mark account actions the AI cannot perform as user-owned, and prepare the required values, evidence, and meaning of the next action.

## 11. Environment and portability

- Project-specific runtimes and caches may live in ignored repository paths.
- Keep moving a project runtime separate from moving an agent home or credential store.
- Reproduce runtime and cache paths through one activation script and verify them with actual commands.
- Use local fallbacks only in development and confirm that release builds exclude them.

## 12. Promoting experience

Keep project-specific collaboration facts in `collaboration/worklog.md`. Promote only principles that apply beyond the project into `collaboration/memory/` or this methodology. After project completion, write a case using [`cases/_template.md`](cases/_template.md).

A useful case contains more than a success list:

- Observed symptoms and exact errors
- Actual causes and failed approaches
- Separate human and AI contributions
- Rules to change for the next project
- Verifiable results such as test counts, builds, and submission states
