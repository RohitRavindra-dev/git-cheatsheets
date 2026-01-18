# Git Tags Cheat Sheet

Quick reference table

| Command | Short | Description |
| :--- | :---: | :--- |
| `git tag` | List | List tags in local repo |
| `git tag -l 'v*'` | List pattern | Filter tags by pattern |
| `git tag <name>` | Lightweight tag | Create lightweight tag at HEAD |
| `git tag -a <name> -m "msg"` | Annotated tag | Create an annotated tag (recommended for releases) |
| `git show <tag>` | Show tag | Show tag object, commit and message |
| `git push origin <tag>` | Push tag | Push a single tag to remote |
| `git push --tags` | Push all tags | Push all local tags to remote |
| `git tag -d <tag>` | Delete local | Delete local tag |
| `git push origin :refs/tags/<tag>` | Delete remote | Remove tag from remote |
| `git tag -f <tag> <commit>` | Force move | Move an existing tag to another commit (use cautiously) |

---

## Overview

Tags mark points in history as important — commonly used for release versions. Prefer annotated tags for releases because they store metadata (tagger, date, message) and can be GPG-signed.

---

## Examples

Create annotated release tag

```bash
git tag -a v1.2.0 -m "Release v1.2.0"
# push tag to origin
git push origin v1.2.0
```

List tags sorted by version-like order (requires newer Git)

```bash
git tag --sort=-v:refname
```

Show tag metadata and the commit it points to

```bash
git show v1.2.0
```

Delete a remote tag

```bash
git tag -d v1.2.0
git push origin :refs/tags/v1.2.0
```

---

## Debugging & Edge Cases

- **Tag name conflicts**: If a tag exists remotely but not locally, fetch tags (`git fetch --tags`) or re-create carefully.
- **Non-version tags**: Use consistent naming (vMAJOR.MINOR.PATCH) for clarity and tooling compatibility.
- **Moving tags**: Moving an annotated tag requires deleting the remote tag and re-pushing the updated one — coordinate with the team.
- **Signed tags**: Create and verify GPG-signed tags: `git tag -s v1.2.0 -m "Release"` and `git tag --verify v1.2.0`.

---

## How-tos

Create and sign an annotated tag

```bash
git tag -s v1.2.0 -m "Signed release v1.2.0"
git push origin v1.2.0
```

Push new tags only (avoid pushing everything)

```bash
git push origin v1.2.0
```

Recover accidentally deleted tag

```bash
# find commit (or tag) in reflog or remote
git fetch origin --tags
git checkout -b restore-tag <commit>
git tag v1.2.0
git push origin v1.2.0
```
