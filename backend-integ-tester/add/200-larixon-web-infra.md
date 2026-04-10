## Larixon Web Integration Test Infrastructure

### Scope

This role covers integration testing for the Larixon backend/web Django monolith. Integration tests exercise **cross-app or multi-service flows** through the Django test client with real database — scenarios that span multiple endpoints, involve external service stubs, or exercise cross-module side effects. Single-endpoint tests with controlled DB belong in `backend-unit-tester`.

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

Integration tests prove that **multiple parts of the system work together correctly**. Reserve this layer for scenarios that cannot be covered by a single-endpoint unit test:

- Multi-step flows: create entity via one endpoint, verify side effects via another
- External service integration: stub third-party APIs and verify the full call chain
- Cross-app interactions: actions in one Django app triggering behavior in another
- Market-specific end-to-end API flows that combine several services

Patterns:

- Use `APITestCase` with `self.client` for endpoint tests
- Create DB fixtures with factories reflecting realistic cross-module state
- Assert full response cycle: status code, body structure, key field values
- For XML endpoints: parse response content with `ET.fromstring()` and use `find()` / `findall()`
- For paginated endpoints: assert `count`, `next`, `results` structure

Example (cross-app integration — advert creation triggers feed update):

```python
class TestAdvertFeedIntegration(APITestCase):
    def test_new_advert_appears_in_facebook_feed(self):
        # Step 1: create advert via API
        self.client.force_authenticate(user=self.seller)
        create_resp = self.client.post("/api/v2/adverts/", advert_data)
        self.assertEqual(create_resp.status_code, 201)
        # Step 2: verify it appears in the feed
        feed_resp = self.client.get("/api/v2/feeds/property/facebook/")
        root = ET.fromstring(feed_resp.content)
        listings = root.findall(".//listing")
        self.assertEqual(len(listings), 1)
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
