# Settings layout notes

| Path | Responsibility |
|------|----------------|
| `config/django/base.py` | Django settings + imports from `config/settings` |
| `config/django/production.py` | Import base; override only when needed |
| `config/django/test.py` | Point `pytest` / test runner here |
| `config/django/local.py` | Optional local overrides via `manage.py` |
| `config/settings/*` | Celery, CORS, Sentry, sessions, third parties |
| `config/env.py` | Shared `environ.Env()` instance |

Reference project: https://github.com/HackSoftware/Django-Styleguide-Example
