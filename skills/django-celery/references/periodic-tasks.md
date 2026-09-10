# setup_periodic_tasks

Deploy-time management command owns all beat schedules:

```python
class Command(BaseCommand):
    @transaction.atomic
    def handle(self, *args, **kwargs):
        IntervalSchedule.objects.all().delete()
        CrontabSchedule.objects.all().delete()
        PeriodicTask.objects.all().delete()

        periodic_tasks_data = [
            {
                "task": some_periodic_task,
                "name": "Do some periodic stuff",
                # https://crontab.guru/#15_*_*_*_*
                "cron": {
                    "minute": "15",
                    "hour": "*",
                    "day_of_week": "*",
                    "day_of_month": "*",
                    "month_of_year": "*",
                },
                "enabled": True,
            },
        ]

        for item in periodic_tasks_data:
            cron = CrontabSchedule.objects.create(**item["cron"])
            PeriodicTask.objects.create(
                name=item["name"],
                task=item["task"].name,
                crontab=cron,
                enabled=item["enabled"],
            )
```

Prefer crontab schedules; if using intervals, read django-celery-beat docs about sharing schedule rows.

Example: https://github.com/HackSoftware/Django-Styleguide-Example/blob/master/styleguide_example/tasks/management/commands/setup_periodic_tasks.py
