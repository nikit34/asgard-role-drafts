## Larixon Unit Test Style

### Default rule

Follow the dominant style of the touched module. New test files must use Kotest DescribeSpec.

### Android and KMP

- **New files**: always Kotest `DescribeSpec` with sentence-style `it("...")`.
- **Existing Kotest files**: extend in the same style.
- **Existing JUnit files**: when adding tests to an existing JUnit file, migrate the file to Kotest DescribeSpec. Do not extend JUnit — the platform mandates Kotest only.
- Common tools in the current Android repo:
  - `Kotest`
  - `MockK`
  - `Turbine`
- Use `action_condition_expectedResult` naming only if migrating from JUnit is explicitly out of scope for the task.

Minimal Kotest example (dominant style in feature modules):

```kotlin
class ReviewConfigMapperTest : DescribeSpec({
    describe("map") {
        it("maps rating to domain model") {
            val result = mapper.map(dto)
            result.rating shouldBe 4
        }
        it("returns null when rating is absent") {
            val result = mapper.map(dtoWithoutRating)
            result.rating shouldBe null
        }
    }
})
```
- Assert behavior, state, and important collaboration. Do not assert implementation noise.
- When coroutines or flows are involved, use the test dispatcher and `Turbine` instead of sleeps.

### Assertions and doubles

- `shouldBe`
- `shouldBeInstanceOf`
- `coEvery`
- `coVerify` — never as the sole assertion in a test; always verify return values or state alongside

### Good Larixon examples

- Android ViewModel style:
  - `feature-advert-review/.../RatingViewModelTest.kt`
- Android use case style:
  - `feature-advert-review/.../SubmitReviewUseCaseTest.kt`
- Android mapper style:
  - `feature-advert-review/.../ReviewConfigMapperTest.kt`

### Mandatory writing rules

- One test = one behavior or one closely related branch
- Use real domain names from the task, not placeholder entities
- Prefer stable factories/builders over giant inline object graphs
- Keep comments rare and only for non-obvious setup
- If a module has legacy JUnit tests, migrate to Kotest when touching the file rather than extending the old style

### Do not

- Mix assertion vocabularies in the same file
- Rewrite the entire file structure when adding a single regression test
- Use sleeps where the repo already has dispatcher- or state-based synchronization
