# Architecture

## Standard

This repo follows the open **[Agent Skills](https://agentskills.io/home)** format ([specification](https://agentskills.io/specification)).

Each skill is a folder with a required `SKILL.md` (YAML frontmatter + instructions). Optional `references/`, `scripts/`, and `assets/` support progressive disclosure.

Distribution via `npx skills add` ([skills.sh](https://skills.sh)) is the install path for compatible agents — not a separate format.

## Goal

Package the HackSoft Django Styleguide as portable Agent Skills so any skills-compatible agent can load Django structure guidance on demand.

## Skill layout (per [spec](https://agentskills.io/specification))

```
skills/<skill-name>/
├── SKILL.md          # Required: name + description + instructions
└── references/       # Optional: deeper docs loaded on demand
```

Rules we follow:

| Rule | How we comply |
|------|----------------|
| `name` matches folder | e.g. `skills/django-services/` → `name: django-services` |
| `description` = what + when | Includes trigger keywords (services, DRF, Celery, …) |
| Progressive disclosure | Metadata at discovery; full `SKILL.md` on activation; `references/` only when needed |
| Keep `SKILL.md` lean | Under 500 lines / ~5k tokens; examples live in `references/` |
| One-level file refs | `SKILL.md` → `references/*.md` only (no deep chains) |
| Validate | `npx skills-ref validate skills/<name>` |

Repo-level files (`README.md`, `AGENTS.md`, `plugin.json`, source styleguide) are packaging/docs — they are not part of the Agent Skills file format itself.

## Progressive disclosure

From [Agent Skills overview](https://agentskills.io/home):

1. **Discovery** — agents load only `name` + `description`
2. **Activation** — matching task → full `SKILL.md` body
3. **Execution** — follow instructions; open `references/` when the task needs detail

`SKILL.md` = principles + short examples. Point agents to specific reference files when a pattern is needed (e.g. list filters → `references/list-filters-pagination.md`).

## Skill boundaries

Coherent units of domain expertise ([best practices](https://agentskills.io/skill-creation/best-practices)):

| Skill | Styleguide sections |
|-------|---------------------|
| `django-styleguide` | Overview, Why not, Cookie Cutter, Cookbook, DX |
| `django-models` | Models |
| `django-services` | Services, Selectors |
| `django-apis` | APIs & Serializers, Urls |
| `django-settings` | Settings |
| `django-errors` | Errors & Exception Handling |
| `django-testing` | Testing |
| `django-celery` | Celery |

## Validation

```bash
npx skills-ref validate skills/django-styleguide
npx skills add . --list
```

## References

- [Agent Skills home](https://agentskills.io/home)
- [Specification](https://agentskills.io/specification)
- [Quickstart](https://agentskills.io/skill-creation/quickstart)
- [Best practices](https://agentskills.io/skill-creation/best-practices)
