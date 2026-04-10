## Larixon Backend Unit Test Style

### Default rule

Follow the dominant style of the touched app/module. Larixon web/core is a Django monolith where conventions may vary between apps. Do not force a universal style across all apps.

### Python / pytest

- This role uses plain `pytest` classes (no `TestCase` subclass) per locked `030-test-patterns.md`
- `django.test.TestCase` and DRF `APITestCase` are for integration tests (`backend-integ-tester`), not this role
- Do not convert an entire existing file's structure just to add one test — but new test classes you write must be plain pytest classes with `mocker` fixture

### Naming conventions

- Test files: `test_{module}.py` or `tests_{module}.py` — follow existing convention in the app
- Test classes: `Test{Subject}{Context}` — e.g. `TestFacebookFeedXMLRendererUnit`
- Test methods: `test_{action}_{condition}_{expected}` — e.g. `test_toplevel_dicts_wrapped_in_listing`
- Keep names descriptive enough to serve as documentation

### Assertions

- Use plain `assert` statements — this role does not use `self.assert*` (that belongs in `TestCase`-based integration tests)
- Use `pytest.raises` for exception assertions
- For comparing complex structures, use plain `assert` with `==`

### Mandatory writing rules

- One test = one behavior or one closely related branch
- Use real domain names from the task, not placeholder entities
- Prefer stable factories/builders over giant inline object construction
- Keep comments rare and only for non-obvious setup
- If a module is legacy, extend the legacy style cleanly instead of starting a style war inside one file

### Parametrized tests

- Use `@pytest.mark.parametrize` for data-driven test variations
- Do not use `parameterized` or `subTest` — those are `TestCase` patterns; use `@pytest.mark.parametrize` exclusively
- Keep parametrize data readable — one row per scenario, not opaque tuples

### Do not

- Mix pytest assertions and `self.assert*` in the same file
- Rewrite the entire file structure when adding a single regression test
- Use `time.sleep()` in unit tests — use freezegun/time_machine for time-dependent logic
- Import production settings directly — use `@override_settings` for test-specific config
