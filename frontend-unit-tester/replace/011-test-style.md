## Larixon Frontend Unit Test Style

### Default rule

Follow the dominant style of the touched module. Do not force a universal style across the whole frontend codebase.

### React / Next.js

- Use React Testing Library for component tests — do not use Enzyme
- Prefer the style already used in the module:
  - Functional test files with `describe` / `it` blocks in newer code
  - Flat `test()` calls in simpler utility modules
- Do not convert an existing test file's style just to add one test

### Naming conventions

- Test files: `ComponentName.test.tsx`, `useHookName.test.ts`, `utils.test.ts` — follow existing convention
- Describe blocks: `describe('ComponentName', () => { ... })`
- Test names: `it('renders loading state when data is fetching', () => { ... })`
- Keep names descriptive enough to serve as documentation

### Component testing patterns

- Render with minimal required props
- Query by role, label, or text — not by class name or test ID unless no semantic alternative exists
- Use `userEvent` for interactions, not `fireEvent` (unless the module already uses `fireEvent`)
- Assert visible output, not internal state or implementation details
- Test loading, error, and empty states when the component handles them

```tsx
import { render, screen } from '@testing-library/react'
import userEvent from '@testing-library/user-event'

describe('FilterPanel', () => {
  it('clears selection when reset button is clicked', async () => {
    render(<FilterPanel categories={mockCategories} />)
    await userEvent.click(screen.getByRole('button', { name: /reset/i }))
    expect(screen.queryByRole('option', { selected: true })).not.toBeInTheDocument()
  })
})
```

### Hook testing patterns

- Use `renderHook` from React Testing Library
- Wrap with necessary providers (QueryClientProvider, context providers)
- Assert return values and state transitions, not internal implementation

```tsx
import { renderHook, act } from '@testing-library/react'

describe('useFilter', () => {
  it('updates selected category', () => {
    const { result } = renderHook(() => useFilter())
    act(() => result.current.setCategory('vehicles'))
    expect(result.current.category).toBe('vehicles')
  })
})
```

### API / data layer mocking

- Use MSW for network-level mocking when the component or hook fetches data
- Use Jest module mocks for simple utility dependencies
- Do not mock React internals (useState, useEffect)
- Mock at the boundary, not deep inside the implementation

### Assertions

- Use `@testing-library/jest-dom` matchers: `toBeInTheDocument`, `toHaveTextContent`, `toBeVisible`
- Use standard Jest matchers for non-DOM assertions
- Prefer specific matchers over generic `toBeTruthy()`

### Mandatory writing rules

- One test = one behavior or one closely related interaction
- Use real domain names from the task, not placeholder entities
- Keep comments rare and only for non-obvious setup
- If a module is legacy, extend the legacy style cleanly instead of starting a style war inside one file
- Wrap components with required providers in a shared `renderWithProviders` utility if one exists

### Do not

- Test implementation details (internal state, private methods, hook internals)
- Snapshot-test entire component trees — use targeted assertions instead
- Use `container.querySelector` when RTL queries are available
- Mix testing styles within one file
- Add `waitFor` or `findBy` when the result is synchronous
- Use `time.sleep()` or `setTimeout` in tests — use RTL async utilities
