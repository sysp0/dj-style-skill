# Model examples

## Course with clean + properties

```python
from django.core.exceptions import ValidationError
from django.utils import timezone


class Course(BaseModel):
    name = models.CharField(unique=True, max_length=255)
    start_date = models.DateField()
    end_date = models.DateField()

    def clean(self):
        if self.start_date >= self.end_date:
            raise ValidationError("End date cannot be before start date")

    @property
    def has_started(self) -> bool:
        return self.start_date <= timezone.now().date()

    @property
    def has_finished(self) -> bool:
        return self.end_date <= timezone.now().date()

    def is_within(self, x: date) -> bool:
        return self.start_date <= x <= self.end_date
```

## Coupled attribute setter

```python
class Token(BaseModel):
    secret = models.CharField(max_length=255, unique=True)
    expiry = models.DateTimeField(blank=True, null=True)

    def set_new_secret(self):
        now = timezone.now()
        self.secret = get_random_string(255)
        self.expiry = now + settings.TOKEN_EXPIRY_TIMEDELTA
        return self
```

## Model test without DB write

```python
class CourseTests(TestCase):
    def test_course_end_date_cannot_be_before_start_date(self):
        course = Course(
            start_date=timezone.now(),
            end_date=timezone.now() - timedelta(days=1),
        )
        with self.assertRaises(ValidationError):
            course.full_clean()
```
