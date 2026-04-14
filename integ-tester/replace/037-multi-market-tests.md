## Larixon Multi-Market Mobile Tests

Replace any platform-level assumptions about other market sets. Larixon mobile currently spans these real markets:

| Market | Android flavor | Locale | Currency | Mandatory notes |
| --- | --- | --- | --- | --- |
| Bazaraki | `bz` | `en-US` | `EUR` | `GDPR_GOOGLE` is enabled for this market |
| Somon | `tj` | `ru-RU` | `TJS` | Cyrillic content is common; dedicated Somon stands exist |
| Unegui | `mn` | `mn-MN` | `MNT` | `EMONGOLIA_ENABLED` is enabled for this market |
| Jacars | `ja` | `en-JM` | `JMD` | Car-market wording and Jamaica locale |
| Pin | `pn` | `en-TT` | `TTD` | Trinidad market |
| Salanto | `sl` | `en-US` | `EUR` | Verify payment-side currency at runtime if flow depends on it |

### Selection rule

- For any user-facing money, locale, or copy-sensitive UI, run at least three markets:
  - `bz`
  - `tj`
  - `mn`
- Add the most affected business market on top of that baseline if the task is market-specific.
- If the task changes a market-only flow, you must include the owning market even if it falls outside the default three-market baseline.
- Prefer native validation paths already available to the team first. Do not silently assume BrowserStack or another cloud-device lab unless the task or owner explicitly puts it in scope.

### Market-specific flows

- `bz`:
  - GDPR-related consent and Google privacy checks are market-specific
- `mn`:
  - eMongolia-related auth or toggles are market-specific
- `tj`, `ja`, `pn`:
  - validate localized labels, formatting, and navigation text instead of assuming one English fallback

### Market switching example

```kotlin
// MockWebServer market config
server.dispatcher = object : Dispatcher() {
    override fun dispatch(request: RecordedRequest) = when {
        request.path?.contains("/config") == true ->
            MockResponses.success("config_bz.json")
        else -> MockResponses.error(404)
    }
}
```

### Android execution

- Use real flavors from Gradle: `./gradlew connectedBzDebugAndroidTest` / `connectedTjDebugAndroidTest`
- If one platform is not in scope for the current sprint, say that clearly and avoid pretending the validation was cross-platform

### Reporting rule

Always state:

- tested market(s)
- platform
- locale-sensitive assertions
- market-specific branches that were intentionally skipped
