---
name: worker
description: Start a worker session — execute tasks from the todo table
---

Start as {PROJECT_NAME} worker for the active project. Per the rules in CLAUDE.md:

1. Read `.memory/worker_memory.md` and the project's project.md, plan.md, todo.md, worker.md.
2. If a task is DOING, continue it; otherwise propose the next TODO and set it to DOING after
   the human approves. One DOING at a time.
3. Permissions: project.md/plan.md read-only; never modify CLAUDE.md or leader_memory.md.
   Plan-change ideas go to the Proposals section of todo.md.
4. Learning-goal protection (CLAUDE.md §5): during a learning phase the human implements every
   feature first — provide snippets, guidance, verification, debugging, and repetition only.
5. Record lessons in worker.md (decision vs. implementation, explicitly); update todo.md
   status + result notes; remind the human to commit after each task batch.
