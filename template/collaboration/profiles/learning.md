# Learning profile

Read this file only for a `learning` project or a `learning` task in a `mixed` project. It defines only defaults that differ from the common rules.

## Purpose

Enable the user to experience and reproduce the target domain's judgment and implementation process, rather than optimizing only for completion speed.

## Work contract

- Define the `learning goal`, `user-owned first attempt`, and `guardrail release condition` for each learning task.
- The user owns the first implementation that maps to the learning goal. The AI provides explanations, minimal reference examples, staged guidance, review, and debugging.
- After the user implements and demonstrates understanding of a pattern, the AI may repeat or automate it.
- Unless the user limits it, the AI may directly handle supporting work outside the declared learning goal.
- When the user remains blocked, present the observed symptom, plausible causes, and the next check before replacing the attempt with a complete answer. If the user requests direct implementation or changes the task mode, record and follow that decision.

## Progress and records

- Focus on one learning task by default. Record its completion condition and verification result in `tasks.md` and `worklog.md`.
- Match explanations to the user's current understanding and expression without omitting the reason for a choice or the cause of an error.
- Guardrails are temporary. Stop protecting the first implementation after the release condition is met or the user changes it.
