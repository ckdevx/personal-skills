# Reusable AGENTS.md template

Copy this file to a project's root and adapt it to that project. Remove this note and any sections that do not apply.

## Engineering principles

Optimize for human comprehension. Prefer the simplest implementation that meets current requirements.

- Prefer explicit, boring code over clever or overly generic code.
- Reuse repository patterns when they fit.
- Avoid designing for hypothetical needs or adding indirection without a concrete current requirement.
- When adding an abstraction, explain the problem it solves, why simpler code is insufficient, and what would be lost without it.
- Use plain language in code, documentation, ADRs, and specs.

## Skills

- For non-trivial implementations, use `simplicity-review` after implementation and before the final code review.
- For review requests, use `code-review-bounded` to assess requirements and repository standards, then stop after its bounded review process.

## Specs and issues

Keep specs and implementation issues as local Markdown files in `docs/`. Publish them to GitHub only when the user explicitly requests publication. This local workflow takes precedence over skill instructions that call for issue-tracker publication or GitHub labels.

## UI design

Avoid unnecessary secondary labels and descriptions. Let the layout do the explaining.
