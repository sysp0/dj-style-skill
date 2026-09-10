# Nested URL trees

Prefer explicit trees when domains nest (example: file upload):

```python
urlpatterns = [
    path(
        "upload/",
        include(
            (
                [
                    path("direct/", FileDirectUploadApi.as_view(), name="direct"),
                    path(
                        "pass-thru/",
                        include(
                            (
                                [
                                    path("start/", FilePassThruUploadStartApi.as_view(), name="start"),
                                    path("finish/", FilePassThruUploadFinishApi.as_view(), name="finish"),
                                ],
                                "pass-thru",
                            )
                        ),
                    ),
                ],
                "upload",
            )
        ),
    )
]
```

Either `domain_patterns` lists or inline trees are fine — pick one team convention.
