# Git Cheat Sheet

## 1. One-time setup (only first time on a new computer)

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global pull.rebase false     # "git pull" will merge (no divergent-branch error)
```

---

## 2. Start a project

### A) Repo already exists on GitHub → clone it

```bash
git clone https://github.com/USER/REPO.git
cd REPO
```

### B) New project on your computer → push to a new GitHub repo

1. Create an **empty** repo on GitHub (no README).
2. Then:

```bash
cd my-project
git init
git add .
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/USER/REPO.git
git push -u origin main
```

---

## 3. Daily work

| Command | When to use |
|---|---|
| `git status` | See what changed (use often) |
| `git add .` | Stage all changes |
| `git add file.html` | Stage one file |
| `git commit -m "message"` | Save a snapshot |
| `git pull origin main` | Get the latest code from GitHub |
| `git push origin main` | Send your commits to GitHub |

**Order to follow every time:**

```bash
git add .
git commit -m "what I changed"
git pull origin <branch>
git push origin <branch>
```

---

## 4. Branches

| Command | When to use |
|---|---|
| `git branch` | List branches (`*` = current) |
| `git switch -c feature-x` | Create a new branch and move to it |
| `git switch main` | Move to an existing branch |
| `git push -u origin feature-x` | First push of a new branch (later just `git push`) |
| `git merge main` | Bring main's latest code into your current branch |
| `git branch -d feature-x` | Delete a local branch after it's merged |
| `git branch -D feature-x` | Force-delete a local branch (unmerged commits are lost) |
| `git fetch --prune` | Remove old `origin/...` entries for branches deleted on GitHub |

> You can't delete the branch you're on. Switch to another branch first.

**Branch flow:**

```bash
git switch main
git pull origin main             # get latest main
git switch -c feature-x          # new branch for the task
# ...work, add, commit...
git push -u origin feature-x
# open a Pull Request on GitHub → merge it
git switch main
git pull origin main
git branch -d feature-x
```

---

## 5. Push rejected? ("fetch first" / "non-fast-forward")

Someone pushed before you. Pull first, then push.

```bash
git pull --no-rebase origin <branch>
git push origin <branch>
```

**"divergent branches" error on pull?** Add `--no-rebase` (or run the `pull.rebase false` setup from section 1).

---

## 6. Merge conflict

You and someone else changed the **same line**.

1. Open the file. You'll see:
   ```
   <<<<<<< HEAD
   your code
   =======
   their code
   >>>>>>> ...
   ```
2. Keep the code you want. Delete the `<<<<<<<`, `=======` and `>>>>>>>` lines.
3. Finish:
   ```bash
   git add <file>
   git commit
   git push origin <branch>
   ```

Want to cancel and go back? Run `git merge --abort`.

---

## 7. Check history

| Command | When to use |
|---|---|
| `git log --oneline` | Short list of commits |
| `git log --oneline --graph --all` | See all branches as a tree |
| `git diff` | See changes not yet staged |
| `git remote -v` | See which GitHub repo you're connected to |

---

## 8. Undo mistakes

| Command | When to use |
|---|---|
| `git restore file.html` | Throw away unsaved changes in a file |
| `git restore --staged file.html` | Un-add a file (keeps the changes) |
| `git commit --amend -m "new msg"` | Fix the last commit message (only if not pushed) |
| `git revert <commit-id>` | Undo a pushed commit safely (makes a new commit) |

---

## Rules to remember

- **Pull before you push.**
- Don't work directly on `main`. Use a branch.
- One branch per task. Delete it after it's merged.
- **Never** use `git push --force` on a shared branch.
- Run `git status` whenever you're confused.
