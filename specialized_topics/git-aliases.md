# Git Aliases Cheat Sheet

Quick reference table

| Alias | Command | Purpose |
| :--- | :--- | :--- |
| `git co` | `git checkout` | Short alias for switching (common)
| `git br` | `git branch` | List/create/delete branches
| `git st` | `git status` | Quick status
| `git lg` | `git log --oneline --graph --decorate --all` | Compact, visual history
| `git amend` | `git commit --amend --no-edit` | Amend last commit (no message change)
| `git unstage` | `git restore --staged` | Unstage files quickly

---

## Overview

Aliases save typing and encourage consistent commands. Keep aliases discoverable (document them) and avoid redefining commands that differ subtly from built-in behavior to reduce confusion for collaborators.

---

## Recommended aliases (examples)

Add these to your global git config (`git config --global --edit`) or set them from the CLI:

```ini
[alias]
  co = checkout
  br = branch
  st = status
  ci = commit
  lg = log --oneline --graph --decorate --all
  amend = commit --amend --no-edit
  unstage = restore --staged
  last = log -1 --stat
  unpushed = log --branches --not --remotes
```

---

## Examples

Set an alias from the CLI

```bash
git config --global alias.lg "log --oneline --graph --decorate --all"
```

Use alias

```bash
git lg
```

Share aliases via dotfiles or a setup script so team members can opt-in; avoid adding surprising behavior to common commands in shared repos.

---

## Debugging & Edge Cases

- **Aliases are local to your config**: They do not propagate to other contributors unless shared.
- **Alias conflicts**: If a built-in command exists with same name, the alias will override it — be careful.
- **Complex aliases**: For commands needing pipes or complex shell expressions, store a script and make an alias call the script for portability.

---

## How-tos

Create a shell alias that calls an external script

```ini
[alias]
  cleanup = !sh ./scripts/git-cleanup.sh
```

Export your aliases for sharing

```bash
git config --list | grep alias. > git-aliases.txt
```

Best practice: document key aliases in `CONTRIBUTING.md` or a shared wiki so teammates can adopt them if desired.
