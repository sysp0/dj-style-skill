---
name: django-apis
description: "Use when building Django REST Framework APIs: thin APIView, InputSerializer/OutputSerializer, list filters/pagination, URL patterns per action. Triggers on: DRF, APIView, serializer, django-filter, pagination, urls.py, nested serializer."
license: MIT
metadata:
  version: "1.0.0"
  source: "https://github.com/HackSoftware/Django-Styleguide"
---

# Django APIs & Serializers (HackSoft)

## API rules

- **1 API per operation** (4 APIs for CRUD)
- Prefer plain `APIView` / `GenericAPIView` over heavy generics
- **No business logic** in the API — call services/selectors
- Naming: `<Entity><Action>Api` → `UserCreateApi`, `CourseDetailApi`

## Serialization rules

- Dedicated **input** and **output** serializers
- Nest as `InputSerializer` / `OutputSerializer` on the API class
- Prefer plain `Serializer` over `ModelSerializer` (optional preference)
- Reuse serializers sparingly (coupling risk)
- Nested fields: `inline_serializer` util from Styleguide Example

## Minimal list

```python
class UserListApi(APIView):
    class OutputSerializer(serializers.Serializer):
        id = serializers.CharField()
        email = serializers.CharField()

    def get(self, request):
        users = user_list()
        return Response(self.OutputSerializer(users, many=True).data)
```

## Create / update

```python
class CourseCreateApi(APIView):
    class InputSerializer(serializers.Serializer):
        name = serializers.CharField()
        start_date = serializers.DateField()
        end_date = serializers.DateField()

    def post(self, request):
        serializer = self.InputSerializer(data=request.data)
        serializer.is_valid(raise_exception=True)
        course_create(**serializer.validated_data)
        return Response(status=status.HTTP_201_CREATED)
```

## Filters + pagination

- API validates query params (`FilterSerializer`)
- Selector applies filters (`django-filter` FilterSet)
- API paginates via helper (`get_paginated_response`)

See `references/list-filters-pagination.md`.

## Object fetching

Keep consistent: fetch in API with a small `get_object` util, or pass ids into services/selectors. Document the project choice.

## URLs

One URL per API; group by domain:

```python
course_patterns = [
    path("", CourseListApi.as_view(), name="list"),
    path("<int:course_id>/", CourseDetailApi.as_view(), name="detail"),
    path("create/", CourseCreateApi.as_view(), name="create"),
    path("<int:course_id>/update/", CourseUpdateApi.as_view(), name="update"),
]
urlpatterns = [path("courses/", include((course_patterns, "courses")))]
```

## References

- `references/list-filters-pagination.md`
- `references/advanced-serialization.md`
- `references/urls-tree.md`
