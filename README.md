# Human-AI Collaboration Harness

A provider-neutral, plain-text harness for making human-AI collaboration explicit and reusable. It synchronizes the human's and AI's observable thinking, expression, judgment, and execution in both directions while keeping final authority with the human.

The harness does not attempt to expose hidden chain-of-thought. It works with visible assumptions, options, constraints, decisions, evidence, and actions.

## Quick start

1. Copy the entire [`template/`](template/) directory and rename that copy for the new workspace. The whole directory is the only supported copy unit.
2. Place or clone the actual development project inside the copied `project/` directory.
3. Open the copied workspace root with Codex or Claude. `AGENTS.md` or `CLAUDE.md` becomes the provider-specific entrypoint.
4. If the project contract is not active, the `init` skill asks for the missing decisions, records them in `PROJECT_RULES.md`, and resumes the original request.
5. After the project, write a case from [`cases/_template.md`](cases/_template.md) and promote only reusable lessons back into the harness.

Do not copy `project/` or a skill directory by itself. The entrypoints, contract, profiles, collaboration state, provider adapters, and development root are designed to work as one unit.

## Workspace model

```text
copied-template/
├── AGENTS.md                 # Codex entrypoint
├── CLAUDE.md                 # Claude entrypoint
├── PROJECT_RULES.md          # Confirmed project contract
├── .agents/skills/           # Codex role adapters
├── .claude/skills/           # Claude role adapters
├── collaboration/            # AI-collaboration state and history
│   ├── profiles/
│   ├── memory/
│   ├── brief.md
│   ├── plan.md
│   ├── tasks.md
│   ├── decisions.md
│   └── worklog.md
└── project/                  # Sole development-project root
```

File placement follows lifecycle, not topic or authorship:

> Would the development project still need this file if detached from the harness?

- If yes, place it in `project/`.
- If no, keep it in `collaboration/`.

Planning, discussion history, task state, and working evidence usually belong to `collaboration/`. Accepted product truth belongs in `project/` when the standalone project must retain it as code, tests, configuration, ADRs, or documentation.

## Collaboration profiles

The user selects a profile during initialization. The harness does not infer it silently.

- `learning`: protects the human's first implementation for the selected learning goal. The AI explains, reviews, debugs, verifies, and helps repeat the pattern.
- `practical`: lets the AI implement the approved scope directly, with verification and documentation proportional to risk.
- `mixed`: assigns `learning` or `practical` per task.

Profiles change defaults, not human authority. Scope expansion, costs, publication, submission, release, and other consequential external actions remain user decisions.

## Roles

- `init`: establishes or repairs the project contract, then hands the original request back to the appropriate role.
- `lead`: owns scope, plans, tracked work, decisions, and completion review.
- `learn`: supports protected learning work without replacing the human's first implementation.
- `work`: implements and verifies approved practical work.
- `ops`: prepares browser, console, release, document, and other operational work within confirmed authority.

Codex and Claude use separate adapter directories, but share the same contract, profiles, project state, and methodology.

## Repository layout

```text
human-ai-collaboration-harness/
├── README.md
├── METHODOLOGY.md
├── cases/
│   └── _template.md
├── LICENSE
└── template/
```

- [`METHODOLOGY.md`](METHODOLOGY.md) defines the provider-neutral rules and reasoning behind the harness.
- [`cases/`](cases/) is where users can accumulate sanitized project lessons. The repository starts with a template rather than author-specific histories.
- [`template/`](template/) is the complete deployable workspace shell.

Harness-authored documents are maintained in English to reduce interpretation loss between AI providers. Documents created inside a deployed project may use the language selected in `PROJECT_RULES.md`.

## Evolving the harness

The intended feedback loop is:

```text
copy template → run a project → write a sanitized case → extract reusable lessons → update the harness
```

Cases are evidence for improving defaults, boundaries, and role behavior. They are not proof that every workflow will fit every user or project. Keep project-specific or private details outside the public repository, and promote only lessons that generalize.

## License

Released under the [MIT License](LICENSE). You may use, modify, distribute, sublicense, and sell copies, provided that the copyright and permission notice are preserved. The software is provided without warranty.
