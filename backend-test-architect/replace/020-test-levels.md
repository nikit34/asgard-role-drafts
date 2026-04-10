## Test Levels for Larixon Web

### Level 1: Unit Tests — Backend (backend-unit-tester)

**Scope**: Pure Python — no HTTP, no database, no Django ORM calls.

**What to test:**
- Service layer methods (`apps/{feature}/services.py`) — mock ORM querysets
- Serializer validation logic — instantiate serializers directly, call `.is_valid()`
- Model methods and properties (custom logic only, not field definitions)
- Utility / helper functions, management commands (pure logic)

**Tooling:** `pytest` + `pytest-mock` / `unittest.mock`

---

### Level 1b: Unit Tests — Frontend (frontend-unit-tester)

**Scope**: React/Next.js components, hooks, stores in isolation.

**What to test:**
- Component rendering and user interaction (React Testing Library)
- Custom hooks with renderHook
- Store/state management logic
- Formatters, mappers, utility functions

**Tooling:** Jest or Vitest + React Testing Library

---

### Level 2: Integration Tests (backend-integ-tester)

**Scope**: Full Django stack — HTTP → View → Serializer → Service → ORM → real test DB.

**What to test:**
- All API endpoints defined in `api-contract.md`
- Request/response format compliance (status codes, field names, types)
- Authentication and permission enforcement
- Database state after mutations
- Pagination, filtering, ordering behavior

**NOT in scope:**
- Multi-step business flows (that's E2E)

**Tooling:** `pytest-django` + DRF `APIClient` + `model_bakery`

---

### Level 3: E2E Tests (e2e-tester)

**Scope**: Full multi-step flows through the browser via Playwright or through the API.

**What to test:**
- Complete user journeys (register → create → update → delete → verify)
- Flows spanning multiple endpoints with consistent state
- Permission boundary flows (user A vs user B)
- Cross-market flows when applicable

**NOT in scope:**
- Single-endpoint behavior (covered by integration tests)
- Internal service method logic (covered by unit tests)

**Tooling:** Playwright (`pytest-playwright`) or `pytest-django` + DRF `APIClient`
