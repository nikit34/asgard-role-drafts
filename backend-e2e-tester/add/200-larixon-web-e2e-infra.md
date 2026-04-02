## Larixon Web E2E Test Infrastructure

### Scope

This role covers browser-level E2E testing for the Larixon web platform using Playwright. Tests verify complete user flows in a real browser against a running stand.

- Repository: `https://gitlab.dev.larixon.com/larixon-classifieds/web/core`
- Local path convention: `~/Core`
- Feature branch convention: branch name matches Jira ticket number (e.g. `CD-4651`)
- Existing E2E tests: `functional_tests/` directory (pytest-based)
- Playwright tests: being introduced alongside existing functional tests

### CI and ownership

- CI runs through GitLab CI pipelines.
- E2E tests run as part of the CI pipeline. The tester role writes and validates tests locally; CI execution belongs to the automated merge/devops flow.
- Do not instruct the user to manually trigger CI unless the task explicitly asks for it.

### Environments

E2E tests execute against shared stands. Always name the exact stand:

- Shared stands: `stand1.dev.larixon.com` through `stand13.dev.larixon.com`
- Market-specific: `bazaraki-master.dev.larixon.com`, `somon-master.dev.larixon.com`, etc.

### Playwright setup

- Framework: Playwright for Python (`playwright` + `pytest-playwright`)
- Browser: Chromium by default, Firefox and WebKit for cross-browser validation
- Base URL: configured per-stand (e.g. `https://bazaraki-master.dev.larixon.com`)

### Local execution

- Install Playwright browsers: `playwright install`
- Run all E2E tests: `python -m pytest functional_tests/`
- Run Playwright tests: `python -m pytest functional_tests/ -k playwright`
- Run with headed browser: `python -m pytest functional_tests/ --headed`
- Run specific test: `python -m pytest functional_tests/test_feeds.py::test_name`
- Run against specific stand: `BASE_URL=https://stand5.dev.larixon.com python -m pytest functional_tests/`

### tester-skills-mcp integration

The team uses `tester-skills-mcp` for E2E workflow:

- **Coverage scan**: `action_e2e_functional_scan` — scans `functional_tests/`, matches against TD, identifies gaps
- **Playwright research**: `action_playwright_research` — smoke-check, locator collection, snapshot analysis
- **E2E implementation**: `build_playwright_e2e_implement_prompt` — implements Playwright tests from TD
- **Functional implementation**: `build_e2e_implement_prompt` — implements pytest E2E tests from TD

Workflow:
1. Run `action_e2e_functional_scan` to identify existing coverage and new scenarios
2. Run `action_playwright_research` for UI context and locator discovery
3. Use `build_playwright_e2e_implement_prompt` for implementation

### Test management and reporting

- **Allure TestOps** is the primary reporting system (not TestRail):
  - Base URL: `https://larixon.testops.cloud`
  - Web project: `https://larixon.testops.cloud/project/1/launches`
- TD cases are synced to Allure via `td-allure-sync` skill
- Do not route through TestRail.

### Multi-market E2E

For user-facing flows affected by locale, currency, or market-specific features:

- Test at minimum: Bazaraki (EN/EUR), Somon (RU/TJS), Unegui (MN/MNT)
- Switch market by changing the base URL to the market-specific stand
- Verify Cyrillic content rendering on Somon stand
- Verify currency display on market-appropriate stands

### Reporting rule

Every E2E result must include:

- stand URL tested against
- browser(s) used
- market(s) tested
- test run link or artifact path
- flaky risks identified
- whether CI metadata is confirmed or still requires escalation
