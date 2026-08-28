---
name: feedback-up
description: Record an explicit user-requested positive collaboration signal for the immediately preceding substantive assistant response, raise its owner role's project-scoped confidence, and preserve concise evidence. Do not use for ordinary praise or as technical verification.
---

# Feedback up

1. Run only when the user explicitly invokes `feedback-up` or clearly asks to apply that named signal. Do not infer it from ordinary praise.
2. Treat this as a meta-operation. It does not become or change the active `response owner role`.
3. Identify the target:
   - Use `lead`, `learn`, `work`, or `ops` when the user names one.
   - Otherwise use the owner role of the immediately preceding substantive assistant response.
   - If the response or owner role is ambiguous, ask one concise clarification and make no file change.
4. Read the collaboration-confidence rules in `PROJECT_RULES.md`, then read only `collaboration/memory/{role}_memory.md` and `collaboration/feedback/{role}.md`. Do not load another role's feedback log.
5. If the same signal for the same target is already the latest recorded event, do not count it twice unless the user explicitly says to repeat it.
6. Preserve existing calibration. If fields are absent, initialize confidence at `50%` and samples at `0`. Apply any required executor-change decay from `PROJECT_RULES.md` before this signal.
7. Add `5` to confidence, cap it at `80`, increment feedback samples by one, and update the memory's `updated` date, calibration date, and current executor when known.
8. Update active interaction adjustments only when the feedback identifies a concrete reusable preference or successful behavior. Pure approval still changes the score but does not justify an invented rule.
9. Replace the feedback file's placeholder row for its first event or append one concise event. Record the target, user judgment and observed result, cause, promoted adjustment, and score transition. Do not copy the full response or sensitive data.
10. Treat user approval as collaboration evidence, not proof that technical claims are correct. Do not alter project scope, implementation, or verification records unless the user separately requests that work.
11. Report only the target role, score transition, and any adjustment recorded.
