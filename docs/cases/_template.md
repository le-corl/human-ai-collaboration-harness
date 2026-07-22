# Case study: {FAMILIAR_TOOL} → {TARGET_TOOL} in {TIMEFRAME}

> **How to fill this in:** hand this file to the AI session(s) that actually worked the project
> and ask them to fill each section from the work logs (`todo.md`, the pair log, `plan.md` rollups).
> Name the engine, the APIs, and the error messages verbatim — **specifics are the evidence**;
> a case that could describe any tool describes none. Strip only what's private: accounts,
> revenue plans, unreleased product details.
>
> File naming: `docs/cases/<familiar>-to-<target>.md`. Contributions via PR are welcome —
> delete this note block before submitting.

## Context

| | |
|---|---|
| Learner | {background; prior experience with the target tool} |
| Goal | {what "done" meant — and what was explicitly a non-goal} |
| Scope | {the project, feature count, one line} |
| Time | {calendar time, intensity} |
| Setup | {tooling, source control, which AI hosts served lead/pair/ops} |
| Models | lead: {model, tier}; pair: {model, tier} |

{1–2 paragraphs: how the spec mapped features to learning goals, and anything unusual
about the setup — e.g. editor automation (MCP) availability, mid-project rule changes.}

## Episodes

{3–6 episodes. The best ones are failures with root causes. Per episode:}

### {N}. {Short, concrete title}
{What happened — symptoms first, verbatim errors if any.}
{Root cause — the actual mechanism, not the patch.}
{Who did what — what the AI contributed vs. what the human decided/built.}
**Lesson:** {the transferable principle, one or two sentences.}

## Outcomes

- {Verifiable results: features shipped, published/released state, tests passed.}
- {Knowledge captured: how many principles recorded, which ones the next project reuses.}
- {Harness performance: milestones, tasks, sessions, context losses (ideally zero).}

## What we'd change

- {Frictions found and the planned structural fix, if any.}
