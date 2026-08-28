---
name: init
description: Initialize or repair the harness project contract when PROJECT_RULES.md is missing, its harness status is not ACTIVE, or a required contract value is unresolved. Do not use for an already active and coherent contract.
---

# Init

1. Preserve the user's original request. Treat `project/` as the sole development-project root, and inspect it plus existing collaboration records for facts that can be verified without asking.
2. If `PROJECT_RULES.md` is missing, do not invent the shared policy. Ask the user to restore that file from the harness template, then resume initialization. Do not overwrite meaningful existing rules or project records.
3. Build a proposed contract from explicit user statements and verified repository facts. Ask once, in one compact message, only for unresolved choices or confirmation that materially affects the work:
   - the current objective;
   - `learning`, `practical`, or `mixed` collaboration profile;
   - project-document and code-language conventions;
   - any project-specific authority, commit, or push policy that differs from the safe default of requiring an explicit user request.
4. Never infer the collaboration profile. Explain the three choices briefly when the user has not already made an explicit selection.
5. Record the confirmed contract in `PROJECT_RULES.md`. Resolve the current objective, collaboration profile, project-document language, code language, commit policy, and push policy; replace optional unknowns with an explicit value such as `None declared` rather than inventing them. Set the profile selection date, then change `Harness state > Status` to `ACTIVE` with the initialization date.
6. Update `collaboration/brief.md`, `plan.md`, `tasks.md`, and `decisions.md` only where their template placeholders must be initialized for the confirmed contract. Preserve meaningful existing content. Do not place harness records inside `project/`.
7. Read only the selected profile in `collaboration/profiles/`, choose the role needed for the user's original request, and continue that request instead of ending after setup.
8. Do not expand product scope or perform commits, pushes, publication, account changes, or other external actions unless the user has authorized them.
