# Git Config Cheat Sheet

Quick reference table

| Setting | Command | Purpose |
| :--- | :--- | :--- |
| `user.name` | `git config --global user.name "Name"` | Set your name for commits
| `user.email` | `git config --global user.email "you@example.com"` | Set your email for commits
| `core.editor` | `git config --global core.editor "code --wait"` | Set preferred editor for commit messages
| `merge.tool` | `git config --global merge.tool meld` | Configure merge tool
| `push.default` | `git config --global push.default simple` | Control default push behavior
| `credential.helper` | `git config --global credential.helper manager` | Manage credentials (Windows Credential Manager)

---

## Overview

`git config` controls Git's behavior at system, global, and repository scopes. Use `--system`, `--global`, or local to set scope. Store safe defaults in global config while project-specific settings belong in the repo-specific config.

---

## Examples

Set your identity

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

Set default editor

```bash
git config --global core.editor "code --wait"
```

Enable color and helpful defaults

```bash
git config --global color.ui auto
git config --global pull.rebase false
```

Windows credential helper example

```bash
git config --global credential.helper manager
```

---

## Debugging & Edge Cases

- **Where is a setting coming from?** Use `git config --show-origin --list` to see source files for settings.
- **Overriding order**: Repository > Global (~/.gitconfig) > System. Passing `--global` writes to `~/.gitconfig`.
- **Environment variables**: Some git behaviors are influenced by env vars (e.g., `GIT_AUTHOR_NAME`, `GIT_COMMITTER_DATE`). Use with caution.

---

## How-tos

Edit the global config with your editor

```bash
git config --global --edit
```

List all settings and their origins

```bash
git config --show-origin --list
```

Set an alias in config

```bash
git config --global alias.co checkout
```

Secure credential storage for macOS/Windows

```bash
git config --global credential.helper osxkeychain  # macOS
# or
git config --global credential.helper manager         # Windows
```
