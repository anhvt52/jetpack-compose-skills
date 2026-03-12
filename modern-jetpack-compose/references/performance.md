# Performance

## LazyList Optimization

- Always provide a `key` parameter in `LazyColumn` / `LazyRow` items. See
  `references/recomposition.md` for details.
- Use `contentType` for heterogeneous lists to allow Compose to reuse item
  composition slots of the same type:

```kotlin
LazyColumn {
    items(feedItems, key = { it.id }, contentType = { it.type }) { item ->
        when (item.type) {
            FeedType.Post -> PostCard(item)
            FeedType.Ad -> AdCard(item)
        }
    }
}
```

- Avoid `Column { items.forEach { ... } }` for large lists — it composes all
  items at once. Use `LazyColumn` for lists of more than ~10 items.
- Avoid expensive operations in the `items` block — do filtering, mapping, and
  sorting outside the lazy list (in the ViewModel or as `remember`-ed values).


## remember for Expensive Calculations

Wrap any expensive calculation in `remember` so it is not recalculated on every
recomposition:

```kotlin
// Recalculated on every recomposition — wrong
val sortedBooks = books.sortedBy { it.title }

// Calculated once, re-calculated only when books changes — correct
val sortedBooks = remember(books) { books.sortedBy { it.title } }
```

Never create `Paint`, `Path`, `DateFormatter`, or other expensive objects inline
during composition.


## Scope of State Reads

Read state as late as possible to narrow which phase of the pipeline reruns.
Prefer reading state inside `Modifier` lambda parameters (layout/draw phase)
over reading it in the composable body (composition phase):

```kotlin
// Reads in composition — whole composable recomposes on scroll
val scrollOffset = scrollState.value
Modifier.offset(y = scrollOffset.dp)

// Reads in layout phase only — avoids composition phase recomposition
Modifier.offset { IntOffset(0, scrollState.value) }
```


## graphicsLayer for Visual Transforms

Use `Modifier.graphicsLayer { }` for alpha, scale, rotation, and translation
transforms. This runs in the draw phase and avoids composition recomposition:

```kotlin
Box(
    modifier = Modifier.graphicsLayer {
        alpha = animatedAlpha
        scaleX = animatedScale
        scaleY = animatedScale
    }
)
```


## Avoid Unnecessary Work in Body

- Move any non-trivial logic out of composable bodies and into the ViewModel or
  a `remember`-ed value.
- `body` executes on every recomposition. Treat it like a hot path.
- Avoid conditional logic that creates structurally different compositions (use
  ternaries / `animateContentSize` instead of if/else that adds/removes composables).


## Profiling Tools

- **Layout Inspector** (Android Studio): inspect composition hierarchy, see
  recomposition counts, identify hot composables.
- **Composition Tracing**: add `androidx.compose.runtime:runtime-tracing` to see
  composable names in Perfetto/Android Studio Profiler traces.
- **Baseline Profiles**: generate with `androidx.profileinstaller` and Macrobenchmark
  to reduce startup time for AOT compilation of Compose code.
