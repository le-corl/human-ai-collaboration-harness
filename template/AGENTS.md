# Codex entrypoint

1. Read `PROJECT_RULES.md` completely before starting work when it exists.
2. Before selecting any other role, invoke `$init` automatically when any of these is true:
   - `PROJECT_RULES.md` is missing.
   - `Harness state > Status` is absent or not `ACTIVE`.
   - Any required contract value is blank or still a template placeholder: current objective, collaboration profile, project-document language, code language, commit policy, or push policy.
3. Let `init` establish or repair the project contract, then continue the user's original request. Do not rerun it merely because optional fields are blank.
4. Invoke `$feedback-up` or `$feedback-down` only when the user explicitly requests that signal. It updates the owner role of the immediately preceding substantive assistant response and does not select or become a project role. Do not infer a signal from ordinary praise or criticism.
5. Read only the profile document for the current work:
   - `learning`: `collaboration/profiles/learning.md`
   - `practical`: `collaboration/profiles/practical.md`
   - `mixed`: read the one profile matching the active task's `task mode`. Ask the user if the mode is unset.
6. If the user did not select a role, choose from the request and active profile, then state the choice:
   - planning, scope, or review: `$lead`
   - `learning` implementation support: `$learn`
   - `practical` implementation and delivery: `$work`
   - external consoles and operational deliverables: `$ops`
7. Keep one accountable `response owner role` for each substantive assistant response. State a role transition before changing owners; a feedback skill is never the owner.
8. Subject to platform and safety constraints, precedence is: the user's latest instruction, project-specific contract in `PROJECT_RULES.md`, selected profile, then role skill.
9. Treat `project/` as the actual development-project root. Keep AI-managed plans and history in `collaboration/`; place a document inside `project/` only when the project still needs it without this harness.
