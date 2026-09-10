# Cookbook: `model_update`

Use a shared update helper for non-side-effect fields; keep side effects in the domain service.

```python
def user_update(*, user: User, data) -> User:
    non_side_effect_fields = ["first_name", "last_name"]

    user, has_updated = model_update(
        instance=user,
        fields=non_side_effect_fields,
        data=data,
    )

    # Side-effect fields / tasks here
    return user
```

Implementations + tests:

- https://github.com/HackSoftware/Django-Styleguide-Example/blob/master/styleguide_example/common/services.py
- https://github.com/HackSoftware/Django-Styleguide-Example/blob/master/styleguide_example/common/tests/services/test_model_update.py

Copy the tests when you copy `model_update`.
