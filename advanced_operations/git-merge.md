# Git Merge Cheat Sheet

Quick reference table

| Command | Short | Description |
| :--- | :---: | :--- |
| `git merge <branch>` | Merge | Merge `<branch>` into current branch |
| `git merge --no-ff <branch>` | No-FF | Create a merge commit even if fast-forward possible |
| `git merge --squash <branch>` | Squash Merge | Combine changes from `<branch>` without creating a merge commit |
| `git mergetool` | Tool | Launch merge tool for conflict resolution |
| `git log --merges` | Merges Log | Show only merge commits |

---

## Overview

Merging combines histories. Use merging to preserve true historical merges; use `--no-ff` for explicit merge commits and `--squash` when you want a single commit.

---

## Examples

Simple merge

```bash
git switch main
git fetch origin
git merge origin/feature-A
```

Create an explicit merge commit

```bash
git merge --no-ff feature-A -m "Merge feature-A into main"
```

Squash merge locally (then commit)

```bash
git merge --squash feature-A
git commit -m "Squashed feature-A"
```

Resolve conflicts

```bash
# after git merge shows conflicts
# open files, fix conflicts
git add <resolved-files>
git commit  # finalize merge commit
```

---

## Debugging & Edge Cases

- **Conflicts:** Resolve manually or use `git mergetool`. After resolving, `git add` then `git commit` completes the merge.
- **Accidental merge commit:** If you haven't pushed, `git reset --hard HEAD~1` will undo the merge (destructive).
- **Merge vs Rebase:** Merge preserves exact history; rebase creates a linear history. Use based on team convention.
- **Octopus merge:** `git merge a b c` can merge multiple branches at once if no conflicts.

---

## How-tos

Abort an in-progress merge

```bash
git merge --abort
```

Use a GUI merge tool

```bash
git mergetool
# configure with git config --global merge.tool <tool>
```
