---
name: django-settings
description: "Use when organizing Django settings: split django/ vs settings/, env via django-environ, DJANGO_ prefixes, optional integrations. Triggers on: Django settings, django-environ, .env, config/base.py, Sentry settings, cookiecutter-django settings."
license: MIT
metadata:
  version: "1.0.0"
  source: "https://github.com/HackSoftware/Django-Styleguide"
---

# Django Settings (HackSoft)

Follow cookiecutter-django layout with a clearer split:

```
config/
├── django/
│   ├── base.py
│   ├── local.py
│   ├── production.py
│   └── test.py
├── settings/
│   ├── celery.py
│   ├── cors.py
│   ├── sentry.py
│   └── sessions.py
├── env.py
├── urls.py
└── wsgi.py
```

## Rules

- Everything required is imported from `base.py`
- Production-only behavior via **env vars**, not mysterious production-only modules
- `test.py` / `local.py` import `base` then override

## env helper

```python
# config/env.py
import environ

env = environ.Env()
```

Read `.env` early in `base.py`:

```python
BASE_DIR = environ.Path(__file__) - 3
env.read_env(os.path.join(BASE_DIR, ".env"))
```

Never commit `.env`; ship `.env.example`.

## Prefixes

Prefix Django-specific vars with `DJANGO_` (`DJANGO_SETTINGS_MODULE`, `DJANGO_DEBUG`). Integration vars (`AWS_*`, `CELERY_*`) may stay unprefixed — **be consistent**.

## Optional integrations

```python
SENTRY_DSN = env("SENTRY_DSN", default="")

if SENTRY_DSN:
    import sentry_sdk
    # configure...
```

Gate with env presence / `USE_*` flags so local works without secrets.

## End of base.py

```python
from config.settings.cors import *  # noqa
from config.settings.sessions import *  # noqa
from config.settings.celery import *  # noqa
from config.settings.sentry import *  # noqa
```

## References

- `references/layout.md` — module responsibilities
