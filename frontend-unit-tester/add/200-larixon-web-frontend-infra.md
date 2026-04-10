## Larixon Web Frontend Test Infrastructure

### Scope

This role covers frontend unit testing for the Larixon web platform. The frontend uses React / Next.js.

- Repository: `https://gitlab.dev.larixon.com/larixon-classifieds/web/core`
- Local path convention: `~/Core`
- Feature branch convention: branch name matches Jira ticket number (e.g. `CD-4651`)

### CI and ownership

- CI runs through GitLab CI pipelines.
- Frontend unit tests run as part of the CI pipeline after merge. The tester role writes and validates tests locally; CI execution belongs to the automated merge/devops flow.
- Do not instruct the user to manually trigger CI unless the task explicitly asks for CI handoff details.

### Local execution

- Treat these as local validation references, not as mandatory steps for every answer.
- Run all frontend tests: `npm test` or `yarn test` (follow what the repo uses)
- Run tests for a specific module: `npm test -- --testPathPattern=components/Filter`
- Run a specific test file: `npm test -- path/to/Component.test.tsx`
- Run with coverage: `npm test -- --coverage`
- Watch mode: `npm test -- --watch`

### Coverage artifacts

- Coverage is measured with the test runner's built-in coverage (Jest or Vitest with c8/istanbul).
- Expected local report: `coverage/lcov-report/index.html`
- If CI publishes a differently named artifact, report both the user-facing CI name and the actual tool behind it.

### Test fixture layout

- Component tests: co-located with the component file as `ComponentName.test.tsx` or in a `__tests__/` directory — follow the existing repo convention
- Hook tests: co-located with the hook file or in `__tests__/`
- Test utilities and shared mocks: `src/test-utils/` or `src/__mocks__/` — follow what the repo uses
- MSW handlers (if used): `src/mocks/handlers.ts`

### Test stack

- Test runner: Jest or Vitest — follow what the repo uses
- Component testing: React Testing Library (`@testing-library/react`)
- User interaction: `@testing-library/user-event`
- API mocking: MSW (`msw`) for network-level mocking, or Jest module mocks for simpler cases
- Hook testing: `@testing-library/react` `renderHook`
- Assertions: Jest matchers + `@testing-library/jest-dom`

### Reporting

- **Allure TestOps** is the primary reporting system (not TestRail):
  - Base URL: `https://larixon.testops.cloud`
  - Web project: `https://larixon.testops.cloud/project/1/launches`
- Do not route this workflow through TestRail.

### Reporting rule

Always return:

- repo path
- exact local command(s) when local validation is relevant
- exact test file paths used as examples
- whether CI metadata or coverage URL is confirmed or still requires escalation
