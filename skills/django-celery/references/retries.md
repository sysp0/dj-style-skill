# Celery retries + on_failure

```python
logger = get_task_logger(__name__)


def _email_send_failure(self, exc, task_id, args, kwargs, einfo):
    email = Email.objects.get(id=args[0])
    from styleguide_example.emails.services import email_failed
    email_failed(email)


@shared_task(bind=True, on_failure=_email_send_failure)
def email_send(self, email_id):
    email = Email.objects.get(id=email_id)
    from styleguide_example.emails.services import email_send

    try:
        email_send(email)
    except Exception as exc:
        logger.warning(f"Exception while sending email: {exc}")
        self.retry(exc=exc, countdown=5)
```

Naming: `_{task_name}_failure` for the failure callback.
