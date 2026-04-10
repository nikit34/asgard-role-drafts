# Jira Task Draft

## Title

ASGARD: подготовить project-specific role drafts для backend/frontend test flow без DevOps и manual checks

## Description

По аналогии с `AW-3` нужно подготовить пакет project-specific additions/replacements для ASGARD-ролей, которые участвуют только в тестовом контуре web/backend+frontend разработки.

В scope входят роли, связанные с проектированием и написанием автотестов после реализации со стороны backend/frontend. Роли DevOps и ручные проверки в эту задачу не входят и не должны описываться как обязательная часть тестового workflow.

Ориентир по потоку:

`Backend Developer` + `Frontend Developer` -> `backend-test-architect` -> `backend-unit-tester` + `frontend-unit-tester` + `backend-integ-tester` + `backend-e2e-tester`

## Scope

Подготовить role drafts для тестовых ролей backend/frontend-контура:

- `backend-test-architect`
- `backend-unit-tester`
- `frontend-unit-tester`
- `backend-integ-tester`
- `backend-e2e-tester`

Если фактические имена ролей в ASGARD/HEIMDALL отличаются, нужно подобрать ближайшие эквиваленты и явно зафиксировать это в README пакета.

## Out Of Scope

- `devops` / запуск тестов как отдельная роль
- ручные проверки
- скриншоты для manual QA
- доработка developer-ролей, не связанных напрямую с тестовым контуром
- бизнес-реализация фичи в backend/frontend-репозиториях

## What To Prepare

Нужно собрать пакет в стиле `AW-3`:

- project additions/replacements/extensions по слотам нужных ролей
- README с перечнем ролей, изменённых слотов, причин изменений и источников
- подтверждённые project-specific правила по backend/frontend test stack
- явные границы ответственности между unit, integration, e2e и test-architect

## Expected Role Responsibilities

### Backend Test Architect (`backend-test-architect`)

- строит test strategy, test matrix, risk map, test data strategy
- разносит сценарии по уровням: Unit BE, Unit FE, Integration, E2E
- не включает DevOps handoff и manual QA steps как обязательную часть workflow
- все test cases имеют тип `auto` — manual test cases out of scope

### Backend Unit Tester (`backend-unit-tester`)

- покрывает backend business logic, services, serializers/mappers, repositories/querysets, views/viewsets (single endpoint, controlled DB)
- использует только backend test stack и локальные способы запуска

### Frontend Unit Tester (`frontend-unit-tester`)

- покрывает frontend business logic, hooks/stores, components/viewmodels, formatters/mappers
- не уходит в e2e/integration scope, если это не предусмотрено strategy

### Backend Integration Tester (`backend-integ-tester`)

- проверяет cross-app и multi-service flows через Django test client
- сценарии с несколькими endpoints, external service stubs, cross-module side effects
- не подменяет собой unit и e2e слои

### Backend E2E Tester (`backend-e2e-tester`)

- покрывает пользовательские бизнес-флоу end-to-end
- использует только automated e2e scope
- не включает ручную валидацию как обязательный deliverable

## Acceptance Criteria

- Подготовлен пакет role drafts по аналогии с `AW-3` только для тестовых ролей backend/frontend-контура.
- В пакете нет требований к `devops` роли и нет шагов с ручными проверками.
- Разделение ответственности между `backend-unit-tester`, `frontend-unit-tester`, `backend-integ-tester`, `backend-e2e-tester`, `backend-test-architect` описано явно и без пересечений.
- README содержит список ролей, слотов, причин изменений и список использованных источников.
- Все спорные места по именам ролей или отсутствующим слотам зафиксированы отдельно, без скрытых допущений.

## Suggested Deliverables

- Папка с draft-файлами для ролей
- README по пакету
- Короткая сводка по найденным role manifest differences

## Notes

- За основу взять структуру и уровень детализации `AW-3`.
- Если в текущих ролях уже есть упоминания DevOps/manual QA, их нужно либо исключить из project overrides, либо явно пометить как out of scope для этой задачи.
- Фокус задачи: только тестовые роли для backend/frontend продукта.
