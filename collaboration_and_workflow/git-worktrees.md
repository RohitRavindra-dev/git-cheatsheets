# Git Worktrees Cheat Sheet

Quick reference table

| Command | Short | Description |
| :--- | :---: | :--- |
| `git worktree add <path> [<branch>]` | Add | Create additional working tree linked to repository |
| `git worktree list` | List | Show worktrees and their branches |
| `git worktree remove <path>` | Remove | Remove a worktree (must be clean) |
| `git worktree prune` | Prune | Remove stale worktree references |
| `git worktree lock <path>` | Lock | Prevent removal of a worktree accidentally |

---

## Overview

`git worktree` lets you check out multiple branches at once in the same repository without cloning. It's lightweight and ideal for parallel work (e.g., backporting, testing, or reviewing large changes).

---

## Examples

Create a worktree for a branch

```bash
git worktree add ../feature-x feature-x
# work in ../feature-x independently from main working tree
```

Create a worktree from a commit (detached)

```bash
git worktree add ../temp <commit-sha>
```

Remove a worktree when done

```bash
git worktree remove ../feature-x
# if removal fails, ensure no processes are using files and that worktree is clean
```

---

## Debugging & Edge Cases

- **Unclean worktree removal**: `git worktree remove` will fail if worktree has local changes; either commit/stash them or force remove after backup.
- **Stale worktrees**: If a worktree was deleted manually, run `git worktree prune` to clean references.
- **Locks**: Use `git worktree lock <path>` to prevent accidental removal during important operations.
- **Disk paths**: Worktrees live outside the main working directory — ensure CI/tools expect this layout.

---

## How-tos

Create a temporary worktree for testing

```bash
git worktree add --detach ../tmp-test <commit-or-branch>
# run tests, then remove
git worktree remove ../tmp-test
```

List and clean up stale references

```bash
git worktree list
git worktree prune
```
