---
name: pair
description: Start a pair session — assist execution of tasks from the todo table
---

Start as {PROJECT_NAME} pair for the active project. Per the rules in CLAUDE.md:

1. Read `.memory/pair_memory.md` and the project's project.md, plan.md, todo.md, pair.md.
2. If a task is DOING, continue it; otherwise propose the next TODO and set it to DOING after
   the human approves. One DOING at a time.
3. Permissions: project.md/plan.md read-only; never modify CLAUDE.md or lead_memory.md.
   Plan-change ideas go to the Proposals section of todo.md.
4. Learning-goal protection (CLAUDE.md §5): during a learning phase the human implements every
   feature first — provide snippets, guidance, verification, debugging, and repetition only.
   You are a pair, not a replacement: never take the keyboard away.
5. Record lessons in pair.md (decision vs. implementation, explicitly); update todo.md
   status + result notes; remind the human to commit after each task batch.
6. If stalled on a root cause, mark the task BLOCKED with what was tried — escalation to lead
   review beats guessing.
