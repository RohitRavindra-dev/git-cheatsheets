# Git Security Cheat Sheet

Quick reference table

| Topic | Command / Tool | Purpose |
| :--- | :--- | :--- |
| Sign commits | `git commit -S -m "msg"` | GPG-sign a commit to prove authorship |
| Sign tags | `git tag -s v1.2.0 -m "msg"` | Create a signed annotated tag |
| Verify commit | `git verify-commit <sha>` | Verify a commit's GPG signature |
| Verify tag | `git tag -v v1.2.0` | Verify tag signature and payload |
| Configure signing key | `git config --global user.signingkey <keyid>` | Set GPG key to sign commits/tags |
| SSH keys | `ssh-keygen -t ed25519` | Generate a modern SSH key for Git host access |
| Credential helper | `git config --global credential.helper manager-core` | Use OS credential store (Windows/Mac/Manager-Core)
| Secret scanning | `trufflehog|git-secrets|gitleaks` | Tools to detect secrets in history and commits |
| Remove secrets | `git filter-repo` / `bfg` | Rewrite history to remove secrets (coordination required) |
| Pre-receive hooks | `pre-receive` hook | Enforce signing, block secrets, check policies on server |
| Protected branches | Host setting | Prevent force-push, require PRs, require signed commits |

---

Overview

Security in Git is multi-layered: protect credentials and keys, sign commits/tags for provenance, scan and remove secrets from history, and enforce server-side policies (protected branches, required checks, hooks). The goal is to make repositories auditable, minimize accidental secret leakage, and make destructive actions explicit and reversible.

---

Examples

Sign a commit (GPG)

```bash
# make sure user.signingkey is set
git config --global user.signingkey 0xABCDEF12
# sign a commit
git commit -S -m "Fix: important change"
```

Verify a tag signature

```bash
git tag -v v1.2.0
```

Create a signed tag and push

```bash
git tag -s v1.2.0 -m "Release v1.2.0"
git push origin v1.2.0
```

Use `git-secrets` to reject commits containing secrets (example)

```bash
# install and configure
git secrets --install
git secrets --add 'AKIA[0-9A-Z]{16}'
# pre-commit will now scan and fail when matching patterns are added
```

Scan history with `gitleaks`

```bash
gitleaks detect --source . --report report.json
```

---

Best Practices & Policies

- Use SSH keys or personal access tokens (PAT) — never embed credentials in code. Prefer `ssh` URLs or credential helpers.
- Require protected branches on the remote: disable force-push, require PR reviews, require passing CI, and optionally require signed commits.
- Sign important commits and all release tags to provide cryptographic provenance.
- Run secret-scanning as part of CI and pre-receive hooks to catch leaks before they land on protected branches.
- Keep GPG/SSH private keys secure and rotate them if compromised. Use hardware-backed keys (YubiKey) where possible.
- Limit and audit who can push to critical branches; prefer PR-based merges with required approvals.

---

How to verify signatures locally

```bash
# show commit with signature details
git show --show-signature <commit>
# verify tag
git tag -v <tag>
```

Server-side enforcement examples

- Require signed commits: host platform (GitHub/GitLab) can require verified commits or signed tags for protected branches.
- Pre-receive hook pseudocode: reject pushes containing patterns matching secret regexes or unsigned commits.

---

Removing secrets from history (coordination required)

1. Backup: `git clone --mirror <repo> ../repo-backup.git`
2. Use `git filter-repo` or `bfg` to remove files/patterns:

```bash
# example using git-filter-repo (recommended)
git filter-repo --path passwords.txt --invert-paths
# or to replace patterns, follow git-filter-repo docs
```

3. Force-push rewritten history to remotes: `git push --force --all && git push --force --tags`.
4. Notify team; everyone must reclone or follow rebase instructions. Rotate any exposed secrets immediately.

---

Credential & Key Hygiene

- Use `ssh-agent` with `ssh-add` and limit key lifetime where supported.
- For HTTPS, use OS credential managers or token-based auth rather than storing passwords.
- Avoid storing long-lived credentials in CI; use short-lived tokens or secret managers.
- Regularly review `~/.ssh/authorized_keys`, personal access tokens, and deployed secrets.

---

CI & Automation

- Run `gitleaks`/`trufflehog`/`git-secrets` in CI to scan PRs and branches.
- Fail builds on secret detection and block merge for failures.
- Store deployment credentials in secret managers (e.g., GitHub Secrets, Vault) rather than repo.

---

Recovery & Incident Response

- If a secret is committed: rotate the secret immediately (API keys, tokens), then remove it from history and force-push (coordinate with team).
- Use backups and reflogs to recover accidentally removed useful commits, but remember `reflog` is local only.
- Maintain an incident playbook that includes steps to rotate credentials, notify stakeholders, and remediate leaked secrets.

---

Notes & Further Reading

- `git-filter-repo` docs: https://github.com/newren/git-filter-repo
- `gitleaks`: https://github.com/zricethezav/gitleaks
- `git-secrets`: https://github.com/awslabs/git-secrets
- Signing with GPG and hardware tokens (YubiKey) for higher assurance

