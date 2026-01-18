# Git Conflicts Cheat Sheet

Quick reference table

| Command | Short | Description |
| :--- | :---: | :--- |
| `git status` | Status | Show conflicted files and state |
| `git merge --abort` | Abort Merge | Abort an in-progress merge (if possible) |
| `git rebase --abort` | Abort Rebase | Abort an in-progress rebase and return to previous state |
| `git mergetool` | Mergetool | Open configured merge tool to resolve conflicts |
| `git add <file>` | Mark Resolved | Mark a conflict as resolved (then commit) |
| `git checkout --ours <file>` | Keep Ours | Use our version for conflicted file (legacy) |
| `git checkout --theirs <file>` | Keep Theirs | Use their version for conflicted file (legacy) |
| `git restore --source=HEAD --staged <file>` | Restore | Restore staged/working copy from HEAD (modern command) |
| `git rerere` | Reuse Resolutions | Reuse recorded conflict resolutions (if enabled) |

---

## Overview

Conflicts occur when Git cannot automatically combine changes from different commits, branches, or stashes. They are normal and require human judgment. This guide shows common commands, step-by-step resolution workflows, and strategies to avoid or recover from conflicts.

---

## Common Conflict Workflows

1. During a merge

```bash
git merge feature-branch
# if conflicts appear
git status
# open conflicted files, resolve markers (<<<<<<< ======= >>>>>>>)
git add <resolved-files>
git commit   # finish merge
```

2. During a rebase

```bash
git rebase main
# if conflict:
# fix files
git add <resolved-files>
git rebase --continue
# to abort
git rebase --abort
```

3. After `git stash pop` conflict

```bash
git stash pop
# conflicts shown
# resolve as usual, stage, and then drop stash manually if needed
git add <resolved-files>
# if stash wasn't dropped, drop it
git stash drop
```

---

## Tools & Strategies

- Use a visual merge tool: `git mergetool` (configure `git config --global merge.tool meld` or similar).
- Prefer small, focused PRs to reduce conflict surface.
- Rebase frequently (locally) to keep branches small and up-to-date — but be careful with shared branches.
- Enable `rerere` (`git config --global rerere.enabled true`) to have Git remember and reuse your conflict resolutions.

---

## Advanced Conflict Resolutions

- Keep `ours` or `theirs` wholesale:

```bash
# keep current branch version (ours)
git checkout --ours path/to/file
# keep incoming version (theirs)
git checkout --theirs path/to/file
# then stage and continue
git add path/to/file
```

- If many files must be taken from one side, use a script:

```bash
# take 'theirs' for all conflicted files
git diff --name-only --diff-filter=U | xargs -r git checkout --theirs --
# stage them
git add -A
```

- To automatically use your branch's version for all conflicts:

```bash
git merge -X ours other-branch
```

Note: `-X ours` and `--ours` differ: `-X ours` is a merge strategy option that prefers our changes while still trying to merge non-conflicting hunks.

---

## Debugging & Edge Cases

- Binary files: Manual resolution is often required; consider keeping large binary files in LFS.
- Submodule conflicts: Conflicts are about which commit SHA the superproject records. Resolve by choosing the correct SHA, then `git add` the submodule path and commit.
- Re-occurring conflicts: Enable `rerere` to speed up repeated conflict resolution.
- Partial resolutions: If you accidentally staged an incomplete resolution, you can reset the file and try again:

```bash
git reset HEAD path/to/file
# edit, then git add again
```

---

## How-tos

Use a GUI/visual diff tool for complex merges

```bash
# configure
git config --global merge.tool meld
# then
git mergetool
```

Abort a merge safely

```bash
git merge --abort
```

Recover from a botched conflict resolution (before commit)

```bash
# restore file to pre-merge state
git merge --abort   # if merge is still in progress
# or reset the file to HEAD
git checkout -- path/to/file
```

Prevent merge conflicts proactively

- Keep branches small and focused.
- Pull/rebase frequently from the mainline.
- Communicate large refactors with the team.
