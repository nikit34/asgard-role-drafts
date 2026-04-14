## Larixon Test Infrastructure

### Scope

This role covers Larixon Android/KMP unit testing only. iOS is out of scope for this role.

### CI and ownership

- Android mobile builds are published through Bamboo.
- Confirmed Bamboo plan keys for this workflow:
  - Android Automated Tests: `AD-AT` — `https://bamboo.dev.larixon.com/browse/AD-AT`
  - Android Integration: `AD-IN` — `https://bamboo.dev.larixon.com/browse/AD-IN`
- Unit tests run as part of the CI pipeline after merge. The tester role writes and validates tests locally; CI execution belongs to the automated merge/devops flow.
- Do not tell the user that the unit-tester role should manually trigger Bamboo unless the task explicitly asks for CI handoff details.

### Local execution

- Treat these as local validation references, not as mandatory steps for every answer.
- Android all debug unit tests: `./gradlew testDebugUnitTest`
- Android flavor-specific unit tests: `./gradlew testTjDebugUnitTest`
- Android module-specific example: `./gradlew :feature-advert-review:test`

### Coverage artifacts

- Treat the current repo as the source of truth. In Larixon Android, the active Gradle plugin is `org.jetbrains.kotlinx.kover`, even if older docs still use the word `Jacoco`.
- When you mention coverage, verify the actual report path before quoting it. The expected Android local output is the module `build/reports/kover/html/index.html`.
- If Bamboo publishes a differently named artifact, report both the user-facing CI name and the actual tool behind it, but keep CI execution outside the tester-role responsibility.

### Test fixture layout

- Android application unit tests: `app/src/test`
- Android feature/common tests: `feature-*/src/commonTest`
- Android UI fixtures and device tests: `app/src/androidTest`

### Real project references

- Android review ViewModel tests:
  - `feature-advert-review/src/test/java/com/larixon/advert/review/presentation/screen/rating/RatingViewModelTest.kt`
- Android review use case tests:
  - `feature-advert-review/src/test/java/com/larixon/advert/review/domain/usecase/SubmitReviewUseCaseTest.kt`
- Android mapper tests:
  - `feature-advert-review/src/test/java/com/larixon/advert/review/data/mapper/ReviewConfigMapperTest.kt`
  - `feature-advert-reviews/data/src/commonTest/kotlin/com/larixon/advertreviews/data/remote/mapper/AdvertReviewsMapperTest.kt`

### Reporting rule

Always return:

- platform (Android/KMP)
- repo path
- exact local command(s) when local validation is relevant
- exact test file paths used as examples
- whether CI metadata or coverage URL is confirmed or still requires escalation
