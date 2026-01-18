# Git Performance Cheat Sheet

Quick reference table

| Area | Command / Tool | Purpose |
| :--- | :--- | :--- |
| Shallow clone | `git clone --depth 1` | Clone fewer commits to reduce time and disk
| Sparse-checkout | `git sparse-checkout` | Check out only part of the repo working tree
| Large files | `git lfs` | Store large binaries outside normal object store
| History rewrite | `git filter-repo` | Fast, safe history rewriting to remove large files
| Pack optimization | `git gc --aggressive` | Optimize packfiles (slow, CPU/memory heavy)

---

## Overview

Big repositories and many history objects can slow Git. Use shallow clones for CI, sparse checkouts for monorepos, and `git lfs` for large files. Rewriting history with `git filter-repo` and pruning unused objects helps reclaim space.

---

## Examples

Shallow clone for CI

```bash
git clone --depth 1 --branch main git@github.com:owner/repo.git
```

Use sparse-checkout to work on a subdirectory

```bash
git clone --no-checkout git@github.com:owner/repo.git
git -C repo sparse-checkout init --cone
git -C repo sparse-checkout set path/to/project
git -C repo checkout
```

Set up Git LFS for large assets

```bash
git lfs install
git lfs track "*.psd"
git add .gitattributes
git commit -m "Enable LFS for PSDs"
```

Rewrite history to remove a large file (use backup)

```bash
# recommended: install git-filter-repo separately
git clone --mirror git@github.com:owner/repo.git
cd repo.git
git filter-repo --path path/to/bigfile --invert-paths
# push rewritten history (coordinate with team)
git push --force --all
```

---

## Debugging & Edge Cases

- **Shallow clones limit commands**: Some commands (e.g., full `git log`) behave differently on shallow clones; deepen with `git fetch --unshallow` as needed.
- **LFS storage limits**: LFS may have storage/bandwidth limits on hosting services; check quotas.
- **Filter-repo caution**: History rewriting is disruptive — coordinate and provide migration instructions.
- **CI and submodules**: Sparse checkouts and submodules can interact unexpectedly in CI; test workflows thoroughly.

---

## How-tos

Free up disk space locally

```bash
git reflog expire --expire=now --all
git gc --prune=now --aggressive
```

Use sparse-checkout for a monorepo

```bash
git clone --no-checkout <repo>
cd <repo>
git sparse-checkout init --cone
git sparse-checkout set <subdir>
git checkout <branch>
```

Monitor large objects

```bash
# list large objects in history (use git-sizer or custom script)
git rev-list --objects --all \
  | git cat-file --batch-check='%(objecttype) %(objectname) %(objectsize) %(rest)' \
  | sed -n 's/^blob //p' \
  | sort -n -k2 | tail -n 20
```
