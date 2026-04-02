## Larixon Web Integration Test Infrastructure

### Scope

This role covers integration testing for the Larixon backend/web Django monolith. Integration tests exercise full request/response cycles through the Django test client with real database.

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

- Use `APITestCase` with `self.client` for endpoint tests
- Create minimal DB fixtures with factories
- Assert full response cycle: status code, body structure, key field values
- For XML endpoints: parse response content with `ET.fromstring()` and use `find()` / `findall()`
- For paginated endpoints: assert `count`, `next`, `results` structure
- Test with authenticated and unauthenticated users
- Test filter, ordering, and limit parameters

### Real project examples

From the Facebook feeds feature (CD-4651):

- `FacebookFeedXMLStructureTest`: tests XML validity, listing count, nested element structure
- `FacebookFeedListTest`: tests condition filters, limit_adverts, limit_description, 404 response
- Parametrized across feed types (property, auto)

### Reporting rule

Every integration result must include:

- endpoint(s) tested
- HTTP method(s)
- test run link or artifact path
- whether CI metadata or coverage URL is confirmed or still requires escalation
