---
name: "react-native-best-practices"
description: "React Native (TypeScript) implementation standards: project layout, functional components + hooks, navigation, state, API layer, theming from design tokens, performance, accessibility, testing. Use when generating or reviewing a React Native app."
version: 1
created: "2026-06-27"
updated: "2026-06-27"
---

# React Native Best Practices

## Project layout
```
src/
  screens/        # one folder per screen (Screen.tsx + styles + tests)
  components/     # reusable presentational + container components
  navigation/     # navigators, route types, linking
  state/          # store, slices/atoms, selectors
  services/api/   # api client, endpoint functions, types
  hooks/          # custom hooks
  theme/          # tokens -> theme (colors, typography, spacing, radius, shadow)
  utils/  types/  assets/
__tests__/        # or co-located *.test.tsx
App.tsx           # entry
```

## Components
- Functional components + hooks only. No class components.
- One responsibility per component (SOLID-S). Split presentational vs container.
- Type every prop with an explicit `Props` interface. No `any`.
- Derive styles from the theme tokens (never hardcode hex/spacing). Use `StyleSheet.create`.

## State
- Local UI state: `useState`/`useReducer`. Server state: React Query (preferred) or thunks.
- Global app state: Redux Toolkit or Zustand — pick one, declared in tsa.technology.state.
- Never duplicate server data in global state; cache via React Query.

## API layer (must call the real backend)
- One typed client in `services/api/` built from `tsa.api_integration` (base_url per env, endpoints, schemas).
- Read base URL from env (`react-native-config` / Expo `extra`), never hardcode.
- Every screen binding in `tsa.api_integration.screen_bindings` maps to a hook (`useXxxQuery`/`useXxxMutation`) fired on the declared trigger (onMount/onClick/onSubmit).
- Centralized error + loading + empty states; surface validation errors (e.g. msisdn pattern) from `entities`.

## Theming / design fidelity
- Build `theme/` from `tsa.design_source.design_tokens`. Map every Figma token (colors/typography/spacing/radius/shadow) into the theme.
- Reproduce each screen from `design_source.screens[].component_tree` and `layout`. Match the named Figma screen.

## Performance & a11y
- Lists: `FlatList`/`SectionList` with `keyExtractor` + `getItemLayout` where possible; `React.memo`/`useMemo`/`useCallback` for hot paths.
- Accessibility: `accessibilityRole`/`accessibilityLabel`, hit slop, dynamic type.

## Quality gates
- `tsc --noEmit` clean, ESLint clean, no TODO/STUB, every screen+endpoint covered, secure storage for tokens (never AsyncStorage for secrets).
