# Git Rebase Cheat Sheet

Quick reference table

| Command | Short | Description |
| :--- | :---: | :--- |
| `git rebase <branch>` | Rebase | Replay commits of current branch onto `<branch>` |
| `git rebase -i <base>` | Interactive | Edit, squash, reorder, or fixup commits |
| `git rebase --onto <newbase> <upstream> <branch>` | Advanced | Move a range of commits onto a new base |
| `git rebase --continue` | Continue | Continue after resolving conflicts during rebase |
| `git rebase --abort` | Abort | Abort the rebase and return to previous state |
| `git rebase --skip` | Skip | Skip the current patch during rebase |
| `git pull --rebase` | Pull Rebase | Fetch then rebase local changes onto remote branch |

---

## Overview

Rebase rewrites history by reapplying commits from one branch onto another. It creates a linear history and is useful for keeping feature branches up to date.

---

## Examples

Update feature branch onto latest main

```bash
git fetch origin
git switch feature-A
git rebase origin/main
# resolve conflicts if any
git rebase --continue
```

Interactive rebase to squash and edit

```bash
git switch feature-A
git rebase -i HEAD~5
# in editor: mark commits as pick/squash/reword
```

Rebase a range of commits onto another branch

```bash
git rebase --onto release v1.2 feature-A
```

---

## Debugging & Edge Cases

- **Conflicts during rebase:** Fix the conflict in files, `git add` them, then `git rebase --continue`. If you want to stop, run `git rebase --abort`.
- **Lost changes after rebase:** Use `git reflog` to find original commits and restore them if necessary.
- **Rebasing public/shared branches:** Avoid rebasing branches that others use; prefer merges or coordinate force-push carefully.
- **Preserve merges:** `git rebase` flattens merge commits; use `git rebase --rebase-merges` to preserve them.

---

## How-tos

Rebase interactively to clean commits before pushing

```bash
git rebase -i origin/main
# squash and edit commits, then force-push with lease
git push --force-with-lease
```

Recover after a bad rebase

```bash
git reflog
# find commit before rebase
git switch -c recover <commit>
```
