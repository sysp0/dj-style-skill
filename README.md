# Django Styleguide Skills

[Agent Skills](https://agentskills.io/home)-compatible packages based on the [HackSoft Django Styleguide](https://github.com/HackSoftware/Django-Styleguide) — services, selectors, models, DRF APIs, settings, errors, testing, and Celery.

Built to the [Agent Skills specification](https://agentskills.io/specification); install with `npx skills add` ([skills.sh](https://skills.sh)).

## Features

- **Services & Selectors** — business logic outside views/serializers
- **Models** — `BaseModel`, `clean`/`full_clean`, constraints, properties
- **DRF APIs** — thin APIs, input/output serializers, filters, pagination
- **Settings** — cookiecutter-style split config + env patterns
- **Errors** — consistent DRF exception handling
- **Testing** — layer-aligned tests + factories
- **Celery** — tasks as interface to services, periodic setup

## Skills

| Skill | Use when |
|-------|----------|
| `django-styleguide` | Architecture overview, where logic lives, anti-patterns |
| `django-models` | Models, validation, properties, methods |
| `django-services` | Services, selectors, naming, `model_update` |
| `django-apis` | DRF APIs, serializers, URLs |
| `django-settings` | Settings layout, env vars, integrations |
| `django-errors` | Exception handlers, API error shapes |
| `django-testing` | Test layout, naming, factories |
| `django-celery` | Tasks, retries, beat periodic tasks |

## Installation

### npx ([skills.sh](https://skills.sh))

```bash
# Install all skills from this repo
npx skills add https://github.com/sysp0/dj-style-skill --skill '*'

# Or pick specific skills
npx skills add https://github.com/sysp0/dj-style-skill --skill django-styleguide
npx skills add https://github.com/sysp0/dj-style-skill --skill django-services
npx skills add https://github.com/sysp0/dj-style-skill --skill django-apis
```

List available skills without installing:

```bash
npx skills add https://github.com/sysp0/dj-style-skill --list
```

### Local path (development)

```bash
npx skills add . --skill '*' -y
```

### Git clone

```bash
git clone https://github.com/sysp0/dj-style-skill.git
```

## Example prompts

- "Create a `user_create` service following HackSoft styleguide"
- "Refactor this DRF ViewSet into thin APIs + selectors"
- "Add `full_clean` validation before save in the service"
- "Set up Celery so tasks only call services"
- "Design a custom DRF exception handler with `message` + `extra`"

## Attribution

Content is adapted from the HackSoft [Django Styleguide](https://github.com/HackSoftware/Django-Styleguide) and [Django Styleguide Example](https://github.com/HackSoftware/Django-Styleguide-Example).

Original authors: [HackSoft](https://hacksoft.io). This package packages those patterns as installable agent skills.

## License

MIT — see [LICENSE](LICENSE).

Styleguide source material remains attributed to HackSoft under their upstream license.
