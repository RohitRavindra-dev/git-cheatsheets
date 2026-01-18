# Git Submodules Cheat Sheet

Quick reference table

| Command | Short | Description |
| :--- | :---: | :--- |
| `git submodule add <url> [path]` | Add | Add a submodule at path pointing to url |
| `git submodule init` | Init | Initialize local config for submodules (after clone) |
| `git submodule update` | Update | Clone/checkout submodule to recorded commit |
| `git submodule update --init --recursive` | Init+Update | Initialize and update nested submodules |
| `git submodule status` | Status | Show submodule commit recorded by superproject |
| `git submodule foreach <cmd>` | Foreach | Run a command in each submodule |
| `git submodule deinit <path>` | Deinit | Unregister submodule from local config |
| `git submodule sync` | Sync | Update local config with remote info (after URL changes)

---

## Overview

Submodules let you keep a repository as a subdirectory of another repository. The superproject records a commit SHA for the submodule; the submodule remains a separate Git repo. Use submodules when you must depend on a specific commit of another repo.

---

## Examples

Add a submodule

```bash
git submodule add git@github.com:owner/lib.git libs/lib
git commit -m "Add lib as submodule"
```

Clone a repo with submodules

```bash
git clone --recurse-submodules git@github.com:owner/repo.git
# or if already cloned
git submodule update --init --recursive
```

Update submodules to latest remote (careful)

```bash
git submodule foreach 'git fetch && git checkout main && git pull'
# then in superproject update recorded SHAs and commit
git add path/to/submodule
git commit -m "Update submodule SHAs"
```

---

## Debugging & Edge Cases

- **Detached HEAD in submodule**: Submodules are checked out to a specific commit, so `git status` may show a detached HEAD. To work inside a submodule, create a branch: `git -C path/to/submodule switch -c work`.
- **URL changes**: After the remote repo moves, update `.gitmodules`, run `git submodule sync` and `git submodule update --init`.
- **Nested submodules**: Use `--recursive` flags when initializing/updating.
- **Submodule merge conflicts**: Conflicts are usually about which commit SHA the superproject records; resolve by choosing the intended submodule commit, then `git add path` and commit.
- **Large teams and submodules**: Submodules add complexity for contributors — consider `git subtree` for simpler workflows when you need integrated history.

---

## How-tos

Remove a submodule cleanly

```bash
# remove entry from .gitmodules and commit
git submodule deinit -f path/to/submodule
rm -rf .git/modules/path/to/submodule
git rm -f path/to/submodule
git commit -m "Remove submodule"
```

Update submodule pointer to a new commit

```bash
cd path/to/submodule
git fetch origin
git checkout <commit-or-branch>
cd ../..
git add path/to/submodule
git commit -m "Bump submodule"
```
