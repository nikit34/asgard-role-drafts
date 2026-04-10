## Larixon Backend Model and Data Layer Tests

Use this slot for Django models, managers, custom querysets, and any data-access layer.

### What model/data-layer tests must prove

- custom model methods produce correct results
- custom managers and querysets filter/annotate correctly
- model validation rules (`clean()`, `validators`) reject invalid data
- signals and hooks trigger expected side effects
- default values and auto-populated fields work correctly
- market- or feature-specific flags change behavior only where expected

### Django-specific patterns

- **Model pure logic** (properties, custom methods, validators): test with `Model.__new__(Model)` pattern — create instance without DB:
  ```python
  advert = Advert.__new__(Advert)
  advert.deadline = timezone.now() - timedelta(days=1)
  assert advert.is_expired() is True
  ```
- Alternatively, use `baker.prepare` (no DB save) to get a populated instance for pure-logic tests
- Use factories only for field population, never for DB persistence in this role
- **Manager and queryset tests** are the primary exception to the no-DB rule. Use `@pytest.mark.django_db` only for these, and keep them in a separate test class clearly named `TestAdvertManager` or `TestAdvertQuerySet` (per BUT-001: "unless the model method inherently requires ORM")
- For manager methods that aggregate or annotate, assert both the query result and the field values

### What NOT to test at this layer

- Do not test Django ORM internals (e.g. that `filter()` works)
- Do not test auto-generated admin or migration files
- Do not re-test serializer output here — that belongs in `032-serializer-tests`

Example (pure unit + DB-backed):

```python
class TestAdvertModel:
    def test_is_expired_returns_true_past_deadline(self):
        advert = Advert()
        advert.deadline = timezone.now() - timedelta(days=1)
        assert advert.is_expired() is True

@pytest.mark.django_db
class TestAdvertManager:
    def test_active_returns_only_active(self):
        baker.make("adverts.Advert", is_active=True)
        baker.make("adverts.Advert", is_active=False)
        assert Advert.objects.active().count() == 1
```

### Fixture rules

- Build the smallest meaningful model instance
- Inline only fields that matter for the behavior under test
- Reuse a factory if the file already has one
- Keep names domain-specific where that changes behavior (e.g. market, rubric, locale)

### Anti-patterns

- asserting only that a queryset is non-empty without checking actual values
- hiding all fields behind giant factory helpers until the test intent is unreadable
- testing model `__str__` as the only coverage for a model with complex business methods
- creating 20 model instances when the test only needs 2
