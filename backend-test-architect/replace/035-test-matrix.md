## Test Matrix Design

### Matrix Structure

Each scenario entry in `test-matrix.md` must have:

| Field | Description |
|-------|-------------|
| `ID` | Unique identifier: `UT-001`, `UTF-001`, `IT-001`, `E2E-001` |
| `Level` | `unit-be` / `unit-fe` / `integration` / `e2e` |
| `Area` | Module, endpoint, component, or flow being tested |
| `Scenario` | Human-readable description of what is verified |
| `Priority` | `P1` (must pass before release) / `P2` (should pass) / `P3` (nice to have) |
| `Test Data` | Input fixture or factory reference |
| `Expected Result` | Observable outcome |
| `Assigned To` | `backend-unit-tester` / `frontend-unit-tester` / `backend-integ-tester` / `e2e-tester` |

### ID Prefix Convention

| Prefix | Level | Downstream role |
|--------|-------|-----------------|
| `UT-` | unit-be | backend-unit-tester |
| `UTF-` | unit-fe | frontend-unit-tester |
| `IT-` | integration | backend-integ-tester |
| `E2E-` | e2e | e2e-tester |

### Priority Rules

- **P1**: All auth/permission checks, all data mutation outcomes, all error codes in api-contract.md, core business logic
- **P2**: Edge cases, optional fields, pagination boundaries, filter combinations, multi-market variants
- **P3**: Corner cases with very low probability, cosmetic consistency

### Coverage Rules

- Every endpoint in `api-contract.md` → minimum one IT-* scenario
- Every HIGH-risk area → minimum one E2E-* scenario
- Every service method → minimum one UT-* scenario
- Every user-facing component with business logic → minimum one UTF-* scenario
- No scenario IDs may overlap across levels

### Реализован column

The `Реализован` column (test class and method name) is filled by the downstream tester after implementation — leave empty in the TD.
