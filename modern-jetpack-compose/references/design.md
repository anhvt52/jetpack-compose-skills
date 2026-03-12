# Material 3 Design

## Use Theme Tokens, Not Hardcoded Values

Always reference design tokens from `MaterialTheme` rather than hardcoding colors,
typography, or shape values. This ensures the app adapts correctly to dark mode,
dynamic color, and user accessibility settings.

```kotlin
// Wrong
Text("Hello", color = Color(0xFF6200EE), fontSize = 20.sp)

// Correct
Text("Hello", color = MaterialTheme.colorScheme.primary, style = MaterialTheme.typography.titleLarge)
```


## Color Scheme

Material 3 uses a role-based color system:

| Token | Use |
|---|---|
| `primary` | Primary actions, key UI elements |
| `onPrimary` | Content on top of primary |
| `secondary` | Supporting actions |
| `surface` | Backgrounds, cards |
| `surfaceVariant` | Alternate surface |
| `error` | Error states |
| `outline` | Borders, dividers |

- Never use `MaterialTheme.colorScheme.primary.copy(alpha = 0.5f)` for disabled
  states — use `DisabledAlpha` constant or the semantic disabled state of components.
- Do not mix Material 3 color tokens with hardcoded `Color(...)` values in the
  same screen — pick one system.


## Dynamic Color (Android 12+)

Support dynamic color based on the user's wallpaper on Android 12+:

```kotlin
@Composable
fun AppTheme(
    darkTheme: Boolean = isSystemInDarkTheme(),
    dynamicColor: Boolean = Build.VERSION.SDK_INT >= Build.VERSION_CODES.S,
    content: @Composable () -> Unit
) {
    val colorScheme = when {
        dynamicColor && darkTheme -> dynamicDarkColorScheme(LocalContext.current)
        dynamicColor -> dynamicLightColorScheme(LocalContext.current)
        darkTheme -> DarkColorScheme
        else -> LightColorScheme
    }
    MaterialTheme(colorScheme = colorScheme, ..., content = content)
}
```


## Typography

Use M3 typography tokens:
`displayLarge`, `displayMedium`, `displaySmall`,
`headlineLarge`, `headlineMedium`, `headlineSmall`,
`titleLarge`, `titleMedium`, `titleSmall`,
`bodyLarge`, `bodyMedium`, `bodySmall`,
`labelLarge`, `labelMedium`, `labelSmall`.

Never hardcode `fontSize` for body text — let the typography system handle it
so Dynamic Type (font scaling) works correctly.


## Shapes

Use `MaterialTheme.shapes.small/medium/large/extraLarge` for consistent rounding.
Map your components to the appropriate shape size from the M3 shape scale.


## Adaptive Layouts

Support phone, tablet, and foldable form factors using `WindowSizeClass`:

```kotlin
val windowSizeClass = calculateWindowSizeClass(activity)
when (windowSizeClass.widthSizeClass) {
    WindowWidthSizeClass.Compact -> PhoneLayout()
    WindowWidthSizeClass.Medium -> TabletLayout()
    WindowWidthSizeClass.Expanded -> DesktopLayout()
}
```

Use `NavigationBar` for compact widths and `NavigationRail` or `NavigationDrawer`
for expanded widths.


## Design Consistency

- Define a `DesignTokens` or `AppTheme` object with custom spacing, icon sizes,
  and animation durations as named constants — do not scatter magic numbers.
- Test all screens in both light and dark themes.
- Test all screens at 200% font scale to catch text truncation issues.


## Material Expressive (BOM 2025.x)

When targeting BOM 2025.01.00+:

- Use `MotionScheme` for animation tokens: `MaterialTheme.motionScheme.defaultSpatialSpec()`
  provides spring animation specs consistent with the design system.
- `ButtonGroup` for grouped action buttons.
- `FloatingToolbar` for edge-anchored expandable toolbars.
- Shapes have been refined — review the M3 Expressive shape scale changes.
