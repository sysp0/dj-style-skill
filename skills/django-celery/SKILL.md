---
name: django-celery
description: "Use when wiring Celery with Django services: thin tasks that call services, transaction.on_commit, import service inside task body, retry/on_failure, django-celery-beat setup_periodic_tasks. Triggers on: Celery, shared_task, celery beat, periodic task, on_commit delay."
license: MIT
metadata:
  version: "1.0.0"
  source: "https://github.com/HackSoftware/Django-Styleguide"
---

# Django Celery (HackSoft)

Treat Celery as **another interface** to core logic — **no business logic in tasks**.

## Pattern

1. Task fetches data → calls service
2. Import the **service inside the task body** (avoids circular imports)
3. Callers import the task at module level with `_task` suffix
4. Schedule with `transaction.on_commit(lambda: task.delay(...))`

```python
@shared_task
def email_send(email_id):
    email = Email.objects.get(id=email_id)
    from styleguide_example.emails.services import email_send
    email_send(email)


# caller
from styleguide_example.emails.tasks import email_send as email_send_task

@transaction.atomic
def user_complete_onboarding(user: User) -> User:
    email = email_get_onboarding_template(user=user)
    transaction.on_commit(lambda: email_send_task.delay(email.id))
    return user
```

## Error handling in the task

Retries / `on_failure` live on the task; callbacks still call services (`email_failed`).

See `references/retries.md`.

## Structure

- Default: `<app>/tasks.py`
- Grow → `tasks/domain_a.py` + import in `tasks/__init__.py` for autodiscover

## Periodic tasks

Use Celery Beat + `django-celery-beat` DatabaseScheduler.

Maintain a management command `setup_periodic_tasks` that **deletes and recreates** definitions from code (run on deploy). Link crontab.guru next to each cron.

See `references/periodic-tasks.md`.

## Beyond

Canvas workflows are fine if tasks still call a clear service interface.

## References

- Official Django first steps: https://docs.celeryq.dev/en/stable/django/first-steps-with-django.html
- Example: https://github.com/HackSoftware/Django-Styleguide-Example/tree/master/styleguide_example/tasks
- `references/retries.md`
- `references/periodic-tasks.md`
