# Modern Jetpack Compose — Agent Skill

An agent skill for writing, reviewing, and reasoning about modern Android UI code
using Jetpack Compose. Compatible with Claude Code, Cursor, Codex, and any platform
that supports the Agent Skills format.

---

## What It Does

Load this skill to give your AI agent deep knowledge of modern Jetpack Compose best
practices. It helps agents:

- **Generate** idiomatic Compose code from scratch
- **Review** existing code for deprecated APIs, anti-patterns, and accessibility gaps
- **Guide** architecture decisions around state management, navigation, and performance

---

## Skill Coverage

| Reference File | Topics Covered |
|---|---|
| `api.md` | Deprecated APIs → modern replacements, Material 2 → 3, accompanist migrations |
| `composables.md` | Naming conventions, state hoisting, slot APIs, `@Preview`, modifier contract |
| `state.md` | MVVM + UDF, ViewModel + StateFlow, `remember` vs `rememberSaveable`, `derivedStateOf` |
| `effects.md` | `LaunchedEffect`, `DisposableEffect`, `SideEffect`, `rememberCoroutineScope` |
| `recomposition.md` | `@Stable`/`@Immutable`, unstable collections, `key()` in lazy lists, lambda stability |
| `navigation.md` | Type-safe Navigation Compose, nested graphs, BackStack-scoped ViewModels, deep links |
| `design.md` | Material 3 theming, dynamic color, adaptive layouts, Material Expressive (BOM 2025.x) |
| `accessibility.md` | TalkBack, semantics, `mergeDescendants`, touch targets, color contrast |
| `performance.md` | LazyList optimization, `remember`, deferred state reads, `graphicsLayer` |
| `kotlin.md` | Sealed interfaces, coroutines, `stateIn`, null safety, `when` expressions |
| `hygiene.md` | Testing with `ComposeTestRule`, lint/detekt, secrets, resource naming |

---

## Installation

### Claude Code

Add to your `CLAUDE.md` or project settings:

```
Use the skill at: modern-jetpack-compose/SKILL.md
```

Or reference it directly when prompting:

```
Using the modern-jetpack-compose skill, review HomeScreen.kt for issues.
```

### Cursor / Codex / OpenAI-compatible platforms

Use the config at `modern-jetpack-compose/agents/openai.yaml`.

---

## Target Platform

- **Compose BOM:** 2024.x (stable default), with notes for 2025.x (Material Expressive)
- **Material Design:** Material 3
- **Architecture:** MVVM + Unidirectional Data Flow (UDF)
- **Language:** Kotlin with coroutines, Flow, and sealed interfaces

---

## Example Usage

**Reviewing code:**
> "Using the modern-jetpack-compose skill, review the files in `feature/home/` for
> best practices issues."

**Generating code:**
> "Using the modern-jetpack-compose skill, write a `BookListScreen` with a LazyColumn,
> search bar, and empty state, backed by a ViewModel."

**Targeted check:**
> "Using `references/accessibility.md` from the modern-jetpack-compose skill, check
> all composables in `components/` for accessibility issues."

---

## Output Format

The skill organizes findings by file. Each issue includes:

1. File and line number
2. Rule being violated
3. Before/after Kotlin snippet

Results end with a prioritized summary of the most impactful changes.

---

## License

MIT — see [LICENSE](LICENSE).

## Author

Anh Vu — [anhvt52@gmail.com](mailto:anhvt52@gmail.com)

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).
