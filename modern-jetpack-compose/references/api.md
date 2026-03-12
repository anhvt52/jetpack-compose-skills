# Modern Jetpack Compose API

Always prefer the most current stable Compose API. Flag deprecated patterns and
suggest their modern replacements.


## Material 2 → Material 3

- Never use `androidx.compose.material` (Material 2) in new code. Use
  `androidx.compose.material3` (Material 3) exclusively.
- Replace `TopAppBar` from material with `TopAppBar` / `CenterAlignedTopAppBar`
  from material3.
- Replace `BottomNavigation` + `BottomNavigationItem` with `NavigationBar` +
  `NavigationBarItem`.
- Replace `Scaffold` parameters `topBar`, `bottomBar`, `floatingActionButton` —
  these exist in M3 Scaffold too, but verify `contentWindowInsets` is handled.
- Replace `MaterialTheme.colors` with `MaterialTheme.colorScheme`.
- Replace `MaterialTheme.typography` — M3 typography tokens differ (e.g.
  `titleLarge` not `h6`).
- Replace `Surface(elevation = ...)` with `Surface(tonalElevation = ...)` in M3.


## Scaffold & Padding

- Always use the `innerPadding: PaddingValues` provided by `Scaffold`'s content
  lambda. Apply it to the root content composable with `Modifier.padding(innerPadding)`.
- Never ignore the `innerPadding` parameter — content will be obscured by system
  bars or the bottom navigation bar.

```kotlin
// Before — padding ignored
Scaffold { _ ->
    LazyColumn { ... }
}

// After — padding applied
Scaffold { innerPadding ->
    LazyColumn(modifier = Modifier.padding(innerPadding)) { ... }
}
```


## State Collection

- Never use `Flow.collectAsState()` in a composable. Always use
  `Flow.collectAsStateWithLifecycle()` from `androidx.lifecycle:lifecycle-runtime-compose`.
  This respects the lifecycle and stops collection when the app is backgrounded.

```kotlin
// Before
val uiState by viewModel.uiState.collectAsState()

// After
val uiState by viewModel.uiState.collectAsStateWithLifecycle()
```


## Accompanist → Built-in

The following accompanist libraries have been superseded by built-in Compose APIs:

| Accompanist library | Replace with |
|---|---|
| `accompanist-systemuicontroller` | `WindowCompat.setDecorFitsSystemWindows()` + `Modifier.windowInsetsPadding()` |
| `accompanist-insets` | Built-in `WindowInsets` in `androidx.compose.foundation` |
| `accompanist-pager` | `HorizontalPager` / `VerticalPager` from `androidx.compose.foundation` |
| `accompanist-flowlayout` | `FlowRow` / `FlowColumn` from `androidx.compose.foundation.layout` |
| `accompanist-navigation-animation` | Built-in `AnimatedNavHost` / predictive back in Navigation Compose 2.7+ |
| `accompanist-permissions` | `rememberPermissionState` still available — no built-in replacement yet |


## Modifier Changes

- `Modifier.composed {}` is deprecated for creating stateful modifiers. Use the
  `Modifier.Node` API instead for custom stateful modifiers.
- `Modifier.clickable` without `indication = null` shows a ripple by default —
  this is correct behavior. Only suppress it intentionally.


## BackHandler

- `BackHandler` is the correct way to intercept back navigation in Compose. It is
  NOT deprecated. Use it when you need to intercept the system back gesture.
- Do not use `OnBackPressedCallback` directly in a composable — wrap it with
  `BackHandler` instead.


## Material Expressive (BOM 2025.x)

Available when using Compose BOM 2025.01.00 or later:

- `ButtonGroup` — for grouping related action buttons with shared emphasis.
- `FloatingToolbar` — expandable toolbar anchored to screen edges.
- `LoadingIndicator` — animated loading state indicator.
- `SplitButton` — button with a primary action and a secondary dropdown.
- Motion tokens — `MotionScheme` provides spring and tween animation presets from
  the theme: `MaterialTheme.motionScheme.defaultSpatialSpec()`.

Note: Check the project's Compose BOM version before using these components. Provide
a fallback or note the minimum version requirement.
