## Factories and Fixtures

### Factory library

- Use `model_bakery` (`baker.make`, `baker.prepare`) — this is the library used in the repo
- If `factory_boy` is found in specific apps, follow that app's convention
- Do not introduce a new factory library

### Shared fixtures

- App-level shared fixtures live in `{app}/tests/conftest.py`
- Cross-app fixtures (if any) live in a top-level `conftest.py`
- Use `@pytest.fixture` for pytest-style tests, `setUp()` for `TestCase` subclasses

### Writing rules

- Build the smallest meaningful model instance
- Inline only fields relevant to the behavior under test
- Use `baker.make` for DB-persisted instances, `baker.prepare` for in-memory only
- Keep factory names domain-specific: `active_advert`, `expired_advert`, not `advert1`, `advert2`
- Each test must create its own data. Do not rely on data left over from another test
- Fixtures must be stateless between tests — never mutate a shared fixture and expect the mutation to persist or reset automatically

### Anti-patterns

- Do not share mutable state across tests via class attributes or module-level variables
- Do not use a single "god fixture" that creates an entire object graph — build only what the test needs
- Do not hardcode PKs — let the database assign them
- Do not call `baker.make` at module level; call it inside the test function or `setUp()`

### Example

```python
import pytest
from model_bakery import baker


@pytest.fixture
def active_advert(db):
    return baker.make("adverts.Advert", status="active", price=100)


def test_advert_is_visible_in_feed(active_advert, api_client):
    response = api_client.get(reverse("facebook-feed-list"))
    assert response.status_code == 200
```
