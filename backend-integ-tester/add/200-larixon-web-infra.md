## Larixon Web Integration Test Infrastructure

### Scope

This role covers single-endpoint Django stack tests (HTTP → View → Serializer → Service → ORM → real DB). Multi-step flows spanning multiple user sessions belong in e2e-tester.

- Repository: `https://gitlab.dev.larixon.com/larixon-classifieds/web/core`
- Local path convention: `~/Core`
- Feature branch convention: branch name matches Jira ticket number (e.g. `CD-4651`)

### CI and ownership

- CI runs through GitLab CI pipelines.
- Integration tests run as part of the CI pipeline after merge. The tester role writes and validates tests locally; CI execution belongs to the automated merge/devops flow.
- Do not instruct the user to manually trigger CI unless the task explicitly asks for CI handoff details.

### Local execution

- Run all tests: `python -m pytest`
- Run integration tests for an app: `python -m pytest adverts/tests/views/`
- Run a specific integration test: `python -m pytest adverts/tests/views/feeds/tests_common.py::FacebookFeedXMLStructureTest`
- Run with verbose output: `python -m pytest -v adverts/tests/views/feeds/`

### Environments

Always name the exact stand when specifying test prerequisites:

- Shared stands: `stand1.dev.larixon.com` through `stand13.dev.larixon.com`
- Market-specific: `bazaraki-master.dev.larixon.com`, `somon-master.dev.larixon.com`, `unegui-master.dev.larixon.com`, `jacars-master.dev.larixon.com`

"dev" or "staging" alone is not enough — always specify the exact stand URL.

### Test management and reporting

- **Allure TestOps** is the primary reporting system (not TestRail):
  - Base URL: `https://larixon.testops.cloud`
  - Web project: `https://larixon.testops.cloud/project/1/launches`
- Do not route this workflow through TestRail.

### Integration test patterns

Integration tests prove that the **full Django stack works correctly for a single endpoint** — routing, view, serializer, service layer, ORM, and real database together.

The platform default uses pytest with `APIClient` fixture. In this project, both pytest-style and Django `APITestCase` exist — follow the dominant style of the test module.

Patterns:

- Use `APITestCase` with `self.client` or pytest with `api_client` fixture for endpoint tests
- Use `reverse()` for all URL resolution — never hardcode URL strings
- Create DB fixtures with factories reflecting realistic state
- Assert full response cycle: status code, body structure, key field values
- For XML endpoints: parse response content with `ET.fromstring()` and use `find()` / `findall()`
- For paginated endpoints: assert `count`, `next`, `results` structure

Example (single-endpoint integration — verify feed returns expected structure):

```python
from django.urls import reverse

class TestFacebookFeed(APITestCase):
    def test_facebook_feed_returns_listings(self):
        self.client.force_authenticate(user=self.seller)
        response = self.client.get(reverse("facebook-feed-list"))
        self.assertEqual(response.status_code, 200)
        root = ET.fromstring(response.content)
        listings = root.findall(".//listing")
        self.assertTrue(len(listings) >= 0)
```

### Real project examples

- `adverts/tests/views/feeds/tests_common.py` — XML structure, filter conditions, pagination

### Reporting rule

Every integration result must include:

- endpoint(s) and HTTP method(s) tested
- stand URL (if tested against a running instance)
- test run artifact path
- factories/fixtures created

CI pipeline execution and configuration are out of scope for this role. Do not report on CI status or suggest CI changes unless the task explicitly asks for it.
