## Larixon Downstream Guidance Extension

### Frontend Unit Tester Guidance Template

```markdown
## Frontend Unit Tester — Guidance for Task {TASK_KEY}

### Scope
Test the following frontend modules at the unit level:

| Module | Components/Hooks to Test | Priority |
|--------|-------------------------|----------|
| `src/components/{feature}/` | `{ComponentName}` rendering, user interaction | P1 |
| `src/hooks/{feature}/` | `use{HookName}` state transitions | P1 |
| `src/stores/{feature}/` | `{storeName}` actions and selectors | P2 |

### What to Mock
- API calls via MSW or jest.mock
- Router/navigation context
- Auth context (logged-in user, anonymous)

### Specific Scenarios (from test matrix)
{paste UTF-* rows from test-matrix.md}

### Coverage Target
- Components with business logic: 80%
- Custom hooks: 90%
- Store/state logic: 85%
```
