## Playwright Web Coverage (Optional)

Keep this slot disabled for the current AW-3 mobile baseline.
Only add it to the final ASGARD role if the integration tester is explicitly expected to cover Larixon web, hybrid, or mobile-webview scenarios in a separate approved flow.

### Safe default

- Current accessible sources do not confirm a dedicated Larixon Playwright web repo, package command, or environment matrix for this mobile workflow.
- Do not invent repo URLs, commands, CI jobs, or base URLs.
- Do not enable this slot for native-only mobile tasks.
- Server clarified on `2026-04-01` that `Playwright` is for web and should not be described inside the current mobile app tester skills.

### Before enabling Playwright guidance, collect

- confirmed GitLab repo URL
- package manager and test command
- base URLs per stand
- how Allure/TestOps is wired for that repo
- whether the feature is native-only, hybrid, or webview-based

### When Playwright is appropriate

- webview content that cannot be covered well from native UI tests
- shared responsive web surfaces reused inside the mobile product
- browser-level behavior such as redirects, cookies, or external auth pages
- cases where the owner explicitly confirms that web coverage belongs to the same tester workflow

### Current decision for AW-3

- Do not reference this slot from the default mobile tester prompts.
- Escalate only if the task explicitly expands from native mobile into web or hybrid coverage.

### Reporting rule

If the required web details are missing, escalate with a short list of missing inputs instead of writing fake Playwright instructions.
