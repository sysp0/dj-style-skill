# Approach 1: DRF default + `detail` wrapper

```python
def drf_default_with_modifications_exception_handler(exc, ctx):
    if isinstance(exc, DjangoValidationError):
        exc = exceptions.ValidationError(as_serializer_error(exc))
    if isinstance(exc, Http404):
        exc = exceptions.NotFound()
    if isinstance(exc, PermissionDenied):
        exc = exceptions.PermissionDenied()

    response = exception_handler(exc, ctx)
    if response is None:
        return response

    if isinstance(exc.detail, (list, dict)):
        response.data = {"detail": response.data}
    return response
```

Clients always receive a dict with `detail`.
