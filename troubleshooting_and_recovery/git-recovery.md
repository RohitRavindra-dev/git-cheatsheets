# Git Recovery Cheat Sheet

Quick reference table

| Command | Short | Description |
| :--- | :---: | :--- |
| `git reflog` | Reflog | Show history of HEAD and branch movements (local only) |
| `git fsck --lost-found` | FSCK | Find dangling objects and lost commits |
| `git checkout -b <name> <sha>` | Recover Branch | Create branch at a recovered commit SHA |
| `git restore --source <commit> -- <file>` | Restore File | Restore a file from a previous commit |
| `git stash list` | Stash List | See saved stashes (may have WIP changes) |
| `git log --all --grep` | Search | Find commits by message across all refs |

---

## Overview

Lost commits, deleted branches, or destructive operations (e.g., `reset --hard`) are recoverable most of the time using `git reflog`, `git fsck`, and other tools — provided GC hasn't removed unreachable objects. This guide covers practical recovery workflows and preventative tips.

---

## Common Recovery Scenarios

1. Recover a deleted branch

```bash
git reflog
# find the SHA for the branch tip before deletion
git checkout -b recovered <sha>
```

2. Recover a lost commit after `reset --hard`

```bash
git reflog
# find the prior HEAD commit SHA
git switch -c recover <sha>
```

3. Recover a deleted file

```bash
git log -- path/to/file
# find commit that had the file
git restore --source <commit> -- path/to/file
```

4. Recover from a forced push that removed commits

- Ask other collaborators to check `git reflog` locally for the lost commits.
- If reflog is not available, check remote backups or CI artifacts that may reference commit SHAs.

---

## Using `git fsck` and lost-found

```bash
git fsck --full --unreachable --no-reflogs
# this lists dangling objects; you can inspect them with git show <sha>
# move useful objects into refs:
git show <sha>  # inspect
git checkout -b recovered <sha>
```

---

## Debugging & Edge Cases

- **Reflog is local**: Reflog entries are not pushed to remotes. Recovery is easier on the machine where the operation happened.
- **Garbage collection (GC)**: Unreachable objects may be pruned by `git gc`. If GC ran and removed objects, recovery becomes much harder. Avoid running GC until recovery attempts are finished.
- **Large rewrites**: If many commits were lost due to a force-push, coordinate with teammates and check CI logs, deployment artifacts, or forks for SHAs.
- **Stash recovery**: Stashes are recorded as refs; `git stash list` and `git stash show -p stash@{n}` can help recover WIP changes.

---

## How-tos

Recover a branch from reflog and push it back

```bash
git reflog
# find SHA of last known good commit
git switch -c restored <sha>
git push origin restored
```

Inspect dangling objects and restore

```bash
git fsck --lost-found
# inspect .git/lost-found/commit or use git show <sha>
```

Back up your repository before performing risky operations

```bash
# create a mirror clone for safety
git clone --mirror . ../repo-backup.git
```

Preventive tips

- Push branches regularly to remote backup.
- Use protected branches on the remote for important refs.
- Keep CI artifacts or deploy logs which may contain commit SHAs useful for recovery.
