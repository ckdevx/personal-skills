---
name: code-review-bounded
description: "Review a PR, MR, branch, diff, or working changes against requirements and repository standards, using evidence and severity to decide merge readiness. Use for initial code reviews, reviewing since a Git reference, or verifying review fixes. Limit speculative edge cases, unnecessary abstractions, and repeated review cycles."
---

# Bounded Code Review

Determine whether the requested change meets its requirements with acceptable operational risk. Optimize for correctness, understandable code, and a finite decision. Do not seek every possible improvement. A review with no findings is valid.

Use two lenses: **Spec** for intended behavior and **Standards** for repository rules and maintainability. Evaluate them separately, then combine their evidence into one severity-based decision. Keep the lens on each finding so neither gets hidden by the other.

Use this as a standalone skill. Do not require Matt Pocock's other skills, a particular issue tracker, or a particular language.

## 1. Establish the review contract

Read applicable `AGENTS.md` files and identify:

- The change, baseline, and exact reviewed revision or working snapshot.
- The issue, PRD/spec, acceptance criteria, expected failures, and explicit non-goals.
- Applicable standards, including `CONTRIBUTING.md`, `CODING_STANDARDS.md`, relevant ADRs, and subsystem guidance.
- Required checks and available test/manual evidence.
- Whether this is an **initial review** or **verification of previous fixes**.

Prefer user-supplied sources, then linked issue/PR descriptions and relevant repository documents. Use documented tracker access when available. If no written spec exists, use explicit user intent and established contracts; state that narrower basis. Do not invent requirements or make missing documentation alone a defect. Ask only when unresolved scope or intent materially changes the decision; continue reviewing unambiguous parts.

For Git reviews, resolve references to SHAs before reading the diff. For branch/PR changes, compare the target merge-base to the reviewed head. For an explicitly requested exact commit comparison, use those endpoints rather than substituting a merge-base. Include staged, unstaged, and relevant untracked files only when reviewing working changes; ordinary branch diffs omit them. Report an empty diff rather than inventing a target. Do not switch branches or modify user work merely to inspect it.

Keep the baseline and scope stable through verification. Record the initial reviewed snapshot so fix changes can be distinguished. If unavailable, disclose that limitation rather than guessing what changed. Do not silently review unrelated parts of the repository.

## 2. Review the change once

Read changed code and enough callers, contracts, configuration, and tests to trace actual behavior. Follow dependencies to confirm suspected defects. Focus on defects introduced, exposed, or worsened by this change. Mention unrelated pre-existing issues separately only when serious and relevant.

**Spec lens:** Check acceptance criteria, expected failures, compatibility, and unjustified scope expansion. Prioritize security boundaries, persistent data, external integrations, and operational failures relevant to the change. Connect incorrect or missing behavior to a specific requirement or established contract.

**Standards lens:** Cite the applicable repository rule for claimed violations. Treat design smells as clues, never automatic failures. Investigate unnecessary indirection, speculative extensibility, duplicated business rules, or confusing responsibilities only when they create a concrete cost in changed code. Prefer local clarity; do not introduce interfaces, factories, or domain types merely to satisfy a heuristic. Repository conventions take precedence over general design preferences.

Avoid reproducing lint/type/format output as subjective findings. Summarize failed required tooling as validation failures instead. A documented mandatory rule can prevent readiness without being a HIGH bug; report it as a required gate without inflating severity.

Use targeted checks when they resolve meaningful uncertainty and are safe in the environment. Review is read-only by default: do not edit code, add tests, create tickets, post comments, merge, or deploy unless authorized. Suggest a focused regression check for a substantiated defect; do not demand tests for every hypothetical scenario.

## 3. Require evidence

For each candidate, establish:

1. **Location:** A narrow changed location and any relevant caller or contract.
2. **Trigger:** A reachable input, execution path, or deployment condition.
3. **Failure:** Actual versus required behavior and its consequence.
4. **Evidence:** Code trace, reproduction, failing check, or specific requirement. Label inference. Reproduction is useful but unnecessary when the code path proves the failure.
5. **Risk:** Impact, likelihood, and justification for severity.

Try to disprove the concern by checking validation, runtime types, authorization, transactions, constraints, and upstream guarantees. Avoid redundant handling behind an enforced contract. Static annotations, casts, and unchecked external data are not runtime guarantees.

Discard unsupported concerns and preferences. Put unresolved material uncertainty under **Unverified checks**, specifying the missing evidence rather than claiming a guessed defect. Missing critical evidence prevents unconditional readiness; it does not justify inflating speculative findings.

Deduplicate root causes and assign stable IDs such as `R1`. Do not split symptoms to inflate the count or reopen rejected/deferred items without new evidence. Keep a compact ledger in the report or conversation; do not create a file unless requested.

## 4. Control edge cases and complexity

Consider an edge case when it is required, plausible under actual production conditions, or could have serious security, integrity, or availability impact. Investigate rare catastrophic failures when the path is credible; low probability alone does not dismiss them.

Do not assume arbitrary scale, unsupported environments, or impossible internal states. Check external boundaries for realistic malformed input, timeouts, and failures when relevant. Do not mandate retries, fallbacks, concurrency controls, or generic provider support without demonstrated need. Cheap handling may make an evidenced optional fix worthwhile; cheapness alone never creates a requirement or merge blocker.

