---
name: django-errors
description: "Use when designing Django/DRF API error responses and custom exception handlers: map Django ValidationError, Http404, PermissionDenied; Approach 1 detail wrapper or Approach 2 message+extra ApplicationError. Triggers on: exception_handler, ValidationError, ApplicationError, API errors, RFC7807."
license: MIT
metadata:
  version: "1.0.0"
  source: "https://github.com/HackSoftware/Django-Styleguide"
---

# Django Errors & Exceptions (HackSoft)

## Checklist

1. Understand DRF exception handling
2. **Agree on error JSON shape early**
3. Install a custom `EXCEPTION_HANDLER`

Also consider [RFC 7807](https://datatracker.ietf.org/doc/html/rfc7807).

## Quirks to fix

- DRF `ValidationError` may return list **or** dict **or** `{"detail": ...}` inconsistently
- Django `ValidationError` (including from `full_clean`) becomes **500** unless mapped
- Map `Http404` / Django `PermissionDenied` to DRF equivalents

Minimal map:

```python
if isinstance(exc, DjangoValidationError):
    exc = exceptions.ValidationError(as_serializer_error(exc))
```

## Approach 1 — always wrap in `detail`

Normalize so clients always see:

```json
{ "detail": "..." }
```

or nested `detail` for field errors. See `references/approach-1-detail.md`.

## Approach 2 — HackSoft proposed (`message` + `extra`)

```json
{
  "message": "Validation error",
  "extra": { "fields": { "email": ["..."] } }
}
```

Raise domain errors via `ApplicationError(message=..., extra=...)` from a `core` app. Keep serializer/model `ValidationError` special-cased.

See `references/approach-2-application-error.md`.

## Guidance

- Do not silence unexpected exceptions (return `None` → real 500 + Sentry)
- Extend carefully (`ObjectDoesNotExist` → `NotFound`, etc.)

## References

- Example handlers: https://github.com/HackSoftware/Django-Styleguide-Example/blob/master/styleguide_example/api/exception_handlers.py
- `references/approach-1-detail.md`
- `references/approach-2-application-error.md`
