---
name: "react-native-test-stabilization"
description: "Stabilize and fix React Native tests (Jest + React Native Testing Library): config, mocks for native modules/navigation, async/act warnings, flaky timers, and integration tests from test scenarios. Use during build/validation when RN tests fail."
version: 1
created: "2026-06-27"
updated: "2026-06-27"
---

# React Native Test Stabilization (Jest + RNTL)

## Setup
- `jest` preset `jest-expo` (Expo) or `react-native`; `@testing-library/react-native` + `@testing-library/jest-native`.
- `jest.setup.ts`: mock `react-native-reanimated`, `@react-navigation/native`, `react-native-config`, and any native module used.

## Common failures -> fixes
- "Invalid hook call" / wrapper missing: wrap render in providers (Navigation, QueryClient, store, ThemeProvider) via a custom `renderWithProviders`.
- `act(...)` warnings / async: use `findBy*`, `waitFor`, and `await` user events; advance fake timers with `jest.runAllTimers()`.
- Network: mock the api client (MSW or jest mocks) using `tsa.api_integration` shapes; assert request to the correct base URL/endpoint.
- Navigation assertions: spy on `navigation.navigate`.

## Coverage
- Component tests for key screens; integration tests for each `requirement.json` test scenario (CRUD): create/read/update/delete flows hitting the mocked backend.
- Gate: `tsc --noEmit` + ESLint clean + tests green before declaring build success.

## ESM / dynamic imports (common HARD failure — fixes `--experimental-vm-modules` error)
- **Never use `await import('...')` (dynamic import) in a test.** Jest throws `TypeError: A dynamic import callback was invoked without --experimental-vm-modules` because the default Jest VM has no ESM dynamic-import support. This is a frequent cause of an otherwise-finished app failing the test gate.
  - WRONG: `const api = await import('../services/api');`
  - RIGHT: `import * as api from '../services/api';` (or `import { createApi } from '../services/api';`) at the TOP of the test file.
- To reset module state between tests use `jest.resetModules()` + `require('../services/api')` (CommonJS `require` needs no ESM flag) — not `await import`.
- When a test asserts the api service's base-url / Authorization interceptor wiring, import the service statically and spy on / inject the axios instance; do NOT lazy-load it.
- Only if dynamic ESM import is genuinely unavoidable: set the `test` script to `node --experimental-vm-modules node_modules/.bin/jest` and add `"extensionsToTreatAsEsm": [".ts",".tsx"]` + an ESM `transform` in `jest.config.js`. Prefer the static-import fix over reconfiguring the runner.
- Test files belong in `src/__tests__/`, `src/screens/__tests__/`, `src/components/__tests__/` (per phase_folder_map) — write them directly at those paths; never create them at the project root and `mv` into `src/`.
