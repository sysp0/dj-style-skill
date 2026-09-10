---
name: django-services
description: "Use when implementing Django business logic: services, selectors, naming <entity>_<action>, keyword-only args, transaction.on_commit for tasks, service tests with mocks. Triggers on: Django service, selector, services.py, selectors.py, service layer, model_update."
license: MIT
metadata:
  version: "1.0.0"
  source: "https://github.com/HackSoftware/Django-Styleguide"
---

# Django Services & Selectors (HackSoft)

## Services (writes / domain actions)

Typical service:

- Lives in `<app>/services.py` (or `services/` package)
- Keyword-only args (`*`) unless 0–1 args
- Type-annotated
- Calls `full_clean()` then `save()` when creating/updating models
- May call other services, selectors, external systems, tasks

```python
def user_create(*, email: str, name: str) -> User:
    user = User(email=email)
    user.full_clean()
    user.save()

    profile_create(user=user, name=name)
    confirmation_email_send(user=user)
    return user
```

### Naming

Prefer `<entity>_<action>`: `user_create`, `item_buy`, `course_update`.

Greppable + natural namespaces (`user_*` in `users.py`).

### Class-based services

Use a class for namespace + reuse (create/update) or multi-step flows (`start` / `finish`). Prefer `@transaction.atomic` on mutating methods.

### Modules

Split `services.py` → `services/jwt.py`, `services/oauth.py` when the app grows. Export via `__init__.py` if desired.

## Selectors (reads)

- Services push; **selectors pull**
- Same style as services; live in `selectors.py`
- May return querysets, lists, or domain DTOs

```python
def user_list(*, fetched_by: User) -> Iterable[User]:
    user_ids = user_get_visible_for(user=fetched_by)
    return User.objects.filter(id__in=user_ids)
```

If the split does not fit the team, keep both as services.

## Testing services

1. Cover business logic thoroughly
2. Hit the database
3. Mock Celery / external I/O
4. Build state via factories, fakes, other services, or `objects.create`

Use `transaction.on_commit(lambda: task.delay(...))` so tasks run after commit.

## References

- `references/class-based-and-flows.md` — upload service / flow examples
- `references/service-test-example.md` — `item_buy` test pattern
- Related: `django-styleguide` → `cookbook-model-update.md`
