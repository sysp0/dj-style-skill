# AGENTS.md — django-styleguide skills

Develop and change skills according to the open [Agent Skills](https://agentskills.io/home) standard ([specification](https://agentskills.io/specification)). See `docs/ARCHITECTURE.md`.

## Repo Structure

```
.
├── skills/
│   ├── django-styleguide/     # Core architecture + where logic lives
│   ├── django-models/         # Models, clean, constraints, properties
│   ├── django-services/       # Services, selectors, model_update
│   ├── django-apis/           # Thin DRF APIs, serializers, URLs
│   ├── django-settings/       # Settings split + env patterns
│   ├── django-errors/         # Exception handling approaches
│   ├── django-testing/        # Test layout + factories
│   └── django-celery/         # Tasks as interface to services
├── docs/
│   └── ARCHITECTURE.md
├── django_styleguide_hacksoft.md   # Full source styleguide (reference)
├── plugin.json
└── README.md
```

Each skill folder follows Agent Skills layout:

- `SKILL.md` — required; YAML `name` + `description` + instructions (keep under ~500 lines)
- `references/` — optional deeper docs (progressive disclosure)

## Rules (always apply when using these skills)

1. **Business logic lives in services/selectors** — not in APIs, serializers, forms, `save()`, managers, or signals.
2. **Services write; selectors read** — unless the team deliberately merges both into services.
3. **Call `full_clean()` in the service before `save()`** for model validation.
4. **Thin APIs** — prefer plain `APIView`; nested `InputSerializer` / `OutputSerializer`.
5. **Naming** — services/selectors: `<entity>_<action>`; APIs: `<Entity><Action>Api`.
6. **Celery tasks are an interface** — fetch data, call a service; use `transaction.on_commit`.
7. **Cherry-pick** — adapt patterns to project context; do not force every rule blindly.

## Source

Patterns adapted from [HackSoft Django Styleguide](https://github.com/HackSoftware/Django-Styleguide).
Full local copy: `django_styleguide_hacksoft.md`.
