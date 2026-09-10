# Class-based services & flows

## Namespace create/update

Use a service class when create and update share helpers:

```python
class FileStandardUploadService:
    def __init__(self, user: BaseUser, file_obj):
        self.user = user
        self.file_obj = file_obj

    @transaction.atomic
    def create(self, file_name: str = "", file_type: str = "") -> File:
        ...
        obj.full_clean()
        obj.save()
        return obj

    @transaction.atomic
    def update(self, file: File, file_name: str = "", file_type: str = "") -> File:
        ...
```

Call from thin APIs and admin `save_model` alike.

## Multi-step flow

```python
class FileDirectUploadService:
    def __init__(self, user: BaseUser):
        self.user = user

    @transaction.atomic
    def start(self, *, file_name: str, file_type: str) -> Dict[str, Any]:
        ...

    @transaction.atomic
    def finish(self, *, file: File) -> File:
        ...
```

Full examples: [Django Styleguide Example files/services.py](https://github.com/HackSoftware/Django-Styleguide-Example/blob/master/styleguide_example/files/services.py)
