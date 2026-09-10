---
name: django-models
description: "Use when writing Django models: BaseModel, clean/full_clean, CheckConstraint, properties, methods, and model tests. Triggers on: Django model, full_clean, model validation, model property, IntegrityError, abstract BaseModel."
license: MIT
metadata:
  version: "1.0.0"
  source: "https://github.com/HackSoftware/Django-Styleguide"
---

# Django Models (HackSoft)

Models own the **data model**, not the domain.

## BaseModel

```python
from django.db import models
from django.utils import timezone


class BaseModel(models.Model):
    created_at = models.DateTimeField(db_index=True, default=timezone.now)
    updated_at = models.DateTimeField(auto_now=True)

    class Meta:
        abstract = True
```

## Validation: `clean` + `full_clean`

Put simple multi-field (non-relational) checks in `clean`. Call `full_clean()` in the **service** before `save`:

```python
def course_create(*, name: str, start_date: date, end_date: date) -> Course:
    obj = Course(name=name, start_date=start_date, end_date=end_date)
    obj.full_clean()
    obj.save()
    return obj
```

Move validation to the service when logic is complex or spans relations.

## Prefer DB constraints

```python
class Meta:
    constraints = [
        models.CheckConstraint(
            name="start_date_before_end_date",
            check=Q(start_date__lt=F("end_date")),
        )
    ]
```

Since Django 4.1, `full_clean()` also checks constraints → nicer `ValidationError` than raw `IntegrityError` on the service path.

## Properties & methods

**OK on the model** if simple and based on non-relational fields.

**Move to service/selector/util** if spanning relations or complex (N+1 risk when serialized).

Methods: use when arguments are required, or when setting one attribute must set others together (`set_new_secret`).

## Testing models

Test only extra behavior (clean, properties, methods). Prefer asserting `full_clean()` without hitting the DB when possible.

## References

- `references/examples.md` — Course / Token examples
