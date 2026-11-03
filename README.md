# Personal Skills

Reusable skills and project guidance for Codex and other compatible agents.

## Reusable guidance

[`AGENTS.md`](AGENTS.md) is a base template for project instructions. Copy it to a project's root, then adapt or remove sections to fit that project.

## Skills

| Skill | Use it for |
| --- | --- |
| [`code-review-bounded`](skills/code-review-bounded/SKILL.md) | Reviewing a change against its requirements and repository standards, with evidence-based findings and a defined stopping point. |
| [`implement`](skills/implement/SKILL.md) | Implementing work described in a spec or tickets, with regular checks and a final code review. |
| [`simplicity-review`](skills/simplicity-review/SKILL.md) | Reviewing a non-trivial implementation for complexity that can be removed without changing behavior. |

## Using the skills

- Ask for a review with `code-review-bounded`.
- Use `implement` for work described by a spec or tickets.
- Use `simplicity-review` after a non-trivial implementation and before the final code review.

## Installing skills

The installer uses `npx skills@latest` to fetch the selected skills from their source repositories. Install Node.js (which provides `npm` and `npx`), then run the script from PowerShell:

```powershell
.\install-skills.ps1
```

The Skills CLI uses project scope by default. The command above installs into this repository; to install into another project, keep that project as the current directory and invoke this script by its full path. To install globally instead, add `-g` to each `skills add` command in the script. See the [Skills CLI reference](https://github.com/vercel-labs/skills/blob/main/docs/commands.md) for options.

## Repository layout

```text
AGENTS.md                 Reusable project guidance template
install-skills.ps1        Installs the selected skills from their sources
skills/
  <skill-name>/
    SKILL.md              Skill instructions and frontmatter
    agents/               Optional agent interface metadata
    assets/               Optional skill assets
```

Keep each skill in its own directory under `skills/`. Only include the optional `agents/` and `assets/` directories when a skill needs them. Keep the root README as a concise catalog; skill-specific procedures belong in their `SKILL.md` files.
