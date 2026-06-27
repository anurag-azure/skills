---
name: "react-native-component-scaffold"
description: "Translate a Figma design (design_source) into faithful React Native screens and components: frame->component mapping, layout from constraints/auto-layout, tokens->StyleSheet, assets, navigation wiring. Use when scaffolding RN screens from Figma."
version: 1
created: "2026-06-27"
updated: "2026-06-27"
---

# React Native Component Scaffold (Figma -> RN)

Input: `tsa.design_source` (figma_files, design_tokens, screens[], components[], navigation_graph) + the raw figma_json.json/figma_screens.json.

## Procedure
1. **Theme first.** Generate `theme/index.ts` from `design_tokens` (colors, typography, spacing, radius, shadow). All components consume the theme.
2. **Components.** For each `design_source.components[]` (figma_frame -> component), scaffold a typed presentational component. Map Figma auto-layout -> flexbox (`flexDirection`, `justifyContent`, `alignItems`, `gap`); constraints -> `position`/`flex`; fills -> `backgroundColor`/`Image`; text nodes -> `<Text>` with typography token.
3. **Screens.** For each `design_source.screens[]`, build `screens/<Screen>.tsx` from its `component_tree`, honoring `layout` (x,y,w,h or auto-layout) and `tokens_used`. Pull images from `assets`/`image_ref`.
4. **Navigation.** Wire `navigation_graph` edges into the navigator (React Navigation): each `{from, trigger, to}` becomes a navigation action.
5. **Data.** Bind screens to API hooks per `tsa.api_integration.screen_bindings`.

## Fidelity rules
- Do not invent screens/components not in the design; do not drop any connected screen.
- Match spacing/typography/colors to tokens exactly; no hardcoded values.
- Honor `fidelity_directives`. Keep pixel/structure parity with the named Figma screen.
- Output is compile-clean TypeScript (no `any`, no TODO).
