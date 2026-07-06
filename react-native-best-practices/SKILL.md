---
name: "react-native-best-practices"
description: "Expo + React Native (TypeScript) standards: Expo Router layout, functional components, React Query, API layer from TSA, theme from Figma tokens, performance, a11y. Use when generating or reviewing an Expo mobile app."
version: 2
created: "2026-06-27"
updated: "2026-07-06"
sources_merged: "anurag-azure v1; partial Jeffallan/react-native-expert (Expo runtime patterns)"
---

# React Native / Expo Best Practices

## Project layout (Expo Router — preferred for greenfield)

```
app/
  _layout.tsx                 # root Stack / providers (QueryClient, Session)
  (family-plan)/
    _layout.tsx               # feature group; modal presentation for FP-MODALS
    <expo_route>.tsx          # one file per design_source.screens[].expo_route
src/
  components/                 # reusable UI (MemberCard, SwipeToConfirm, …)
  hooks/                        # useFamilyGroup, useFamilyMembers, …
  services/api/                 # axios client + familyPlanApi endpoints
  services/auth/                # cognito + expo-secure-store
  store/                        # SessionContext
  theme/                        # from design_tokens — sole source of visual values
  types/
  assets/layouts/               # optional per-screen Figma JSON for FigmaCanvas
__tests__/                      # Jest + RTL (jest-expo preset)
app.json | app.config.ts
package.json
```

Legacy React Navigation stack layout (`src/screens/` + `navigation/`) is allowed only when TSA/ADR explicitly rejects Expo Router.

## Components
- Functional components + hooks only.
- Typed `Props` interfaces; no `any`.
- Styles from `theme` via `StyleSheet.create` — never hardcode hex/spacing in components.
- Figma fidelity layer: `FigmaCanvas` + functional overlays (forms, API states) when `assets/layouts/*.json` exists.

## State
- Server state: **TanStack Query v5** (`useQuery` / `useMutation`).
- Session: **React Context** (`SessionContext` — `accountNum`, JWT).
- Local UI: `useState` / `useReducer` per screen.
- Never duplicate server data in global client state.

## API layer (zero mock — CONSTR-001)
- Typed client from `tsa.api_integration` (base_url per env, 10 endpoints).
- Base URL from Expo `extra` / env — never hardcode ngrok in source.
- Every `screen_bindings` row → hook fired on declared trigger (`onMount` / `onSubmit` / `onConfirm`).
- Four UI states on data screens: SkeletonLoader, EmptyState, ErrorToast, success data render.

## Theming / design fidelity
- `theme/` from `tsa.design_source.design_tokens` only.
- Do **not** import generic palettes from external UX skill — Figma tokens are authoritative.
- Match named Figma screen; optional layout JSON for absolute-position canvas layer.

## Performance (from react-native-expert)
- Lists: `FlatList` / `SectionList` — not `ScrollView` for long member lists.
- List items: `React.memo` + `useCallback` for `renderItem` / `keyExtractor`.
- `removeClippedSubviews`, sensible `windowSize` on large lists.
- Images: sized assets; avoid full-width uncached remote loads on every row.

## Platform UX (Expo)
- `SafeAreaView` or `react-native-safe-area-context` for notches.
- Forms: `KeyboardAvoidingView` + `keyboardShouldPersistTaps="handled"`.
- Android hardware back: handle in modal flows via Expo Router `router.back()`.
- RTL: `I18nManager.isRTL` + theme-aware flex; test Arabic layout.

## Security
- JWT: **expo-secure-store** only — never AsyncStorage for secrets.
- MSISDN validation client-side before API (`^\+[0-9]{8,15}$`).

## Quality gates
- `npx tsc --noEmit` clean; ESLint clean; no TODO/STUB.
- Every story ID registered; every bound endpoint wired.
- Run `npx expo doctor` after dependency changes (see `react-native-expo-runtime` skill).
