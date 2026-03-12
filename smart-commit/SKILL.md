---
name: smart-commit
description: >
  Generates conventional commit messages following Conventional Commits spec strictly.
  Use this skill whenever the user wants to commit changes, stage and commit, push to
  a branch, or asks for a commit message. Even if the user just says "commit this",
  "I want to push", "make a commit", or "subir cambios" — always use this skill.
  Never commit without explicit user confirmation.
---

# Smart Commit

Generate strict conventional commit messages, show them for confirmation, then commit.

## Step 1: Understand the changes

```bash
git status
git diff --staged
```

If nothing is staged, tell the user and ask if they want to stage specific files or everything (`git add .`).

## Step 2: Check if changes should be split

Before writing a single commit message, ask: **do these staged changes belong to the same concern?**

Split into separate commits when staged files touch **unrelated modules or concerns**. For example:
- Android build config changes + npm dependency bumps → two commits
- A new feature + a bug fix in a different area → two commits
- All changes are in the same feature/module → one commit is fine

If a split makes sense, tell the user:

```
I see changes in two unrelated areas. It'd be better to split these into two commits:

1. build(android): upgrade compileSdkVersion to 34
2. chore(deps): bump react-native-camera and axios

Want me to stage and commit them one by one?
```

Wait for their confirmation before proceeding.

## Step 3: Generate the commit message

Always follow **Conventional Commits** — no exceptions.

### Format
```
type(scope): short description

Optional body — only when the why isn't obvious from the subject line.

BREAKING CHANGE: description  ← only if applicable
```

### Types
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

### Scope
- Derived from the **module or area changed** (e.g., `voucher`, `auth`, `android`, `deps`)
- kebab-case, lowercase
- Omit only if the change is truly global

### Subject line
- Imperative mood: "add", "fix", "remove" — never "added", "fixes"
- Lowercase, no period at the end
- Max 72 characters total

### Body — keep it minimal
The body is **optional**. Only add it when the subject line alone doesn't explain *why* the change was needed. When in doubt, skip it.

- One or two sentences max
- Explain the *why*, not the *what* (the diff already shows the what)
- Skip it entirely for straightforward changes

Good example (body needed):
```
fix(voucher): restrict DeunaVeci to CNB schema only

DeunaVeci was appearing in non-CNB flows, breaking business rules.
```

Good example (no body needed):
```
chore(deps): upgrade axios to 1.7.2
```

### Breaking changes
- Add `BREAKING CHANGE: <description>` in footer
- Also append `!` to the type: `feat!(auth): ...`

## Step 4: Show and confirm

Present it clearly:

```
Here's the commit message:

---
fix(voucher): restrict DeunaVeci to CNB schema only
---

Good to go? I'll commit once you confirm.
```

**Never run `git commit` before the user confirms.** Always wait for an explicit "ok", "yes", "sí", "lgtm", or similar.

## Step 5: Commit

Once confirmed:

```bash
git commit -m "$(cat <<'EOF'
type(scope): short description
EOF
)"
```

Use a heredoc to preserve formatting. Show the result with `git log --oneline -1`.

## Edge cases

- **Nothing staged**: Show which files are unstaged, ask if they want to stage all or specific ones.
- **Split needed**: Propose the split, wait for confirmation, then commit one by one.
- **User provides a draft message**: Refine it to match the convention — don't replace it entirely.
- **Merge/revert commits**: Don't override auto-generated messages.
