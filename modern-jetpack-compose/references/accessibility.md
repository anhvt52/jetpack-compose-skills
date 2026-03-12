# Accessibility

## Content Descriptions

Every `Image` composable must have a `contentDescription`:

- **Decorative images** (purely visual, no information): `contentDescription = null`.
- **Meaningful images**: provide a localized string describing the content.

```kotlin
// Decorative
Image(painter = painterResource(R.drawable.banner), contentDescription = null)

// Meaningful
Image(
    painter = painterResource(R.drawable.avatar),
    contentDescription = stringResource(R.string.user_avatar_description)
)
```

Flag any `Image` with a non-obvious resource name (e.g., `R.drawable.img_003`) that
has `contentDescription = null` without a clear comment explaining why it is decorative.


## Semantics

Use `Modifier.semantics { }` to add or override accessibility information:

```kotlin
Box(
    modifier = Modifier.semantics {
        contentDescription = "Profile picture of ${user.name}"
        role = Role.Image
    }
)
```

Common semantic properties:
- `contentDescription` — overrides the TalkBack announcement
- `role` — `Role.Button`, `Role.Image`, `Role.Switch`, `Role.Tab`, `Role.RadioButton`
- `stateDescription` — describes the current state (e.g., "Checked", "Expanded")
- `heading` — marks a node as a section heading


## mergeDescendants

Use `mergeDescendants = true` to group a composable's children into a single
TalkBack announcement, preventing the user from navigating through each child
element individually.

```kotlin
Row(modifier = Modifier.semantics(mergeDescendants = true) { }) {
    Icon(Icons.Default.Star, contentDescription = null) // hidden by merge
    Text("4.5 stars")
    Text("(128 reviews)")
    // TalkBack reads: "4.5 stars, 128 reviews" as one item
}
```


## clearAndSetSemantics

Use `clearAndSetSemantics { }` to completely replace auto-generated semantics
with a custom description, removing all child semantics.

```kotlin
Row(modifier = Modifier.clearAndSetSemantics {
    contentDescription = "Rating: 4.5 stars from 128 reviews"
}) {
    StarRating(4.5f)
    Text("(128 reviews)")
}
```


## Touch Targets

Minimum touch target size is **48×48dp**. Use `Modifier.minimumInteractiveComponentSize()`
on custom interactive elements to enforce this automatically.

Material components (`Button`, `IconButton`, etc.) handle this internally — do not
add extra padding on top.


## Color Contrast

- Ensure text contrast ratio meets WCAG AA: at least 4.5:1 for normal text,
  3:1 for large text (18sp+ or 14sp bold+).
- Never use color as the only way to convey information. Pair with an icon, text,
  or pattern.

```kotlin
// Wrong — only color differentiates status
Box(modifier = Modifier.background(if (isOnline) Color.Green else Color.Red))

// Correct — icon + color
Row {
    Icon(if (isOnline) Icons.Default.CheckCircle else Icons.Default.Cancel, ...)
    Text(if (isOnline) "Online" else "Offline")
}
```


## Custom Interactive Elements

When using `Modifier.clickable` or `Modifier.pointerInput` on non-Button composables,
add semantic role and action:

```kotlin
Box(
    modifier = Modifier
        .clickable(onClickLabel = "Open book details") { onBookClick() }
        .semantics { role = Role.Button }
) { ... }
```

Prefer `Button` / `IconButton` over custom clickable elements when possible —
they include correct semantics out of the box.


## Custom Accessibility Actions

For complex custom interactions, provide named accessibility actions:

```kotlin
Modifier.semantics {
    customActions = listOf(
        CustomAccessibilityAction("Add to favorites") { onFavorite(); true },
        CustomAccessibilityAction("Share") { onShare(); true }
    )
}
```
