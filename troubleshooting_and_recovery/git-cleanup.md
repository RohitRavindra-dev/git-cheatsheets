# Git Cleanup Cheat Sheet

Quick reference table

| Command | Short | Description |
| :--- | :---: | :--- |
| `git gc` | GC | Run garbage collection to optimize repo |
| `git gc --aggressive` | Aggressive GC | More thorough, slower GC |
| `git prune` | Prune | Remove unreachable objects (usually called by gc) |
| `git remote prune origin` | Remote Prune | Remove remote-tracking refs that no longer exist on remote |
| `git fetch --prune` | Fetch Prune | Fetch and remove deleted remote branches locally |
| `git clean -fd` | Clean | Remove untracked files and directories (use carefully) |
| `git reflog expire --expire=now --all` | Expire Reflog | Expire reflog entries (precedes aggressive garbage collection) |
| `git filter-repo` | Filter Repo | Replace old `filter-branch`; rewrite history to remove files (recommended)

---

## Overview

Cleanup operations reclaim space, remove stale references, and tidy the repo. Be careful—many cleanup commands are destructive if misused. Prefer backups before history-rewriting operations.

---

## Examples

Prune and garbage collect safely

```bash
git fetch --prune
# then
git gc --prune=now
```

Remove local branches that have been deleted on remote

```bash
# show remote-deleted branches
git branch -vv | awk '/: gone]/{print $1}'
# delete them
git remote prune origin
```

Remove untracked files and directories

```bash
# Dry run first
git clean -nd
# then actual remove
git clean -fd
```

Rewrite history to remove large file (use `git filter-repo`)

```bash
# recommended: use git-filter-repo (install separately)
# remove a file from all history
git filter-repo --path filename --invert-paths
```

---

## Debugging & Edge Cases

- **Accidental data loss**: If you prematurely run `git gc` or `git prune`, use `git reflog` to find recent SHAs before objects are pruned. If objects have been pruned, recovery is difficult.
- **Large repo performance**: `git gc --aggressive` can help but will take longer and may use more memory. Consider packed refs and shallow clones for CI to speed up operations.
- **Removing sensitive data**: Use `git filter-repo` or BFG to remove secrets from history, then force-push to rewrite remote history—coordinate with team and rotate compromised secrets.
- **Shallow clones**: Be careful when running cleanup on shallow repos — some operations behave differently.

---

## How-tos

Safely clean up remote-tracking branches

```bash
git fetch --prune
git remote prune origin
```

Free disk space and optimize local repo

```bash
git reflog expire --expire=now --all
git gc --prune=now --aggressive
```

Remove large files from history and rewrite remote

1. Back up repository (mirror clone).
2. Use `git filter-repo` to remove files.
3. Force-push rewritten branches: `git push --force --all` and `git push --force --tags`.
4. Inform team to reclone or rebase onto rewritten history.

