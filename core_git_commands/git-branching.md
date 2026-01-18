# Git Branching Cheat Sheet

Quick reference table

| Command | Short | Description |
| :--- | :---: | :--- |
| `git branch` | List | List local branches |
| `git branch -r` | Remote | List remote branches |
| `git branch <name>` | Create | Create a new branch locally |
| `git switch <name>` | Switch | Switch to branch (`git checkout` older) |
| `git switch -c <name>` | New+Switch | Create and switch to new branch |
| `git checkout -b <name>` | Old create | (older Git) create and switch |
| `git merge <branch>` | Merge | Merge branch into current |
| `git rebase <branch>` | Rebase | Reapply commits on top of <branch> |
| `git rebase -i <base>` | Interactive | Squash, reorder, edit commits |
| `git branch -d <name>` | Delete | Delete branch if merged |
| `git branch -D <name>` | Force Delete | Delete branch even if unmerged |
| `git push origin --delete <name>` | Delete Remote | Remove remote branch |
| `git push -u origin <name>` | Push with upstream | Push and set upstream tracking |

---

## Examples

Feature branch workflow

```bash
# create feature
git switch -c feature-A
# make commits
git push -u origin feature-A
# open PR, after merge
git switch main
git pull
git branch -d feature-A
```

Interactive rebase to squash commits

```bash
git switch feature-A
git rebase -i main
# mark 's' to squash, save and exit
```

Rebase vs Merge example

```bash
# keep history linear
git fetch origin
git rebase origin/main
# or preserve merge history
git merge origin/main
```

---

## Debugging & Outliers

- **Rebase conflicts**: Resolve conflicts, `git add` files, then `git rebase --continue`. To abort: `git rebase --abort`.
- **Accidentally rebased pushed branch**: Avoid force-push if others share the branch. If necessary, use `git push --force-with-lease` to reduce risk.
- **Detached HEAD after checkout of commit**: Create branch `git switch -c fix` to keep changes.
- **Merging large feature**: Consider using `--no-ff` to keep a merge commit for clarity: `git merge --no-ff feature-A`.

---

## How-tos

Rename a branch locally and remotely

```bash
# locally
git branch -m old-name new-name
# delete remote old name and push new
git push origin :old-name new-name
git push -u origin new-name
```

Recover branch from reflog

```bash
git reflog
git switch -c recovered <commit>
```

Create branch from stash and apply

```bash
git stash push -m "WIP"
git stash branch new-branch
```
