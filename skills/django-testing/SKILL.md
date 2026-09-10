---
name: django-testing
description: "Use when organizing Django tests: mirror apps with tests/models|services|selectors, naming test_<thing>.py and ThingTests, factory_boy factories. Triggers on: Django TestCase, factory_boy, test structure, service tests, selector tests."
license: MIT
metadata:
  version: "1.0.0"
  source: "https://github.com/HackSoftware/Django-Styleguide"
---

# Django Testing (HackSoft)

Split tests by layer — same boxes as production code.

```
app_name/
└── tests/
    ├── factories.py
    ├── models/
    │   └── test_some_model.py
    ├── selectors/
    │   └── test_some_selector.py
    └── services/
        └── test_some_service.py
```

## Naming

| Code | Test file | Test class |
|------|-----------|------------|
| `a_very_neat_service` | `tests/services/test_a_very_neat_service.py` | `AVeryNeatServiceTests` |
| `common/utils.py` | `tests/test_utils.py` | per-function cases |
| `common/utils/files.py` | `tests/utils/test_files.py` | match module tree |

## What to test where

| Layer | Focus |
|-------|--------|
| Models | Extra validation / properties / methods |
| Services | Domain exhaustively; hit DB; mock tasks/externals |
| Selectors | Query / permission filtering behavior |
| APIs | HTTP contract (thin — logic already in services) |

## Factories

Prefer `factory_boy` + fakes (`faker`) over giant fixtures.

Reads:

- https://www.hacksoft.io/blog/improve-your-tests-django-fakes-and-factories
- https://factoryboy.readthedocs.io/

## Talk

[Quality Assurance in Django — DjangoCon Europe 2022](https://www.youtube.com/watch?v=PChaEAIsQls)

## References

- `references/structure.md`
