# Advanced serialization

When output is complex / needs heavy joins, serialize in a function — not only in `OutputSerializer` or the selector:

```python
class SomeGenericFeedApi(BaseApi):
    def get(self, request):
        feed = some_feed_get(user=request.user)
        return Response(some_feed_serialize(feed))


def some_feed_serialize(feed: List[FeedItem]):
    feed_ids = [item.id for item in feed]
    objects = (
        FeedItem.objects.select_related(...)
        .prefetch_related(...)
        .filter(id__in=feed_ids)
    )
    some_cache = get_some_cache(feed_ids)
    result = []
    for feed_item in objects:
        feed_item._calculated_field = some_cache.get(feed_item.id)
        result.append(FeedItemSerializer(feed_item).data)
    return result
```

Live in `<app>/serializers.py` as functions when DRF serializer classes are not enough.
