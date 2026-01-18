# Git Basics Cheat Sheet

Quick reference table

| Command | Short | Description |
| :--- | :---: | :--- |
| `git init` | Init | Create a new repository in current directory |
| `git clone <url>` | Clone | Clone remote repo to local |
| `git status` | Status | Show working tree status |
| `git add <path>` | Add | Stage changes for commit |
| `git commit -m "msg"` | Commit | Record staged changes |
| `git commit --amend` | Amend | Modify last commit (local only) |
| `git log` | Log | Show commit history |
| `git diff` | Diff | Show unstaged changes |
| `git diff --staged` | Staged Diff | Show staged changes |
| `git reset --soft <commit>` | Reset Soft | Move HEAD, keep index and working tree |
| `git reset --mixed <commit>` | Reset Mixed | Move HEAD, reset index (default) |
| `git reset --hard <commit>` | Reset Hard | Move HEAD, reset index and working tree (DANGEROUS) |
| `git restore <file>` | Restore | Restore file from HEAD (Git >=2.23) |
| `git restore --staged <file>` | Unstage | Remove file from index |
| `git show <commit>` | Show | Show a commit's details |

---

## Examples

Basic workflow

```bash
# Clone and make changes
git clone git@github.com:owner/repo.git
cd repo
# edit files
git add .
git commit -m "Add feature"
# push
git push origin main
```

Amend last commit (local only)

```bash
git add forgotten-file.txt
git commit --amend --no-edit
```

Undo local changes (working tree)

```bash
# discard unstaged changes
git restore path/to/file
# unstage file
git restore --staged path/to/file
# reset everything to last commit (warning: destructive)
git reset --hard HEAD
```

View concise history

```bash
git log --oneline --graph --decorate --all
```

---

## Debugging & Outliers

- **Detached HEAD**: You are on a commit, not a branch. Create a branch to preserve work: `git switch -c my-branch`.
- **No changes to commit**: `git commit` will fail if nothing is staged; use `git add` first.
- **Recover a deleted file**: `git checkout -- path/to/file` (or `git restore` in newer Git) from a commit: `git checkout <commit> -- path/to/file`.
- **Broken repository**: `git fsck` to check integrity; `git reflog` to find lost refs.

---

## How-tos

Create an empty commit

```bash
git commit --allow-empty -m "empty: marker"
```

Split changes into multiple commits using interactive add

```bash
git add -p
```

Create a branch and push to set upstream

```bash
git switch -c feature-x
git push -u origin feature-x
```

Recover an accidentally deleted commit

```bash
# find commit in reflog
git reflog
# restore by creating a branch at that commit
git switch -c recovered <commit>
```
