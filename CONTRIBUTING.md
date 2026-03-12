# Contributing

Contributions are welcome — improvements to existing reference files, new rules,
bug fixes, and corrections.

## What to Contribute

- **Corrections** — a rule that is wrong, outdated, or misleading
- **New rules** — a Compose best practice that isn't covered
- **API updates** — newer Compose BOM versions introduce new patterns
- **Examples** — clearer before/after code snippets
- **New reference files** — for topics not yet covered

## Guidelines

### Keep rules actionable

Every rule should be something an agent can act on. Prefer:

> "Use `collectAsStateWithLifecycle()` instead of `collectAsState()`."

Over:

> "Be aware of lifecycle considerations when collecting flows."

### Include before/after examples

Rules are much more useful with a concrete Kotlin snippet showing the wrong
pattern and the correct replacement.

### Stay focused

Each reference file covers one concern area. Add rules to the most relevant file.
If a rule spans multiple files, pick the primary one and cross-reference.

### Don't over-specify

Avoid rules that are purely stylistic opinion with no clear correctness benefit.
The skill is for best practices, not style enforcement.

## How to Contribute

1. Fork the repository
2. Create a branch: `git checkout -b fix/rule-name` or `feat/new-rule`
3. Make your changes
4. Open a pull request with a clear description of what changed and why

## Reporting Issues

Open a GitHub issue if you find:
- A rule that produces incorrect guidance
- A missing important best practice
- An API that has been updated and the rule is now outdated
