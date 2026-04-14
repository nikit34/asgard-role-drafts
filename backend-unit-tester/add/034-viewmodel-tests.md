## Larixon Backend Pure Logic Helper Tests

This slot covers unit-testable pure logic that is extracted from or supports Django views — permission predicates, query parameter parsing, response formatting helpers, pagination calculators.

### What belongs here

- Permission check functions callable without request context (e.g. `can_user_edit(user, advert) -> bool`)
- Query parameter parsing and validation helpers (e.g. `parse_price_range(params) -> tuple`)
- Custom pagination calculators (e.g. `calculate_offset(page, page_size) -> int`)
- Response formatting utilities (e.g. `format_price(amount, currency) -> str`)
- Rate limit calculation functions

### What does NOT belong here

- Full HTTP request/response cycle — that belongs in `backend-integ-tester`
- View classes tested via `self.client` — that belongs in `backend-integ-tester`
- `APITestCase` or `TestCase` with HTTP client — that belongs in `backend-integ-tester`
- Anything requiring `@pytest.mark.django_db` — that belongs in `backend-integ-tester`

### Example

```python
class TestPaginationHelper:
    def test_calculates_offset_from_page_number(self):
        result = calculate_offset(page=3, page_size=20)
        assert result == 40

    def test_raises_on_negative_page(self):
        with pytest.raises(ValueError, match="Page must be positive"):
            calculate_offset(page=-1, page_size=20)


class TestPermissionPredicate:
    def test_owner_can_edit(self):
        assert can_user_edit(user_id=1, owner_id=1) is True

    def test_non_owner_cannot_edit(self):
        assert can_user_edit(user_id=2, owner_id=1) is False
```

### Anti-patterns

- Using `self.client.get(...)` — this is an integration test, not a unit test
- Using `@pytest.mark.django_db` — if you need DB, the test belongs in integration
- Testing entire DRF viewset methods with real request objects
- Importing `APIRequestFactory` — request creation belongs to integration tests
