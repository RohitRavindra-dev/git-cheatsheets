# Git Reset & Revert Cheat Sheet

Quick reference table

| Command | Short | Description |
| :--- | :---: | :--- |
| `git reset --soft <commit>` | Reset Soft | Move HEAD to `<commit>`, keep index & working tree |
| `git reset --mixed <commit>` | Reset Mixed | Move HEAD to `<commit>`, reset index (default) |
| `git reset --hard <commit>` | Reset Hard | Move HEAD to `<commit>`, reset index and working tree (DESTRUCTIVE) |
| `git revert <commit>` | Revert | Create a new commit that undoes `<commit>` (safe for shared history) |
| `git restore <file>` | Restore | Restore file from HEAD or a commit (Git >=2.23) |

---

## Overview

`git reset` rewrites the current branch history by moving HEAD and optionally updating the index and working tree. `git revert` is the safe alternative for undoing commits on public branches because it creates a new commit that negates a previous one.

---

## Examples

Move branch back but keep changes staged

```bash
git reset --soft HEAD~1
```

Discard all local changes and match `origin/main`

```bash
git fetch origin
git reset --hard origin/main
```

Revert a bad commit (create an inverse commit)

```bash
git revert <bad-commit>
# resolve conflicts if any, then commit
```

Restore a single file from a previous commit

```bash
git restore --source <commit> --staged --worktree path/to/file
```

---

## Debugging & Edge Cases

- **Reset vs Revert:** Use `reset` for local, private branches. Use `revert` for public/shared branches to avoid rewriting history.
- **Accidental `reset --hard`**: If you haven't garbage-collected, `git reflog` can help find lost commits and recover.
- **Staging area confusion:** `git reset --mixed` moves HEAD and updates index; files remain in working tree unstaged.
- **Reverting merge commits:** Reverting merge commits requires `-m` parent selection, e.g., `git revert -m 1 <merge-commit>`.

---

## How-tos

Undo last commit but keep changes in working tree

```bash
git reset --soft HEAD~1
```

Create a safe undo on main

```bash
git revert <commit>
git push origin main
```

Recover from destructive reset

```bash
git reflog
# find commit SHA before reset
git switch -c recover <sha>
```
