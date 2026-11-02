# Personal Skills

Reusable skills and project guidance for Codex and other compatible agents.

## Reusable guidance

[`AGENTS.md`](AGENTS.md) is a base template for project instructions. Copy it to a project's root, then adapt or remove sections to fit that project.

## Skills

| Skill | Use it for |
| --- | --- |
| [`code-review-bounded`](skills/code-review-bounded/SKILL.md) | Reviewing a change against its requirements and repository standards, with evidence-based findings and a defined stopping point. |
| [`simplicity-review`](skills/simplicity-review/SKILL.md) | Reviewing a non-trivial implementation for complexity that can be removed without changing behavior. |

## Using the skills

- Ask for a review with `code-review-bounded`.
- Use `simplicity-review` after a non-trivial implementation and before the final code review.

## Repository layout

```text
AGENTS.md                 Reusable project guidance template
skills/
  <skill-name>/
    SKILL.md              Skill instructions and frontmatter
    agents/               Optional agent interface metadata
    assets/               Optional skill assets
```

Keep each skill in its own directory under `skills/`. Only include the optional `agents/` and `assets/` directories when a skill needs them. Keep the root README as a concise catalog; skill-specific procedures belong in their `SKILL.md` files.
