---
name: "react-native-navigation-state-guidelines"
description: "React Navigation + state-management guidance for React Native: typed navigators, route params, deep links, and choosing/structuring Redux Toolkit vs Zustand vs Context. Use when wiring navigation and app state."
version: 1
created: "2026-06-27"
updated: "2026-06-27"
---

# React Native Navigation & State Guidelines

## Navigation (React Navigation)
- Use a typed `RootStackParamList` (and nested param lists). Type `useNavigation`/`useRoute`.
- Stack for flows, Tabs for top-level sections, Drawer where the design shows one.
- Build the navigator from `tsa.design_source.navigation_graph`. Each edge = a typed `navigation.navigate('Screen', params)`.
- Centralize linking config for deep links; keep route names in a constants file.

## State
- Decision: Redux Toolkit for complex shared/cross-screen state; Zustand for lighter global state; Context only for low-frequency values (theme, auth user).
- Server data belongs in React Query, NOT global state.
- Slices/atoms are typed; selectors are memoized; no business logic in components.
- Auth token in secure storage (Keychain/Keystore via expo-secure-store), exposed through an auth hook.

## Anti-patterns
- No navigation by string without param typing. No giant single store. No server data mirrored into Redux.
