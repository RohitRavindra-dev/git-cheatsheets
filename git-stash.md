# Git Stash Cheat Sheet 🛠️

A guide to temporarily shelving changes and managing your "work-in-progress" (WIP) stack.

---

## 📥 Saving Changes (Stashing)

| Command | Usage | Description |
| :--- | :--- | :--- |
| `git stash` | Quick Save | Stashes tracked modifications (modified & staged files). |
| `git stash push -m "msg"` | **Recommended** | Saves stash with a descriptive label for easy identification. |
| `git stash -u` | Untracked Files | Includes new files that haven't been added to Git yet. |
| `git stash -a` | All Files | Stashes everything, including ignored files (from `.gitignore`). |
| `git stash -p` | Partial Stash | Interactive mode; choose specific "hunks" of code to stash. |

---

## 📤 Retrieving Changes (Unstashing)

| Command | Usage | Description |
| :--- | :--- | :--- |
| `git stash pop` | Apply & Delete | Applies the latest stash and removes it from the stack. |
| `git stash apply` | Apply & Keep | Applies changes but keeps the stash saved (safer). |
| `git stash apply n` | Specific Stash | Applies a specific stash (e.g., `git stash apply 2`). |
| `git stash pop --index` | Keep Staging | Restores the "staged" status of files as they were. |

---

## 📋 Managing the Stack

| Command | Usage | Description |
| :--- | :--- | :--- |
| `git stash list` | View All | Shows all saved stashes in the stack (`stash@{0}`, etc.). |
| `git stash show -p` | View Diff | Displays the actual code changes inside the latest stash. |
| `git stash drop n` | Delete One | Deletes a specific stash from the stack. |
| `git stash clear` | Delete All | **Caution:** Wipes your entire stash history. |
| `git stash branch <name>` | Recover to Branch | Creates a new branch and pops the stash there (avoids conflicts). |

---

## 💡 Pro-Tips

* **Global Stack:** Stashes are **not** branch-specific. You can stash on `feature-A` and pop on `feature-B`.
* **Conflict Handling:** If `git stash pop` causes a conflict, Git will **not** delete the stash. You must resolve conflicts, then use `git stash drop` manually.
* **Safety First:** Use `git stash apply` if you are worried about merge conflicts; it ensures you don't lose the stash data if the merge gets messy.
