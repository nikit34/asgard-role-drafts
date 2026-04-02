# AW-3 ASGARD Role Drafts

This directory contains project-specific role additions and overrides prepared for Jira `AW-3`:

- `unit-tester`
- `integ-tester`

The files are organized to mirror the actions requested in the Jira task:

- `add/<slot>.md`
- `replace/<slot>.md`
- `extend/<slot>.md`

## Status

- ASGARD roles inspected on `2026-04-01`: `unit-tester` (Manifest v2, 15 slots), `integ-tester` (Manifest v3, 15 slots)
- Account permission: `viewer` — files need to be uploaded by a user with `write:roles`
- Bamboo plan keys confirmed: `AD-AT`, `AD-IN`, `ID-II`
- Allure TestOps confirmed as reporting system (not TestRail): projects 69 and 168
- Tests run on emulators/simulators — BrowserStack is out of scope
- Playwright is out of scope (separate web flow, not part of mobile tester skills)
- Tester roles write and validate locally; CI execution belongs to the merge/devops flow

## Scope covered in this package

### Unit Tester

| Slot | Action | Reason |
| --- | --- | --- |
| `013-test-architect-input.md` | add | Bridge to test-architect downstream guidance: scenario IDs, coverage targets, risk map, test data |
| `200-larixon-test-infra.md` | add | Larixon-specific ownership boundaries, local run references, fixture layout, Android+iOS notes |
| `011-test-style.md` | replace | Actual Larixon test style is mixed Android/KMP plus iOS XCTest/Quick, not one pure platform pattern |
| `020-coverage.md` | replace | Current Android repo uses Kover; legacy Jacoco wording exists in old docs |
| `031-repository-tests.md` | replace | Need Larixon examples and iOS-equivalent guidance |
| `032-usecase-tests.md` | replace | Need real Larixon domain examples, including review flow |
| `033-viewmodel-tests.md` | replace | Need Kotest/Turbine/dispatcher patterns from real code plus iOS equivalent |
| `034-mapper-tests.md` | replace | Need real DTO-to-domain examples, including paginated reviews |

### Integration Tester

| Slot | Action | Reason |
| --- | --- | --- |
| `013-test-architect-input.md` | add | Bridge to test-architect downstream guidance: scenario IDs, screen/flow assignments, MockWebServer setup |
| `037-multi-market-tests.md` | replace | Platform copy is not aligned with Larixon mobile markets |
| `200-larixon-test-devices.md` | add | Environments, devices, TestOps, emulator/simulator baseline |
| `036-accessibility-tests.md` | extend | Larixon-specific a11y constraints and mobile automation hints |

## Confirmed inputs used for the drafts

### Android

- Repo: `/Users/permi/classifieds-android-app`
- Current test stack in `CLAUDE.md` and Gradle:
  - `JUnit 5`
  - `Kotest`
  - `MockK`
  - `Turbine`
  - `Espresso`
  - `Allure`
  - `Kover`
- Real review-domain tests exist:
  - `feature-advert-review/.../RatingViewModelTest.kt`
  - `feature-advert-review/.../SubmitReviewUseCaseTest.kt`
  - `feature-advert-review/.../ReviewConfigMapperTest.kt`
  - `feature-advert-reviews/.../AdvertReviewsMapperTest.kt`
- Product flavors confirmed from Gradle:
  - `tj`
  - `mn`
  - `bz`
  - `ja`
  - `pn`
  - `sl`
- Market-specific flags confirmed:
  - `GDPR_GOOGLE` is enabled for `bz`
  - `EMONGOLIA_ENABLED` is enabled for `mn`
- Android UI tests already publish Allure metadata through annotations and `allure.properties`

### iOS

- Repo: `/Users/permi/classifieds-ios-app`
- Current unit/UI stack confirmed from `Podfile` and tests:
  - unit tests: `XCTestCase`, `Quick`, `Nimble`
  - UI tests: `XCTest`, `XCUIApplication`
  - Allure helpers in `LarixonUITests/Helpers/AllureXCTestExtensions.swift`
- Market targets confirmed from `AppCustomization.swift` files:
  - `Bazaraki`
  - `Somon.tj`
  - `Unegui.mn`
  - `Jacars`
  - `Pin.tt`
  - `Salanto`
