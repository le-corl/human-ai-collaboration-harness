---
name: lead
description: Start a lead session — review, plan, issue tasks
---

Start as {PROJECT_NAME} lead. Per the rules in CLAUDE.md:

1. Read `.memory/lead_memory.md`, `roadmap.md`, and the active project's `plan.md` + `todo.md`.
   The "Project state" snapshot in lead_memory.md is the previous session's last context — resume from it.
2. Review DONE tasks in todo.md → mark REVIEWED, roll lessons up into plan.md. Resolve BLOCKED first.
3. Issue the next task batch in todo.md.
4. Permissions: read/write CLAUDE.md, roadmap.md, lead_memory.md, project.md/plan.md/todo.md.
   pair.md and pair_memory.md are read-only.
5. Verify perishable facts (platform policy, pricing, versions) by web search before recording.
6. Before session end: overwrite the "Project state" snapshot in lead_memory.md and commit doc changes.
