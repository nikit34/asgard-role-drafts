## Larixon Backend Model and Data Layer Tests

Use this slot for Django models and any data-access layer logic that can be tested without a database.

### What model/data-layer tests must prove

- custom model methods produce correct results
- model validation rules (`clean()`, `validators`) reject invalid data
- default values and auto-populated fields work correctly
- market- or feature-specific flags change behavior only where expected

### Django-specific patterns — pure unit (no DB)

All tests in this role MUST be pure unit — no `@pytest.mark.django_db`, no database access.

- **Model pure logic** (properties, custom methods, validators): use `Model.__new__(Model)` pattern — create instance without DB:
  ```python
  advert = Advert.__new__(Advert)
  advert.deadline = timezone.now() - timedelta(days=1)
  assert advert.is_expired() is True
  ```
- Alternatively, use `baker.prepare` (no DB save) to get a populated instance for pure-logic tests:
  ```python
  advert = baker.prepare("adverts.Advert", deadline=timezone.now() - timedelta(days=1))
  assert advert.is_expired() is True
  ```
- Use `baker.prepare` only for field population, never `baker.make` (which persists to DB)
- For validators, instantiate the validator directly and call it with test data

### Manager and queryset tests — NOT in this role

Manager/queryset tests require `@pytest.mark.django_db` and `baker.make`, which makes them integration tests. These belong in `backend-integ-tester`, not here.

If the task assigns manager/queryset scenarios (e.g. `Advert.objects.active()`), route them to the backend-integ-tester in your output or escalate via `/tracker-comment`.

### What NOT to test at this layer

- Do not test Django ORM internals (e.g. that `filter()` works)
- Do not test auto-generated admin or migration files
- Do not re-test serializer output here — that belongs in `032-serializer-tests`
- Do not use `@pytest.mark.django_db` — if you need DB, the test belongs in `backend-integ-tester`

Example (pure unit only):

```python
class TestAdvertModel:
    def test_is_expired_returns_true_past_deadline(self):
        advert = Advert.__new__(Advert)
        advert.deadline = timezone.now() - timedelta(days=1)
        assert advert.is_expired() is True

    def test_is_expired_returns_false_future_deadline(self):
        advert = baker.prepare("adverts.Advert", deadline=timezone.now() + timedelta(days=1))
        assert advert.is_expired() is False
```

### Fixture rules

- Build the smallest meaningful model instance
- Inline only fields that matter for the behavior under test
- Reuse a factory if the file already has one
- Keep names domain-specific where that changes behavior (e.g. market, rubric, locale)

### Anti-patterns

- using `@pytest.mark.django_db` in this role (route to integ-tester instead)
- using `baker.make` instead of `baker.prepare` (make = DB write)
- asserting only that a queryset is non-empty without checking actual values
- hiding all fields behind giant factory helpers until the test intent is unreadable
- testing model `__str__` as the only coverage for a model with complex business methods
