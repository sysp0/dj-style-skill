# Approach 2: `message` + `extra` + `ApplicationError`

Target shape:

```json
{ "message": "...", "extra": {} }
```

Validation errors:

```json
{
  "message": "Validation error",
  "extra": { "fields": { "email": ["This field cannot be blank."] } }
}
```

Handler sketch:

```python
def hacksoft_proposed_exception_handler(exc, ctx):
    if isinstance(exc, DjangoValidationError):
        exc = exceptions.ValidationError(as_serializer_error(exc))
    if isinstance(exc, Http404):
        exc = exceptions.NotFound()
    if isinstance(exc, PermissionDenied):
        exc = exceptions.PermissionDenied()

    response = exception_handler(exc, ctx)

    if response is None:
        if isinstance(exc, ApplicationError):
            return Response(
                {"message": exc.message, "extra": exc.extra},
                status=400,
            )
        return response

    if isinstance(exc.detail, (list, dict)):
        response.data = {"detail": response.data}

    if isinstance(exc, exceptions.ValidationError):
        response.data = {
            "message": "Validation error",
            "extra": {"fields": response.data["detail"]},
        }
    else:
        response.data = {
            "message": response.data["detail"],
            "extra": {},
        }
    return response
```

Extend with `ApplicationValidationError` / `ApplicationPermissionError` as needed.
