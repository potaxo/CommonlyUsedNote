# UsefulGitCommands

## Status of checkout and normal
- When you checkout a branch, it will show the status of the branch.
- But when you checkout a file, it will not show the status of the file.
> This is why when you use `git branch --show-current`, you get blank. You also can't use `git push` or `git pull` in that state.
>
> It will not show the status of the file, but it will show the status of the branch.
- To getout of this state, you can use `git checkout <branch-name>` to switch back to a branch.

---

## Rollback to a specific commit
- First, in specific branch, use `git log` to find the commit hash you want to rollback to.
- Then, use `git reset --hard <commit-hash>` to rollback to that commit.
- In the end, you can use `git push origin master --force` to update the remote repository with the changes.
---

## How to amend a commit
- If you want to change the last commit message, you can use `git commit --amend -m "New commit message"`.
- If you want to add changes to the last commit, you can stage the changes and then use `git commit --amend`.
    - example:
```bash
# You committed app.js but forgot index.js
git add app.js
git commit -m "Add feature to app.js"

# Oops! Forgot index.js
git add index.js
git commit --amend
```
#### ⚠️ **Important Warning**

If you've already **pushed** your commit to a shared remote (like GitHub), and you amend it afterward, you'll need to **force push**:

```bash
git push --force
```

**BUT** — be careful:
>Force-pushing rewrites history and can cause confusion for teammates if you're collaborating.
>If you haven’t pushed yet — no problem at all!

---

## **Comparison: `git stash` vs `git commit --amend`**

| Feature                            | **`git stash`**                                                                           | **`git commit --amend`**                                                                 |
| ---------------------------------- | ----------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| **Purpose**                        | Temporarily save **uncommitted** changes so you can work on something else                | Modify the **last commit** (to add, change, or fix what is already committed)            |
| **What it saves?**                 | Uncommitted changes (working directory + staged files)                                    | Updates the contents and/or message of the most recent commit                            |
| **Does it create a commit?**       | ❌ No commit — it saves changes into a stash (like a clipboard)                            | ✅ Yes — it rewrites (modifies) the last commit                                           |
| **Typical use case**               | "I need to switch branches quickly, but I don't want to lose my current uncommitted work" | "I just made a commit, but forgot to include something — I want to fix that last commit" |
| **Does it affect commit history?** | ❌ No                                                                                      | ✅ Yes (rewrites last commit)                                                             |
| **Risky if already pushed?**       | ❌ No risk — stash is only local                                                           | ⚠️ Yes — amending and force-pushing can disrupt shared history                           |

---