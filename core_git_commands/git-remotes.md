# Git Remotes Cheat Sheet

Quick reference table

| Command | Short | Description |
| :--- | :---: | :--- |
| `git remote -v` | List | Show remotes and URLs |
| `git remote add <name> <url>` | Add | Add a new remote repository |
| `git remote remove <name>` | Remove | Remove a remote |
| `git remote rename <old> <new>` | Rename | Rename a remote |
| `git fetch <remote>` | Fetch | Fetch objects and refs from remote |
| `git pull` | Pull | Fetch + merge (or rebase with --rebase)
| `git push` | Push | Push commits to remote |
| `git push -u origin <branch>` | Push Upstream | Set upstream on first push |
| `git push --force-with-lease` | Safe Force | Force push but check remote hasn't moved |
| `git clone --mirror` | Mirror | Mirror clone for backups or mirroring |
| `git remote show <name>` | Show | Show remote details and tracking branches |

---

## Examples

Set upstream and push

```bash
git switch -c feature-x
git push -u origin feature-x
```

Fetch vs Pull

```bash
# fetch only (safe)
git fetch origin
# inspect and merge manually
git log origin/main..main
# or pull to fetch and merge
git pull origin main
```

Safe force push

```bash
# after a local rebase
git push --force-with-lease origin feature-x
```

---

## Debugging & Outliers

- **Remote URL changed**: Update with `git remote set-url origin <new-url>`.
- **Authentication issues**: Check SSH keys (`ssh -T git@github.com`) or credential helpers.
- **Push rejected (non-fast-forward)**: Someone else pushed; fetch and rebase or merge before pushing.
- **Accidental force-push**: Use `git reflog` to find lost commits and recreate branch from commit.
- **Large files preventing push**: Use `git lfs` or remove large files from history (`git filter-repo` or `git filter-branch`).

---

## How-tos

Delete remote branch

```bash
git push origin --delete feature-x
```

Change remote URL

```bash
git remote set-url origin git@github.com:owner/repo.git
```

Mirror-push to another repo

```bash
# create mirror clone
git clone --mirror git@github.com:owner/repo.git
cd repo.git
# push to mirror
git push --mirror git@backup.example.com:owner/repo.git
```
