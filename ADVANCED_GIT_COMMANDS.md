# Advanced Git Commands

This guide explains advanced Git commands that help you manage your work more effectively. Each section includes a description and practical examples.

---

## Table of Contents

1. [git stash](#git-stash)
2. [git cherry-pick](#git-cherry-pick)
3. [git revert](#git-revert)
4. [git reset](#git-reset)

---

## git stash

`git stash` temporarily shelves (stashes) changes you've made to your working directory so you can switch context and come back to them later.

### When to use it

Use `git stash` when you need to quickly switch branches but aren't ready to commit your current changes.

### Examples

**Stash your current changes:**
```bash
git stash
```

**Stash with a descriptive message:**
```bash
git stash push -m "WIP: add login form validation"
```

**List all stashes:**
```bash
git stash list
# Output:
# stash@{0}: On main: WIP: add login form validation
# stash@{1}: On feature/signup: half-finished signup form
```

**Apply the most recent stash (keeps the stash):**
```bash
git stash apply
```

**Apply and remove the most recent stash:**
```bash
git stash pop
```

**Apply a specific stash:**
```bash
git stash apply stash@{1}
```

**Drop (delete) a specific stash:**
```bash
git stash drop stash@{0}
```

**Clear all stashes:**
```bash
git stash clear
```

---

## git cherry-pick

`git cherry-pick` applies the changes introduced by one or more existing commits onto the current branch. It lets you pick individual commits from any branch and replay them on another.

### When to use it

Use `git cherry-pick` when you want to apply a specific bug fix or feature commit from another branch without merging the entire branch.

### Examples

**Cherry-pick a single commit by its SHA:**
```bash
git cherry-pick a1b2c3d
```

**Cherry-pick multiple commits:**
```bash
git cherry-pick a1b2c3d e4f5g6h
```

**Cherry-pick a range of commits (inclusive):**
```bash
git cherry-pick a1b2c3d^..e4f5g6h
```

**Cherry-pick without automatically committing (stage the changes only):**
```bash
git cherry-pick --no-commit a1b2c3d
```

**Cherry-pick from a specific branch (its latest commit):**
```bash
git cherry-pick feature/hotfix
```

> **Tip:** If a conflict occurs during `git cherry-pick`, resolve the conflicts, then run `git cherry-pick --continue`. To abort, run `git cherry-pick --abort`.

---

## git revert

`git revert` creates a **new commit** that undoes the changes introduced by a previous commit. Unlike `git reset`, it does not alter the existing history, making it safe to use on shared/public branches.

### When to use it

Use `git revert` to undo a commit that has already been pushed to a shared branch (e.g., `main`) without rewriting history.

### Examples

**Revert the most recent commit:**
```bash
git revert HEAD
```

**Revert a specific commit by its SHA:**
```bash
git revert a1b2c3d
```

**Revert a commit without opening the editor (use default message):**
```bash
git revert --no-edit a1b2c3d
```

**Revert multiple commits (creates one revert commit per original commit):**
```bash
git revert a1b2c3d e4f5g6h
```

**Stage the revert changes without committing (for manual editing):**
```bash
git revert --no-commit a1b2c3d
# Make additional edits, then:
git commit -m "Revert: remove broken login feature"
```

> **Note:** `git revert` is the preferred way to undo changes on shared branches because it preserves the commit history.

---

## git reset

`git reset` moves the current branch pointer (HEAD) to a specified commit, and optionally modifies the staging area and working directory. It can rewrite history, so use it carefully — especially on shared branches.

### The three modes

| Mode | Effect on Staging Area | Effect on Working Directory |
|------|------------------------|------------------------------|
| `--soft` | Changes kept (staged) | Unchanged |
| `--mixed` (default) | Changes unstaged | Unchanged |
| `--hard` | Changes discarded | Changes discarded |

### When to use it

Use `git reset` to undo local commits, unstage files, or discard uncommitted changes. Avoid using `--hard` on commits that have been pushed to a shared branch.

### Examples

**Unstage a file (move it from staged back to unstaged):**
```bash
git reset HEAD file.txt
# or in Git 2.23+:
git restore --staged file.txt
```

**Undo the last commit, keeping changes staged (`--soft`):**
```bash
git reset --soft HEAD~1
```

**Undo the last commit, keeping changes unstaged (`--mixed`, default):**
```bash
git reset HEAD~1
# equivalent to:
git reset --mixed HEAD~1
```

**Undo the last 3 commits, keeping changes unstaged:**
```bash
git reset HEAD~3
```

**Undo the last commit and discard all changes (`--hard`):**
```bash
git reset --hard HEAD~1
```

**Reset to a specific commit SHA:**
```bash
git reset --hard a1b2c3d
```

> **Warning:** `git reset --hard` permanently discards uncommitted changes and the removed commits. Make sure you really want to lose that work before using it.

---

## Summary

| Command | Rewrites History? | Safe for Shared Branches? | Use Case |
|---------|-------------------|---------------------------|----------|
| `git stash` | No | Yes | Temporarily save uncommitted work |
| `git cherry-pick` | No (adds new commits) | Yes | Apply specific commits from another branch |
| `git revert` | No (adds new commits) | **Yes** | Safely undo a pushed commit |
| `git reset` | **Yes** | **No** (use with caution) | Undo local commits or unstage files |
