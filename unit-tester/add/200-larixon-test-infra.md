## Larixon Test Infrastructure

### Scope

This role covers Larixon mobile work across Android/KMP and iOS. Always name the platform you are talking about. Do not answer with Android-only assumptions when the task is cross-platform.

### CI and ownership

- Android mobile builds are published through Bamboo. The accessible process docs confirm Bamboo usage and a known mobile build entry point `LA-BAZ`.
- Confirmed Bamboo plan pages currently referenced for this workflow:
  - `https://bamboo.dev.larixon.com/browse/AD-AT`
  - `https://bamboo.dev.larixon.com/browse/AD-IN`
  - `https://bamboo.dev.larixon.com/browse/ID-II`
- Tester-role ownership stops before direct CI orchestration. Do not tell the user that the unit-tester role should manually trigger Bamboo unless the task explicitly asks for CI handoff details.
- If CI metadata is requested, report only confirmed facts such as plan names or artifact labels visible in the task, repo, or attached docs. Do not invent hidden plan keys.
- iOS CI in the accessible sources is not symmetrical with Android. Current process docs say iOS builds are often assembled manually, while the repo itself contains Fastlane configuration. State the actual evidence you found.

### Local execution

- Treat these as local validation references, not as mandatory steps for every answer.
- Android all debug unit tests: `./gradlew testDebugUnitTest`
- Android flavor-specific unit tests: `./gradlew testTjDebugUnitTest`
- Android module-specific example: `./gradlew :feature-advert-review:test`
- iOS unit tests example: `xcodebuild test -workspace Larixon.xcworkspace -scheme Bazaraki -destination 'platform=iOS Simulator,name=iPhone 14'`

### Coverage artifacts

- Treat the current repo as the source of truth. In Larixon Android, the active Gradle plugin is `org.jetbrains.kotlinx.kover`, even if older docs still use the word `Jacoco`.
- When you mention coverage, verify the actual report path before quoting it. The expected Android local output is the module `build/reports/kover/html/index.html`.
- If Bamboo publishes a differently named artifact, report both the user-facing CI name and the actual tool behind it, but keep CI execution outside the tester-role responsibility.
- iOS coverage publication was not confirmed in accessible sources. Do not fabricate a URL or a gate if you do not have one.

### Test fixture layout

- Android application unit tests: `app/src/test`
- Android feature/common tests: `feature-*/src/commonTest`
- Android UI fixtures and device tests: `app/src/androidTest`
- iOS unit tests: `LarixonTests`
- iOS UI fixtures: `LarixonUITests/Fixtures`

### Real project references

- Android review ViewModel tests:
  - `feature-advert-review/src/test/java/com/larixon/advert/review/presentation/screen/rating/RatingViewModelTest.kt`
- Android review use case tests:
  - `feature-advert-review/src/test/java/com/larixon/advert/review/domain/usecase/SubmitReviewUseCaseTest.kt`
- Android mapper tests:
  - `feature-advert-review/src/test/java/com/larixon/advert/review/data/mapper/ReviewConfigMapperTest.kt`
  - `feature-advert-reviews/data/src/commonTest/kotlin/com/larixon/advertreviews/data/remote/mapper/AdvertReviewsMapperTest.kt`
- iOS Quick example:
  - `LarixonTests/LocalizationSpec.swift`
- iOS XCTest example:
  - `LarixonTests/API/APIAuthorizationTest.swift`

### Reporting rule

Always return:

- platform
- repo path
- exact local command(s) when local validation is relevant
- exact test file paths used as examples
- whether CI metadata or coverage URL is confirmed or still requires escalation
