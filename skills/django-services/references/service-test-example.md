# Service test pattern

Service under test creates a Payment and delays a charge task:

```python
@transaction.atomic
def item_buy(*, item: Item, user: User) -> Payment:
    if item in items_get_for_user(user=user):
        raise ValidationError(f"Item {item} already in {user} items.")

    payment = Payment(item=item, user=user, successful=False)
    payment.full_clean()
    payment.save()

    transaction.on_commit(lambda: payment_charge.delay(payment_id=payment.id))
    return payment
```

Tests:

```python
class ItemBuyTests(TestCase):
    @patch("project.payments.services.items_get_for_user")
    def test_buying_item_that_is_already_bought_fails(self, mock_items):
        mock_items.return_value = [item]
        with self.assertRaises(ValidationError):
            item_buy(user=user, item=item)

    @patch("project.payments.services.payment_charge.delay")
    def test_buying_item_creates_payment_and_calls_charge(self, mock_charge):
        payment = item_buy(user=user, item=item)
        self.assertEqual(1, Payment.objects.count())
        mock_charge.assert_called_once()
```

Mock selectors you already unit-tested; mock task `.delay`; assert DB side effects.