- Locale and timezone values were taken from market `AppCustomization.swift` files, not guessed

### Shared QA infrastructure

- **Allure TestOps** (primary reporting, not TestRail):
  - Base URL: `https://larixon.testops.cloud`
  - Android project: `https://larixon.testops.cloud/project/69/launches`
  - iOS project: `https://larixon.testops.cloud/project/168/launches`
- **Bamboo** plan keys:
  - `AD-AT` — Android Automated Tests: `https://bamboo.dev.larixon.com/browse/AD-AT`
  - `AD-IN` — Android Integration: `https://bamboo.dev.larixon.com/browse/AD-IN`
  - `ID-II` — iOS Integration: `https://bamboo.dev.larixon.com/browse/ID-II`
- **Execution**: emulators (Android) and simulators (iOS) — BrowserStack out of scope
- **Playwright**: out of scope (separate web flow)
- Tester roles write and validate locally; CI execution belongs to the merge/devops flow

## Remaining steps

- Upload these files to ASGARD Role Composer as project additions/replacements (requires `write:roles`)
- Verify assembled prompts via "Preview Assembled" in HEIMDALL after upload

## Sources

### Jira

- `AW-3`: `https://larixon.atlassian.net/browse/AW-3`

### Confluence

- Mobile QA process: `https://larixon.atlassian.net/wiki/spaces/It1/pages/3531276289`
- BrowserStack choice: `https://larixon.atlassian.net/wiki/spaces/It1/pages/3493298191`
- Device matrix example: `https://larixon.atlassian.net/wiki/spaces/It1/pages/4071096331`

### Android code

- `/Users/permi/classifieds-android-app/CLAUDE.md`
- `/Users/permi/classifieds-android-app/gradle/libs.versions.toml`
- `/Users/permi/classifieds-android-app/app/build.gradle`
- `/Users/permi/classifieds-android-app/app/src/androidTest/resources/allure.properties`
- `/Users/permi/classifieds-android-app/feature-advert-review/src/test/java/com/larixon/advert/review/presentation/screen/rating/RatingViewModelTest.kt`
- `/Users/permi/classifieds-android-app/feature-advert-review/src/test/java/com/larixon/advert/review/domain/usecase/SubmitReviewUseCaseTest.kt`
- `/Users/permi/classifieds-android-app/feature-advert-review/src/test/java/com/larixon/advert/review/data/mapper/ReviewConfigMapperTest.kt`
- `/Users/permi/classifieds-android-app/feature-advert-reviews/data/src/commonTest/kotlin/com/larixon/advertreviews/data/remote/mapper/AdvertReviewsMapperTest.kt`
- `/Users/permi/classifieds-android-app/app/src/androidTest/kotlin/tj/somon/somontj/test/e2e/PinAuthorizationTest.kt`

### iOS code

- `/Users/permi/classifieds-ios-app/Podfile`
- `/Users/permi/classifieds-ios-app/LarixonTests/LocalizationSpec.swift`
- `/Users/permi/classifieds-ios-app/LarixonTests/API/APIAuthorizationTest.swift`
- `/Users/permi/classifieds-ios-app/LarixonUITests/Fixtures/BaseUITestCase.swift`
- `/Users/permi/classifieds-ios-app/LarixonUITests/Helpers/AllureXCTestExtensions.swift`
- `/Users/permi/classifieds-ios-app/LarixonUITests/Tests/Navigation/TabBarTests.swift`
- `/Users/permi/classifieds-ios-app/Larixon/Bazaraki/AppCustomization.swift`
- `/Users/permi/classifieds-ios-app/Larixon/Somon.tj/AppCustomization.swift`
- `/Users/permi/classifieds-ios-app/Larixon/Unegui.mn/AppCustomization.swift`
- `/Users/permi/classifieds-ios-app/Larixon/Jacars/AppCustomization.swift`
- `/Users/permi/classifieds-ios-app/Larixon/Pin.tt/AppCustomization.swift`
- `/Users/permi/classifieds-ios-app/Larixon/Salanto/AppCustomization.swift`
