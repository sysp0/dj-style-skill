# List API: filters + pagination

## API

```python
class UserListApi(ApiErrorsMixin, APIView):
    class Pagination(LimitOffsetPagination):
        default_limit = 1

    class FilterSerializer(serializers.Serializer):
        id = serializers.IntegerField(required=False)
        is_admin = serializers.NullBooleanField(required=False)  # avoid BooleanField default False
        email = serializers.EmailField(required=False)

    class OutputSerializer(serializers.Serializer):
        id = serializers.CharField()
        email = serializers.CharField()
        is_admin = serializers.BooleanField()

    def get(self, request):
        filters_serializer = self.FilterSerializer(data=request.query_params)
        filters_serializer.is_valid(raise_exception=True)
        users = user_list(filters=filters_serializer.validated_data)
        return get_paginated_response(
            pagination_class=self.Pagination,
            serializer_class=self.OutputSerializer,
            queryset=users,
            request=request,
            view=self,
        )
```

## Selector + django-filter

```python
class BaseUserFilter(django_filters.FilterSet):
    class Meta:
        model = BaseUser
        fields = ("id", "email", "is_admin")


def user_list(*, filters=None):
    filters = filters or {}
    qs = BaseUser.objects.all()
    return BaseUserFilter(filters, qs).qs
```

## Helper

Extract pagination from DRF generics into `get_paginated_response` so plain `APIView` stays thin.

Example project: https://github.com/HackSoftware/Styleguide-Example
