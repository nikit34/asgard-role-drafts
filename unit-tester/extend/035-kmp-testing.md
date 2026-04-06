## KMP Testing Additions

### Where KMP tests live

- Shared logic: `commonTest` source set (e.g. `feature-*/data/src/commonTest/kotlin/...`)
- Platform-specific: `androidTest` or `iosTest` when the implementation uses platform APIs
- If a module has only `commonMain`, tests go in `commonTest`

### Running KMP tests

- All common tests: `./gradlew :feature-module:allTests`
- Common only: `./gradlew :feature-module:jvmTest` (runs commonTest on JVM target)
- iOS targets: `./gradlew :feature-module:iosSimulatorArm64Test` (requires simulator)

### What belongs in commonTest vs platform test

| commonTest | Platform test |
|-----------|--------------|
| Mappers, DTOs, domain models | Platform-specific serialization (NSCoding, Parcelable) |
| Use cases with interface dependencies | Implementations that call platform SDK |
| Business rules, validators, formatters | Keychain, SharedPreferences wrappers |

### expect/actual in tests

- Mock the `expect` interface in commonTest using fakes, not platform mocks
- If MockK is only available on JVM, use manual fakes in commonTest and MockK in androidTest
- Do not add `mockk` to commonTest dependencies — it will fail on iOS targets

### Naming

- Keep the same naming convention as Android tests (DescribeSpec or JUnit depending on module)
- Suffix platform-specific test files with the target: `TokenStorageTest.android.kt`, `TokenStorageTest.ios.kt`
