## Test Architect Upstream Input

After completing Step Zero and before planning your test suite (Phase 2), check whether a Test Architect has already produced a test strategy for this feature.

### Discovery

```
1. From the Jira task, identify the parent story key ({PARENT_KEY})
2. Search for test-architect artifacts:
   find production-documentation/ -path "*/test-matrix.md" | head -5
3. The artifacts live under one of:
   - production-documentation/task-{TA_TASK_KEY}/   (test-architect subtask key)
   - production-documentation/{PARENT_KEY}-*/        (parent story folder)
4. If found, read ALL of:
   - test-strategy.md   — scope, boundaries, test levels
   - test-matrix.md     — scenario IDs (T-001..T-NNN), priorities, assignments
   - risk-map.md        — HIGH/MEDIUM/LOW per area
   - test-data-strategy.md — test data, user accounts, preconditions
   - coverage-targets.md   — flow coverage targets
```

### What to use

When test-architect artifacts exist, they override your own planning:

| Aspect | Without test-architect | With test-architect |
|--------|----------------------|---------------------|
| Which user flows to test | You decide from requirements | Use "E2E Tester Guidance" section from test-strategy.md |
| Scenario list | You derive from UX spec | Use assigned scenario IDs from test-matrix.md (your rows have `Test Level = E2E`) |
| Risk-based depth | You assess yourself | Use risk levels from risk-map.md — HIGH = full flow + edge cases, MEDIUM = happy path + main error, LOW = smoke only |
| Test data | You create yourself | Use precondition definitions from test-data-strategy.md |
| Multi-market matrix | You identify from UI | Use market-specific rows from test-matrix.md |
| What NOT to test | Implicit | Use explicit "out of scope for E2E Tester" boundaries from test-strategy.md |

### Traceability

When following test-architect guidance, include scenario IDs in test names:

```python
# T-101: User can filter adverts by category and see correct results
def test_filter_adverts_by_category(page):
    ...
```

Reference the scenario IDs in your Jira completion comment so test-architect can verify coverage.

### Conflicts

If test-architect guidance conflicts with what you see in the UI (e.g. a flow listed for testing does not exist, or the page structure differs):

1. Follow what the actual UI has — do not test phantom flows
2. Post a `/tracker-comment` noting the discrepancy with the specific T-xxx ID
3. Continue with the rest of the plan

### Artifacts not found

If no test-architect artifacts exist for this feature, follow the blocking protocol defined in `012-step-zero.md`: BLOCK via /tracker-blocked mentioning Test Architect. Do not proceed without upstream test strategy.
