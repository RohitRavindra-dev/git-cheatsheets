# Git Collaboration Cheat Sheet

Quick reference table

| Command | Short | Description |
| :--- | :---: | :--- |
| `git fetch upstream` | Fetch upstream | Fetch changes from original repo (fork workflows) |
| `git pull --rebase` | Rebase pull | Rebase your changes onto updated remote branch |
| `git push origin HEAD` | Push branch | Push current branch to your fork/remote |
| `git request-pull <start> <url> <end>` | Request Pull | Generate a pull request summary (less common) |
| `git checkout -b <branch>` | New branch | Create a feature branch locally |
| `git switch -c <branch>` | New & switch | Create and switch to a branch (modern command) |
| `git merge --no-ff <branch>` | No-FF merge | Create explicit merge commit to preserve context |
| `git push --force-with-lease` | Safe force | Force-push safely after rebase when appropriate |

---

## Overview

This cheatsheet covers common collaboration patterns used in teams: forks and PRs, branch naming, typical pull request workflows, when to rebase vs merge, and practical tips for code review and safe pushes.

---

## Recommended workflow (fork & PR)

1. Fork the upstream repository on the hosting service (GitHub/GitLab).
2. Clone your fork and add the upstream remote:

```bash
git clone git@github.com:you/repo.git
cd repo
git remote add upstream git@github.com:org/repo.git
```

3. Create a branch for your change, work, commit, and push to your fork:

```bash
git switch -c feature/issue-123
# make changes
git add .
git commit -m "Fix: description"
git push -u origin feature/issue-123
```

4. Open a PR from your fork's branch to upstream `main` (or target branch) and address review comments.

---

## Rebase vs Merge (team guidance)

- Rebase keeps history linear and is great for small feature branches and tidy history. Use `git rebase` locally, then `git push --force-with-lease` to update your branch.
- Merge preserves a true history (including merge commits). Use `git merge --no-ff` for explicit merges when you want to show that a feature branch was integrated.
- Team policy should decide which to prefer; do not rebase public branches used by others.

---

## PR & Review Tips

- Keep PRs small and focused: easier to review, faster to merge.
- Use clear commit messages and a short PR description with context, test instructions, and screenshots when applicable.
- Run the repo's CI locally (or via a container) before pushing to avoid trivial CI failures.
- Address review comments with additional commits or an interactive rebase to squash and rewrite, depending on team preference.

---

## Debugging & Edge Cases

- **Out-of-date branch:** If your PR is behind `main`, update it with either a merge or rebase:
  - Rebase: `git fetch upstream && git rebase upstream/main` then `git push --force-with-lease`.
  - Merge: `git fetch upstream && git merge upstream/main` then push normally.
- **Accidental force-push:** If you force-pushed and broke someone's work, use `git reflog` on the other machine or the remote branch history (if available) to recover lost commits.
- **Large PRs:** Break into smaller PRs or use WIP/Draft PRs to gather early feedback.

---

## How-tos

Create a branch from upstream `main` (safe forked workflow)

```bash
git fetch upstream
git switch -c feature/from-upstream upstream/main
```

Rebase local branch onto upstream main and push

```bash
git fetch upstream
git rebase upstream/main
git push --force-with-lease
```

Squash commits before merging (if desired)

```bash
git rebase -i upstream/main
# mark squash (s) for commits to combine, then push
```

Resolve a PR merge conflict locally

```bash
git fetch upstream
git checkout feature-branch
git merge upstream/main
# resolve conflicts, git add, then git commit
# push updated branch
```
