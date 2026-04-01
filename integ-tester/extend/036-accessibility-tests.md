## Larixon Accessibility Additions

Add these Larixon-specific rules on top of the platform accessibility guidance.

### Directionality

- Current Larixon mobile markets in accessible sources are left-to-right.
- Do not create RTL acceptance criteria unless the task explicitly introduces an RTL language.

### Touch targets

- Android interactive controls should meet at least `48dp`.
- iOS interactive controls should meet at least `44pt`.
- This is especially important for:
  - star rating controls
  - hide eye icon
  - delete/trash icon
  - chevron-driven profile rows
  - tab/filter chips

### Localized text checks

For UI that ships to multiple markets, verify that localized copy:

- is not clipped
- is not ellipsized in a broken way
- still leaves room for badges, counters, or action icons

Use at least `RU`, `MN`, and `EN` market variants when the task changes dense UI.

### Stable automation hooks

- Prefer stable semantics or accessibility identifiers for important controls.
- Decorative icons must not become separately focusable if the parent row is the real action.
- Badge counters should be announced meaningfully, for example as `2 pending reviews`, not as a bare number.

### Native screen reader passes

For high-risk UI changes, request at least one manual assistive-tech pass:

- TalkBack on Android
- VoiceOver on iOS

If manual a11y validation was not done, say that clearly in the result.
