# MOB-617 Pilot Brief

Use this task as the first proof-of-use for the `AW-3` role drafts.

## Task

- Jira: `https://larixon.atlassian.net/browse/MOB-617`
- Summary: `New "Reviews" item on the Profile screen`

## Why this is a good pilot

`MOB-617` touches several things that the updated roles must reason about well:

- profile navigation
- badge counters
- pending vs completed review states
- paginated review lists
- review submission status transition
- deep link behavior
- click-through back to advert context
- localized multi-market UI

It also maps well to existing Android review-domain code and tests, which makes it useful for both gap analysis and realistic prompt output.

## Useful existing code to inspect first

### Android review domain

- `feature-advert-review/src/test/java/com/larixon/advert/review/presentation/screen/rating/RatingViewModelTest.kt`
- `feature-advert-review/src/test/java/com/larixon/advert/review/domain/usecase/SubmitReviewUseCaseTest.kt`
- `feature-advert-review/src/test/java/com/larixon/advert/review/data/mapper/ReviewConfigMapperTest.kt`
- `feature-advert-reviews/data/src/commonTest/kotlin/com/larixon/advertreviews/data/remote/mapper/AdvertReviewsMapperTest.kt`

### Android UI automation style

- `app/src/androidTest/kotlin/tj/somon/somontj/test/e2e/PinAuthorizationTest.kt`

### iOS automation style

- `LarixonUITests/Tests/Navigation/TabBarTests.swift`
- `LarixonUITests/Fixtures/BaseUITestCase.swift`

## What Unit Tester should produce on MOB-617

The role should inspect the existing review-related code and return:

- a gap list of missing or outdated unit coverage
- concrete test candidates for:
  - badge count derivation for the new profile row
  - pending vs completed filter model/state
  - moving a review from pending to completed after submit
  - no-deal branch
  - hide branch
  - deeplink parsing for `lc://profile/reviews`
  - mapper coverage for paginated review-list payloads
  - click-through model data required to open advert context

### Recommended layer mapping

- UseCase:
  - submit review
  - mark no-deal
  - hide review
- ViewModel or presentation state:
  - filter tab state
  - loading, success, error, empty state
  - tab move after successful review submission
- Mapper:
  - paginated list payload
  - review card fields
  - optional tags/comment

## What Integration Tester should produce on MOB-617

The role should return a scenario set for:

- open `Reviews` from Profile
- badge counter visible when pending reviews exist
- badge counter hidden when pending reviews count is zero
- pending card:
  - tap stars opens review flow
  - tap `No deal` updates state correctly
  - tap `Hide` removes or hides the card as specified
- completed card:
  - rating is shown
  - tags are shown
  - text comment is shown when present
  - delete action behavior is validated
- after successful review submission, the item moves from pending to completed
- advert title/context is clickable and opens advert context
- deep link `lc://profile/reviews`
- pagination
- empty state
- multi-market smoke for at least `bz`, `tj`, `mn`

## Ambiguities to surface early

The Jira description still contains change markers and open questions. The pilot should explicitly flag them before test design is considered complete:

- whether the `All` tab is removed
- final behavior of review deletion
- final source of tab counters
- whether iOS implementation is in the same delivery scope
- exact backend contract for hide/no-deal actions
- whether this pilot is scenario-design-only or should also describe local native validation
- whether any part of this delivery expands into web or hybrid behavior; otherwise keep Playwright out of scope

## Suggested Unit Tester prompt seed

```text
You are validating Jira MOB-617 for Larixon mobile. Use the Larixon unit-test role rules from AW-3. Inspect existing review-related code and tests first, especially feature-advert-review and feature-advert-reviews. Return:
1. current unit-test coverage that already exists,
2. missing unit-test scenarios by layer (use case, view model/presentation, mapper),
3. exact file targets for new tests,
4. any spec ambiguities that block reliable unit-test design.
Do not invent Bamboo plan keys or backend contracts. Do not prescribe direct Bamboo execution from the tester role. Keep BrowserStack and Playwright out of the baseline output unless the task explicitly requires them. If something is not confirmed in code or task text, call it out explicitly.
```

## Suggested Integration Tester prompt seed

```text
You are validating Jira MOB-617 for Larixon mobile. Use the Larixon integration-test role rules from AW-3. Build a test scenario set for the new Reviews entry in Profile and the Reviews list screen. Cover profile entry, counters, pending/completed states, review submission, no-deal, hide, deletion, pagination, empty state, advert click-through, and deep link lc://profile/reviews. Include a multi-market pass for bz, tj, and mn. Do not assume RTL, BrowserStack as the default runner, or any Playwright/web flow. Do not prescribe direct Bamboo execution from the tester role. If the task text is ambiguous, list the ambiguity before finalizing the scenario set.
```
