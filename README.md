# smart-commit

A Claude Code skill that generates conventional commit messages following the [Conventional Commits](https://www.conventionalcommits.org) spec. It analyzes your staged changes, proposes a commit message, and always asks for your confirmation before committing.

## What it does

- Reads your staged changes and understands what was modified
- Picks the right **type** (`feat`, `fix`, `chore`, `ci`, `build`, etc.)
- Derives the **scope** from the files or modules changed
- Writes a clean, imperative subject line in English
- Adds a body only when the reason behind the change isn't obvious
- Detects when staged changes belong to **different concerns** and suggests splitting into separate commits
- **Never commits without your explicit confirmation**

## Examples

Simple change:
```
fix(voucher): restrict DeunaVeci rendering to CNB schema only
```

Multiple unrelated changes staged → proposes split:
```
1. build(android): upgrade compileSdkVersion to 34
2. chore(deps): bump react-native-camera and axios
```

Change with a non-obvious reason:
```
fix(voucher): restrict DeunaVeci rendering to CNB schema only

DeunaVeci was appearing in non-CNB flows, breaking business rules.
```

## Installation

### Requirements

- [Claude Code](https://claude.ai/code) installed and running

### Steps

1. Clone this repo:
```bash
git clone https://github.com/TatianaFonseca/smart-commit-skill
```

2. Copy the skill to your Claude skills folder:
```bash
cp -r smart-commit-skill/smart-commit ~/.claude/skills/
```

3. The skill is now active. No restart needed — Claude Code picks it up automatically.

## How to use it

Just talk naturally. The skill triggers automatically when you want to commit:

- `"quiero hacer un commit"`
- `"commit this"`
- `"I want to push my changes"`
- `"subir mis cambios al voucher"`

Claude will:
1. Check what's staged (`git diff --staged`)
2. Suggest splitting if changes are unrelated
3. Generate the commit message and show it to you
4. Wait for your confirmation before running `git commit`

## Commit types reference

| Type | When to use |
|------|-------------|
| `feat` | New feature or behavior |
| `fix` | Bug fix |
| `refactor` | Code change with no feature or bug impact |
| `chore` | Maintenance, config, dependency updates |
| `ci` | CI/CD changes |
| `docs` | Documentation only |
| `test` | Adding or fixing tests |
| `perf` | Performance improvement |
| `build` | Build system or tooling |
| `style` | Formatting, whitespace — no logic change |

## Uninstall

```bash
rm -rf ~/.claude/skills/smart-commit
```
