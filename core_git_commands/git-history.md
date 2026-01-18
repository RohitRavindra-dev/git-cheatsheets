# Git History Cheat Sheet

Quick reference table

| Command | Short | Description |
| :--- | :---: | :--- |
| `git log` | Log | Show commit history (many format options) |
| `git log --oneline --graph --decorate` | One-line graph | Compact visual history |
| `git reflog` | Reflog | Show local HEAD and branch movements (recover lost commits) |
| `git blame <file>` | Blame | Show commit and author per line |
| `git bisect start` | Bisect | Binary search to find commit that introduced a bug |
| `git show <commit>` | Show | Show commit details and diff |

---

## Overview

Commands to explore and recover history. `git log` is the primary tool for history, `git reflog` helps recover lost commits, `git blame` finds who changed a line, and `git bisect` helps find introducing commits via binary search.

---

## Examples

Compact graph of recent commits

```bash
git log --oneline --graph --decorate --all -n 50
```

Find when a line last changed

```bash
git blame -L 120,140 -- path/to/file
# use -p for porcelain output or -L to limit lines
```

Bisecting a bug

```bash
git bisect start
git bisect bad               # current commit is bad
git bisect good v2.3.0       # known-good commit
# Git will checkout a mid commit; run tests, then
# if it's bad:
git bisect bad
# if good:
git bisect good
# repeat until culprit is found
git bisect reset
```

Recover a lost commit via reflog

```bash
git reflog
# find SHA of lost commit
git switch -c recovered <sha>
```

---

## Debugging & Edge Cases

- **Reflog is local**: reflog entries exist only in your local repository; they are not pushed to remotes.
- **Bisect with build/test automation**: Use `git bisect run <script>` to automate testing each step.
- **Blame noise**: Use `git blame --ignore-rev` or `--incremental` to ignore refactors or massive reformatting commits.
- **Large histories**: Use `--since`, `--author`, or `--grep` filters on `git log` to find relevant commits quickly.

---

## How-tos

Search commit messages for a keyword

```bash
git log --all --grep="fix memory leak" --oneline
```

Automate bisect with a test script

```bash
# script should exit 0 for good, non-zero for bad
git bisect start
git bisect bad
git bisect good v2.3.0
git bisect run ./test-script.sh
```

Create a branch at a reflog entry

```bash
git reflog
git switch -c restore-branch <sha>
```
