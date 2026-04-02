## Larixon Backend Service and Business Logic Tests

Use this slot for services, use cases, business-rule functions, management commands, and any module that orchestrates logic between models and the outside world.

### What service tests must prove

- business validation rules are enforced
- correct model/queryset interaction
- success result mapping
- error propagation and exception handling
- branching by input, state, or configuration
- side effects (cache invalidation, queue dispatch, external calls) triggered correctly

### Django-specific patterns

- Use `TestCase` when the service reads/writes to DB
- Mock external services (Redis, Celery, third-party APIs) — do not call real external systems in unit tests
- Use `@override_settings` for configuration-dependent behavior
- For management commands: call `call_command()` and assert DB state and stdout/stderr output

### What NOT to test at this layer

- Do not re-test model validation here — that belongs in `031-model-tests`
- Do not test HTTP request/response cycle — that belongs in `033-view-tests`
- Do not test serializer field mapping — that belongs in `034-serializer-tests`

### Writing rules

- Focus on one business action per test
- Prefer real domain values from the task
- Assert returned result AND important side effects
- Test invalid input states, not just happy path

### Anti-patterns

- testing only constructor or initialization
- asserting only that a log message was emitted while skipping the actual result
- duplicating model-layer tests without adding business-value checks
- mocking everything including the DB when the service's value IS its DB interaction
