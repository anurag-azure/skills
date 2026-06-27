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
