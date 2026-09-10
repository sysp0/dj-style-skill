---
name: django-styleguide
description: "Use when designing or reviewing Django architecture: where business logic should live, services vs selectors vs models vs APIs, anti-patterns (signals, fat views, fat serializers). Triggers on: Django structure, HackSoft styleguide, separation of concerns, service layer."
license: MIT
metadata:
  version: "1.0.0"
  author: "Reza Ghasemi"
  source: "https://github.com/HackSoftware/Django-Styleguide"
---

# Django Styleguide (HackSoft)

Opinionated, production-tested Django structure. Cherry-pick what fits the project.

## Core rule

**Business logic should live in:**

- Services (mostly writes)
- Selectors (mostly reads)
- Model properties / `clean` (simple, non-relational cases only)

**Business logic should NOT live in:**

- APIs / Views
- Serializers / Forms
- Model `save`
- Custom managers / querysets (as the whole domain)
- Signals (except loose coupling / cache invalidation)

## Core vs interface

How the app behaves (domain) must stay separate from how you talk to it (API, management command, Celery task, admin).

## Model properties vs selectors

Prefer a **selector** when:

- The value spans multiple relations
- Serialization would easily cause N+1

## Why not fat APIs / serializers

1. Logic fragments across layers → hard to trace
2. Generics hide behavior → hard to change

CRUD generics are fine for trivial cases; leave them once domain grows.

## Why not managers / signals for domain

- Domain ≠ data model; logic often spans many models + external systems
- Signals make heavy coupling implicit — prefer explicit service calls

## Starter projects

- [Django Styleguide Example](https://github.com/HackSoftware/Django-Styleguide-Example)
- [cookiecutter-django](https://github.com/cookiecutter/cookiecutter-django)

## Related skills

| Topic | Skill |
|-------|--------|
| Models / validation | `django-models` |
| Services / selectors | `django-services` |
| DRF APIs / URLs | `django-apis` |
| Settings / env | `django-settings` |
| Exception handling | `django-errors` |
| Tests | `django-testing` |
| Celery | `django-celery` |

## References

- `references/cookbook-model-update.md` — generic `model_update` pattern
- `references/dx-typing.md` — mypy / django-stubs notes
- Full source: repo root `django_styleguide_hacksoft.md`