Judge remedies against their complexity cost. Prefer a local correction, removing unnecessary code, or an established mechanism. Additional abstractions and configuration need justification. Do not prescribe architecture changes for MEDIUM/LOW findings. If a serious defect needs a broader change, explain why a smaller fix cannot preserve required behavior.

## 5. Classify and decide

| Severity | Threshold | Default action |
| --- | --- | --- |
| BLOCKER | Credible sensitive-data exposure, unauthorized critical actions, destructive data loss, or widespread service failure | Fix before merge |
| HIGH | Concrete acceptance-criteria failure, important broken workflow, or likely production defect with substantial impact | Fix before merge |
| MEDIUM | Evidenced defect with limited impact or an uncommon noncritical trigger | Offer a small fix or defer; no automatic review cycle |
| LOW | Concrete but minor readability/maintenance concern | Optional; usually defer |
| NIT | Style preference or equivalent alternative | Omit unless requested |

Assign severity from demonstrated consequences. Do not call all security-related code BLOCKER, all missing tests HIGH, or all rule violations bugs. Mandatory acceptance/project gates still apply. An explicitly required workflow that does not work is a HIGH finding even when its implementation is small.

Do not automatically implement optional suggestions or create backlog work. Recommend tracking only worthwhile items. Do not manufacture speculative improvement buckets; empty optional sections are valid.

## 6. Bound verification and stop

Default to **one full initial review and one focused verification pass**, not two full reviews. If no must-fix findings exist and required evidence is complete, stop after the initial review.

On verification:

- Retain the scope, baseline, requirements, and finding IDs.
- Verify required fixes and inspect the fix diff and directly affected paths for BLOCKER/HIGH regressions.
- Mark prior findings resolved, unresolved, deferred, or rejected, with a brief reason.
- Do not restart the whole review, mine untouched code for improvements, or reopen optional items.
- Report newly discovered serious defects whenever credible evidence emerges, even beyond the narrow fix diff; explain why they warrant expanded attention.

After verification, return a decision and stop. If required fixes remain, return **CHANGES REQUIRED**, state the remaining work and targeted verification needed, and hand control back to the user/calling workflow. Do not launch unlimited fix/review loops or approve because the pass budget expired. Further reviews must be explicitly requested or part of an already authorized bounded workflow; focus them on unresolved issues. A materially changed implementation may justify a new initial review with its scope stated.

Inside an implementation workflow, interpret this report as follows: repair BLOCKER/HIGH findings and mandatory gate failures; do not convert every optional suggestion into a task. Trigger another review only for required fixes. An instruction to "fix review findings" does not authorize MEDIUM/LOW/NIT scope expansion unless the user explicitly requests it.

Return **MERGE READY** only when the reviewed scope satisfies its requirements, no BLOCKER/HIGH defect remains, mandatory checks are satisfied, and required manual checks are completed or credibly reported. State supporting evidence and distinguish checks actually run from checks reported by others. The verdict is advice; do not merge.

Return **REVIEW INCOMPLETE** when missing intent, access, or required validation prevents a supported decision and no confirmed must-fix defect already requires **CHANGES REQUIRED**. Specify the smallest missing check or answer. Failed required checks prevent readiness even without a confirmed code defect. Never infer readiness solely from zero findings or passing happy-path tests.

## 7. Report concisely

Use this format, omitting empty optional sections:

```markdown
Verdict: MERGE READY | CHANGES REQUIRED | REVIEW INCOMPLETE
Scope: baseline → reviewed revision/snapshot; initial or verification
Basis: requirements and standards used; important scope assumptions

### Must fix before merge
- R1 — HIGH — Spec — path:line
  Trigger and failure: <reachable scenario; expected vs actual behavior>
  Evidence and impact: <trace/check/requirement; why it matters>
  Smallest remedy: <proportionate correction and useful verification>

### Required gates / Unverified checks
- <failed or missing check, provenance, and effect on readiness>

### Optional improvements
- R2 — MEDIUM — Standards — path:line: <evidenced concern and defer/fix rationale>

### Verification ledger
- R1: resolved | unresolved | deferred | rejected — <evidence/reason>

### Evidence
- Spec: <criteria assessed and limitations>
- Standards: <rules assessed and limitations>
- Checks: <actually run / reported / not run; relevant results>
- Manual checks: <performed / user-reported / pending / not applicable>

Next action: <specific required work, or "No further review required for this scope.">
```

Use plain language and narrow actionable findings. On a clean review, use a short verdict, scope, and evidence summary instead of filling a template. State zero findings when appropriate. Keep unrelated improvements out of the closing recommendation.

## Reference

Consulted: [Matt Pocock's code-review skill](https://github.com/mattpocock/skills/blob/main/skills/engineering/code-review/SKILL.md), retrieved 2026-10-07. Preserve its separation of requirement and standards checks and stable review baseline. Apply the evidence, complexity, severity, and stopping policies above to the merge recommendation. Keep the lenses separate during analysis; parallel reviewers are optional when authorized and useful, and must share these same policies.
