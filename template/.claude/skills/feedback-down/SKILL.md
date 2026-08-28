---
name: feedback-down
description: Record an explicit user-requested negative collaboration signal for the immediately preceding substantive assistant response, lower its owner role's project-scoped confidence, and preserve the mismatch cause. Do not use for ordinary criticism or automatic self-evaluation.
---

# Feedback down

1. Run only when the user explicitly invokes `feedback-down` or clearly asks to apply that named signal. Do not infer it from ordinary criticism.
2. Treat this as a meta-operation. It does not become or change the active `response owner role`.
3. Identify the target:
   - Use `lead`, `learn`, `work`, or `ops` when the user names one.
   - Otherwise use the owner role of the immediately preceding substantive assistant response.
   - If the response or owner role is ambiguous, ask one concise clarification and make no file change.
4. Read the collaboration-confidence rules in `PROJECT_RULES.md`, then read only `collaboration/memory/{role}_memory.md` and `collaboration/feedback/{role}.md`. Do not load another role's feedback log.
5. If the same signal for the same target is already the latest recorded event, do not count it twice unless the user explicitly says to repeat it.
6. Preserve existing calibration. If fields are absent, initialize confidence at `50%` and samples at `0`. Apply any required executor-change decay from `PROJECT_RULES.md` before this signal.
7. Subtract `10` from confidence, floor it at `20`, increment feedback samples by one, and update the memory's `updated` date, calibration date, and current executor when known.
8. Use the available response, result, and evidence to classify the likely cause as agent error, user premise, shared mismatch, external change, or unknown. The explicit signal lowers autonomy even when the factual dispute is unresolved, but it must not rewrite verified evidence as false.
9. Add or revise an active interaction adjustment only when a concrete future behavior follows from the feedback. Keep it concise, remove superseded guidance, and do not invent a rule from unexplained dissatisfaction.
10. Replace the feedback file's placeholder row for its first event or append one concise event. Record the target, user judgment and observed result, cause, promoted adjustment, and score transition. Do not copy the full response or sensitive data.
11. Do not undo work, change project scope, or issue a corrected substantive answer unless the user separately requests it.
12. Report only the target role, score transition, likely cause, and any adjustment recorded.
