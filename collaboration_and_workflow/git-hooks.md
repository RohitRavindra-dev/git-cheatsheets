# Git Hooks Cheat Sheet

Quick reference table

| Hook (client) | When | Typical use |
| :--- | :--- | :--- |
| `pre-commit` | Before commit | Run linters, tests, block bad commits |
| `prepare-commit-msg` | Before editor | Modify commit message template |
| `commit-msg` | After message | Validate commit message format (e.g., Conventional Commits) |
| `pre-push` | Before push | Run tests or build to prevent broken pushes |
| `post-commit` | After commit | Notifications, local automation |

| Hook (server) | When | Typical use |
| :--- | :--- | :--- |
| `pre-receive` | On push | Block push based on policy (CI, commit message, signed commits) |
| `update` | On push per ref | Validate single ref updates |
| `post-receive` | After push | Trigger CI, deployments, notifications |

---

## Overview

Git hooks are scripts that run at key points in Git's lifecycle. Client hooks run locally; server hooks run on the central repository. Hooks live in the `.git/hooks/` directory and must be executable. For reproducible team hooks, use a framework (e.g., `pre-commit`, Husky) to install them in contributors' environments.

---

## Examples

Simple `pre-commit` shell hook to run `eslint` and prevent commit if it fails

```bash
#!/usr/bin/env bash
npm run lint || { echo "Lint failed, aborting commit"; exit 1; }
```

Commit-msg hook to enforce Conventional Commits style (simple example)

```bash
#!/usr/bin/env bash
MSG_FILE="$1"
MESSAGE=$(cat "$MSG_FILE")
if ! echo "$MESSAGE" | grep -E "^(feat|fix|docs|chore|refactor)(\(.+\))?: .+"; then
  echo "Invalid commit message format"; exit 1
fi
```

Use `pre-commit` framework (Python) example config to run black and flake8

```yaml
repos:
- repo: https://github.com/psf/black
  rev: 23.1.0
  hooks:
  - id: black
- repo: https://github.com/pycqa/flake8
  rev: 4.0.1
  hooks:
  - id: flake8
```

---

## Debugging & Edge Cases

- **Hooks not running:** Ensure script is executable (`chmod +x .git/hooks/pre-commit`) and path is correct. Frameworks like `pre-commit` install hooks into `.git/hooks`.
- **CI vs Hook duplication:** Keep heavy operations in CI; use hooks for fast client-side checks only.
- **Blocking commits/pushes:** Use hooks sparingly; overly strict hooks can frustrate contributors. Prefer warnings or CI gates for slow checks.
- **Hook portability:** Use portable scripts (POSIX shell) or ship language-specific runtimes via container or tool (e.g., node, python) documented in CONTRIBUTING.

---

## How-tos

Install `pre-commit` and enable hooks

```bash
pip install pre-commit
pre-commit install
pre-commit run --all-files
```

Set up a simple server-side `pre-receive` hook to reject unsigned commits (pseudo-script)

```bash
#!/usr/bin/env bash
while read oldrev newrev refname; do
  # inspect commits and reject if unsigned (implement verification here)
  :
done
```

Make a hook executable

```bash
chmod +x .git/hooks/pre-commit
```
