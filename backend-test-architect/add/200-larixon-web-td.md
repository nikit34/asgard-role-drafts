## Larixon Web Test Design Infrastructure

### Scope

- Repository: `https://gitlab.dev.larixon.com/larixon-classifieds/web/core` (local: `~/Core`)
- Feature branch = Jira ticket number (e.g. `CD-4651`). Always analyze the feature branch, not master. If not specified, ask.

### Test layers → downstream routing

| Layer | Covers | Downstream role |
| --- | --- | --- |
| Unit BE | service/serializer/model methods — no HTTP, no DB | `backend-unit-tester` |
| Unit FE | React component/hook/store/mapper in isolation | `frontend-unit-tester` |
| Integration | single-endpoint through Django test client with real DB | `backend-integ-tester` |
| E2E UI | browser-level Playwright against a running stand | `e2e-tester` |

Assign each scenario to exactly one layer. Do not duplicate assertions across layers.

### Prioritization

| Priority | Meaning |
| --- | --- |
| P1 | Critical — blocks business or core user flow |
| P2 | Important — regression risk, significant secondary flow |
| P3 | Supplementary — edge case, cosmetic, nice-to-have |

### Markets

Include market-specific rows in the test matrix when the feature touches locale, currency, or user-facing content:

| Market | Locale | Currency | Notes |
| --- | --- | --- | --- |
| Bazaraki | EN | EUR | GDPR/Google privacy |
| Somon | RU | TJS | Cyrillic content |
| Unegui | MN | MNT | eMongolia integration |
| Jacars | en_JM | JMD | Car-market specific |
| Pin | en_TT | TTD | Trinidad |
| Salanto | EN | EUR | Verify payment currency |

### Output artifacts

Produce under `production-documentation/task-{TASK_KEY}/` or as a Confluence page in [TDs 2026](https://larixon.atlassian.net/wiki/spaces/itdep/folder/4091674628):

- `test-strategy.md` — scope, boundaries, level assignments. Include `frontend-unit-tester` when FE code is changed.
- `test-matrix.md` — scenario IDs (`UT-*` / `UTF-*` / `IT-*` / `E2E-*`), priorities, layer, assigned downstream role.
- `risk-map.md` — HIGH/MEDIUM/LOW per area.
- `test-data-strategy.md` — fixture shapes, API response examples, error codes.
- `coverage-targets.md` — per-module targets with justification.

### TD format (required for `tester-skills-mcp` parsing)

Each test case row must have:
- `[x]`/`[ ]` checkbox, unique ID (e.g. 1.1, 2.3), `Name` column
- `Реализован:` — leave empty (filled by downstream tester after implementation)
- Задача / Предусловия / Действие / Ожидаемый результат
- Priority, Layer, Type: `auto`

TD synced to Allure TestOps web project (`project/1`) via `td-allure-sync`. Publication and generation happen in separate chats (generation consumes most context).
