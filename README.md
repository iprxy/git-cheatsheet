# Git Cheat Sheet & Edge Cases

This cheat sheet provides:
1. **Naming conventions for commit messages** (to keep a clean and informative history).
2. **Basic Git commands** (everyday usage).
3. **Edge cases and solutions** (common tricky scenarios).
4. **Interactive/advanced Git workflows** (to refine or restructure commits).

---

## Table of Contents

1. [Git Commit Naming Conventions](#git-commit-naming-conventions)
2. [Basic Git Commands](#basic-git-commands)
3. [Edge Cases and Solutions](#edge-cases-and-solutions)
   - [Outdated Branch With Uncommitted Changes](#outdated-branch-with-uncommitted-changes)
   - [Outdated Branch With Unpushed Commits](#outdated-branch-with-unpushed-commits)
   - [Switching Branches With Uncommitted Changes](#switching-branches-with-uncommitted-changes)
   - [Reverting Changes](#reverting-changes)
   - [Recovering a Deleted Branch](#recovering-a-deleted-branch)
   - [Undoing the Last Commit](#undoing-the-last-commit)
   - [Merging and Rebasing Conflicts](#merging-and-rebasing-conflicts)
   - [Deleting a Remote Branch](#deleting-a-remote-branch)
4. [Advanced Git Workflows](#advanced-git-workflows)
   - [Interactive Rebase](#interactive-rebase)
   - [Cherry-Pick](#cherry-pick)
   - [Diff Between Branches](#diff-between-branches)

---

## Git Commit Naming Conventions

A consistent naming convention for commits helps everyone understand the purpose and scope of each change. Below are common types:

- **feat**: A new feature.  
  Example: `feat: Add user registration form`

- **fix**: A bug fix.  
  Example: `fix: Resolve crash on app startup`

- **refactor**: A code change that neither fixes a bug nor adds a feature (e.g., restructuring code for clarity).  
  Example: `refactor: Simplify API request handling`

- **style**: Changes that do not affect the meaning of the code (formatting, missing semicolons, etc.).  
  Example: `style: Apply consistent code formatting`

- **docs**: Documentation only changes.  
  Example: `docs: Update API usage examples`

- **test**: Adding or correcting tests.  
  Example: `test: Add unit tests for user service`

- **chore**: Changes to the build process or auxiliary tools (e.g., updating dependencies).  
  Example: `chore: Bump dependency versions`

- **perf**: Performance improvements.  
  Example: `perf: Optimize database queries`

- **ci**: Changes to CI configuration files and scripts.  
  Example: `ci: Add GitHub Actions workflow`

- **build**: Changes that affect the build system or external dependencies.  
  Example: `build: Update Babel config`

- **revert**: Reverting a previous commit.  
  Example: `revert: Revert "feat: Add user registration form"`

### General Guidelines
1. **Use present tense**: âAdd featureâ rather than âAdded feature.â
2. **Capitalize** the first letter.
3. **No period** at the end.
4. **Keep it short** (50â72 characters recommended).
5. If needed, add a longer description in the commit body below the short message.

---

## Basic Git Commands

### Initial Setup
```bash
git init                            # Initialize a new Git repository
git clone <repo-url>                # Clone an existing repository
git config --global user.name "Name"  # Set your username
git config --global user.email "Email" # Set your email
```

### Branching
```bash
git branch                          # List branches
git branch <branch-name>            # Create a new branch
git checkout <branch-name>          # Switch to a branch
git checkout -b <branch-name>       # Create and switch to a new branch
```

### Staging and Committing
```bash
git status                          # Show working tree status
git add <file>                      # Stage a file
git add .                           # Stage all changes
git commit -m "commit message"      # Commit staged changes
```

### Pulling and Pushing
```bash
git pull origin <branch>            # Fetch and merge changes from remote
git push origin <branch>            # Push changes to remote
```

### Merging and Rebasing
```bash
git merge <branch-name>             # Merge another branch into the current branch
git rebase <branch-name>            # Rebase the current branch onto another branch
```

### Viewing Changes (Diff)
```bash
git diff                            # Show unstaged changes
git diff --cached                   # Show staged changes
```

---

## Edge Cases and Solutions

### Outdated Branch With Uncommitted Changes

You want to update your branch with the latest changes from `main` without losing local uncommitted work.

1. **Stash your changes** (save them temporarily):
   ```bash
   git stash
   ```
2. **Pull the latest `main`**:
   ```bash
   git checkout main
   git pull origin main
   ```
3. **Switch back and merge**:
   ```bash
   git checkout <your-branch>
   git merge main
   ```
4. **Pop your stash** (reapply your changes):
   ```bash
   git stash pop
   ```
   Resolve conflicts if any.

---

### Outdated Branch With Unpushed Commits

You have commits on your branch but need to update it with the latest `main`.

1. **Update `main`**:
   ```bash
   git checkout main
   git pull origin main
   ```
2. **Rebase or merge onto your branch**:
   ```bash
   git checkout <your-branch>
   git rebase main  # or git merge main
   ```
3. **Push**:
   - If you rebased, you may need to force push:
     ```bash
     git push origin <your-branch> --force
     ```
   - If you merged, a normal push is fine:
     ```bash
     git push origin <your-branch>
     ```

---

### Switching Branches With Uncommitted Changes

You need to switch branches but have uncommitted changes you don't want to lose.

1. **Option 1: Stash changes**:
   ```bash
   git stash
   git checkout <new-branch>
   git stash pop
   ```
2. **Option 2: Make a temporary commit**:
   ```bash
   git add .
   git commit -m "WIP: temporary commit"
   git checkout <new-branch>
   ```
   Later, you can amend or squash this commit.

---

### Reverting Changes

#### Discard local changes:
```bash
git checkout -- <file>   # Revert a file to the last committed state
git reset --hard         # Discard all local changes (Dangerous!)
```

#### Unstage changes:
```bash
git reset <file>
```

---

### Recovering a Deleted Branch

1. **Find the commit SHA**:
   ```bash
   git reflog
   ```
2. **Create a new branch** from that commit:
   ```bash
   git checkout -b <restored-branch-name> <commit-SHA>
   ```

---

### Undoing the Last Commit

1. **Keep changes but uncommit**:
   ```bash
   git reset --soft HEAD~1
   ```
2. **Discard the commit and its changes**:
   ```bash
   git reset --hard HEAD~1
   ```

---

### Merging and Rebasing Conflicts

1. **View conflicts**:
   ```bash
   git status
   ```
2. **Resolve conflicts** in each file manually (look for `<<<<`, `====`, `>>>>` lines).
3. **Mark files as resolved**:
   ```bash
   git add <file>
   ```
4. **Continue**:
   - For merge:
     ```bash
     git commit
     ```
   - For rebase:
     ```bash
     git rebase --continue
     ```
5. **Abort if needed**:
   ```bash
   git merge --abort
   git rebase --abort
   ```

---

### Deleting a Remote Branch

1. **Delete the local branch**:
   ```bash
   git branch -d <branch-name>
   # or force delete if not merged:
   git branch -D <branch-name>
   ```
2. **Delete the remote branch**:
   ```bash
   git push origin --delete <branch-name>
   ```

---

## Advanced Git Workflows

### Interactive Rebase

Use interactive rebase to modify commit history (squash, edit, reorder, etc.).

```bash
git rebase -i HEAD~N
```
- `N` is the number of commits from HEAD you want to rewrite.
- Replace `pick` with `squash` (or `s`), `edit` (or `e`), etc.
- Save and follow Gitâs instructions.

---

### Cherry-Pick

Apply a specific commit to your current branch:
```bash
git checkout <target-branch>
git cherry-pick <commit-SHA>
```

---

### Diff Between Branches

Compare files or just see which files changed:
```bash
git diff <branch1> <branch2>             # Shows the actual changes
git diff --name-only <branch1> <branch2> # Shows only filenames
```

---

## Final Notes

- Always work on a **feature branch** rather than committing directly to `main`.
- **Commit often**, with clear and concise messages.
- Use **stash** or **temporary commits** to avoid losing work.
- Practice **interactive rebase** to keep your commit history clean.

Keep this cheat sheet handy, and happy coding!
