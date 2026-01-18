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
| `git stash push <path>` | Specific Files | Stashes only the specified files or directories. |

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
| `git stash show -p n` | View Diff of Specific | Displays the actual code changes inside a specific stash (e.g., `git stash show -p 2`). |
| `git stash show` | View Summary | Shows a summary of changes in the latest stash. |
| `git stash drop n` | Delete One | Deletes a specific stash from the stack. |
| `git stash clear` | Delete All | **Caution:** Wipes your entire stash history. |
| `git stash branch <name>` | Recover to Branch | Creates a new branch and pops the stash there (avoids conflicts). |

---

## 💡 Pro-Tips

* **Global Stack:** Stashes are **not** branch-specific. You can stash on `feature-A` and pop on `feature-B`.
* **Conflict Handling:** If `git stash pop` causes a conflict, Git will **not** delete the stash. You must resolve conflicts, then use `git stash drop` manually.
* **Safety First:** Use `git stash apply` if you are worried about merge conflicts; it ensures you don't lose the stash data if the merge gets messy.

---

## 📝 Examples

### Basic Stash and Pop
```bash
# Make some changes to files
echo "new line" >> file.txt

# Stash with message
git stash push -m "Added new line to file.txt"

# Check status (should be clean)
git status

# Pop the stash
git stash pop

# Changes are back
```

### Stashing Untracked Files
```bash
# Create a new file
echo "untracked content" > newfile.txt

# Stash including untracked
git stash -u

# File is gone from working directory
ls newfile.txt  # not found

# Pop to get it back
git stash pop
```

---

## 🚨 Outliers & Edge Cases

- **Untracked Files:** `git stash` by default ignores untracked files. Use `-u` to include them, or `-a` for everything including ignored.
- **Conflicts:** If applying a stash causes conflicts, resolve them manually, then `git add` the resolved files and `git stash drop` to remove the stash.
- **Empty Stash:** If there are no changes, `git stash` will fail with "No local changes to save."
- **Stash on Clean Repo:** Same as above.
- **Multiple Stashes:** Use `git stash list` to see all, and reference by `stash@{n}`.

---

## 🛠️ How-Tos

### How to Stash Specific Files
Use `git stash push <path>`:
```bash
git stash push file1.txt file2.txt -m "Stashing specific files"
```

### How to View Stash Contents Without Applying
```bash
git stash show stash@{1}  # Shows summary
git stash show -p stash@{1}  # Shows full diff
```

### How to Recover from a Messy Stash Pop
If `git stash pop` conflicts:
1. Resolve conflicts in the files.
2. Stage the resolved files: `git add <resolved-files>`
3. Drop the stash: `git stash drop`

### How to Stash Changes Temporarily While Switching Branches
```bash
git stash push -m "Work in progress"
git checkout other-branch
# Do work on other-branch
git checkout original-branch
git stash pop
```
