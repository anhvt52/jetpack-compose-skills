# Modern Jetpack Compose — Agent Skill

<!-- yarkr:badges:start -->
[![Indexed on Yarkr](https://api.yarkr.com/functions/v1/api/api/v1/badges/repos/anhvt52/jetpack-compose-skills/version.svg)](https://yarkr.com/arks/anhvt52/jetpack-compose-skills)
<!-- yarkr:badges:end -->

An agent skill for writing, reviewing, and reasoning about modern Android UI code
using Jetpack Compose. Compatible with Claude Code, Cursor, Codex, and any platform
that supports the Agent Skills format.

![Claude Code](https://img.shields.io/badge/Claude_Code-supported-blueviolet?logo=anthropic)
![Cursor](https://img.shields.io/badge/Cursor-supported-blue?logo=cursor)
![Codex](https://img.shields.io/badge/Codex-supported-green?logo=openai)
![Gemini](https://img.shields.io/badge/Gemini-supported-orange?logo=google)

---

## Why This Skill?

LLMs frequently generate outdated or incorrect Jetpack Compose code. Common problems:

- **Wrong state collection** — using `collectAsState()` instead of `collectAsStateWithLifecycle()`
- **Unstable recomposition** — passing unstable lambdas or undecorated data classes that cause unnecessary redraws
- **Ignored Scaffold padding** — forgetting to apply `paddingValues` from `Scaffold`'s content lambda, causing content to render under system bars
- **Stale APIs** — referencing accompanist libraries that have since been upstreamed into Compose, or Material 2 APIs that have Material 3 equivalents
- **Architecture drift** — mixing state hoisting patterns or bypassing the UDF model

This skill gives your AI agent a curated, up-to-date reference for all of these areas so it generates correct, idiomatic Compose code from the start.

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

### Recommended — npx (Claude Code, Cursor, Codex, Gemini)

```bash
npx skills add https://github.com/anhvt52/jetpack-compose-skills
```

This installs the skill for all detected agents on your machine. You can also scope it:

```bash
# Project-level only
npx skills add https://github.com/anhvt52/jetpack-compose-skills

# Global (all your projects)
npx skills add https://github.com/anhvt52/jetpack-compose-skills -g
```

If `npx` is unavailable, install Node.js first:

```bash
brew install node
```

### Manual — Claude Code

Clone the repo and add to your project's `CLAUDE.md`:

```
Use the skill at: path/to/modern-jetpack-compose/SKILL.md
```

### Manual — Cursor / Codex / OpenAI-compatible platforms

Use the config at `modern-jetpack-compose/agents/openai.yaml`.

---

<!-- yarkr:install:start -->
## Install with Yarkr

This repo is indexed on [Yarkr](https://yarkr.com/arks/anhvt52/jetpack-compose-skills) as the ark [anhvt52/jetpack-compose-skills](https://yarkr.com/arks/anhvt52/jetpack-compose-skills), so its skills install into Claude Code, Codex, Cursor and other agent harnesses with one command.

### CLI

Install the whole ark, which Yarkr keeps equal to this repo:

```bash
npx yarkr-cli sync anhvt52/jetpack-compose-skills
```

Or add individual skills:

```bash
npx yarkr-cli add skill:anhvt52/jetpack-compose-skills/modern-jetpack-compose
```

No Node.js? `curl -fsSL https://yarkr.com/install.sh | sh` (macOS, Linux) or `irm https://yarkr.com/install.ps1 | iex` (Windows PowerShell) installs `yarkr` as a standalone binary; then type `yarkr` in place of `npx yarkr-cli`.

### MCP

Connect your agent to the ark over MCP:

```bash
npx yarkr-cli mcp add anhvt52/jetpack-compose-skills
```

Or point any MCP client at `https://api.yarkr.com/functions/v1/api/mcp/o/anhvt52/jetpack-compose-skills`.
<!-- yarkr:install:end -->

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
