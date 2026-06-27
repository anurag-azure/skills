---
name: "react-web-test-stabilization"
description: "Stabilize React web tests (Vitest/Jest + React Testing Library): provider wrappers, router/query mocks, async findBy/waitFor, MSW for network. Use during build/validation when web tests fail."
version: 1
created: "2026-06-27"
updated: "2026-06-27"
---

# React Web Test Stabilization (Vitest/Jest + RTL)

- Use `renderWithProviders` wrapping Router + QueryClient + store + theme.
- Network: MSW handlers shaped from `tsa.api_integration`; assert correct base URL/endpoint/method.
- Async: prefer `findBy*`/`waitFor`; `await userEvent...`; fake timers flushed.
- Router: use `MemoryRouter`/`createMemoryRouter` with the route under test; assert navigation.
- Coverage: per-route component tests + integration tests for each requirement.json scenario.
- Gate: `tsc --noEmit` + ESLint + tests + `vite build` before success.
