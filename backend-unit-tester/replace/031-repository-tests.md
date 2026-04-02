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

- Use `TestCase` (with DB) for tests that need real model instances
- Use factories to create model instances — avoid raw `Model.objects.create()` with dozens of fields
- Test querysets against real DB, not mocked ORM — Django's `TestCase` wraps each test in a transaction
- For manager methods that aggregate or annotate, assert both the query result and the field values

### What NOT to test at this layer

- Do not test Django ORM internals (e.g. that `filter()` works)
- Do not test auto-generated admin or migration files
- Do not re-test serializer output here — that belongs in `034-serializer-tests`

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
