## Larixon Backend View Logic Tests

This slot covers unit-testable logic extracted from or related to Django views — permission predicates, query parameter parsing, response formatting helpers, pagination logic.

### What belongs here

- Permission check functions that are callable without request context
- Query parameter parsing and validation helpers
- Custom pagination classes (`.paginate_queryset` logic)
- Response formatting utilities
- Rate limit calculation functions

### What does NOT belong here

- Full HTTP request/response cycle — that belongs in `backend-integ-tester`
- View classes tested via `self.client` — that belongs in `backend-integ-tester`
- `APITestCase` or `TestCase` with HTTP client — that belongs in `backend-integ-tester`

### Example

```python
class TestPaginationHelper:
    def test_calculates_offset_from_page_number(self):
        result = calculate_offset(page=3, page_size=20)
        assert result == 40

    def test_raises_on_negative_page(self):
        with pytest.raises(ValueError, match="Page must be positive"):
            calculate_offset(page=-1, page_size=20)
```

### Anti-patterns

- Using `self.client.get(...)` — this is an integration test, not a unit test
- Using `@pytest.mark.django_db` — if you need DB, the test belongs in integration
- Testing entire DRF viewset methods with real request objects
