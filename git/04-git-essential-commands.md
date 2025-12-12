# Essential Git Commands

Master the most important Git commands for daily development work.

## Branch Management Commands

### List All Branches

**Local branches**:
```bash
git branch
```

**Remote branches**:
```bash
git branch -r
```

**All branches (local and remote)**:
```bash
git branch -a
```

### Create a New Branch

```bash
git branch branch-name
```

Or create and switch in one command:
```bash
git checkout -b branch-name
```

Or (newer syntax):
```bash
git switch -c branch-name
```

### Switch to a Branch

```bash
git checkout branch-name
```

Or (newer syntax):
```bash
git switch branch-name
```

### Delete a Branch

**Delete local branch**:
```bash
git branch -d branch-name
```

**Force delete local branch**:
```bash
git branch -D branch-name
```

**Delete remote branch**:
```bash
git push origin --delete branch-name
```

### Rename a Branch

```bash
git branch -m old-branch-name new-branch-name
```

## Viewing History

### View Commit History

```bash
git log
```

**View last N commits**:
```bash
git log -n 5
```

**View in one-line format**:
```bash
git log --oneline
```

**View with graph visualization**:
```bash
git log --oneline --graph --all
```

**View commits by specific author**:
```bash
git log --author="John Doe"
```

**View commits in a date range**:
```bash
git log --since="2 weeks ago" --until="1 week ago"
```

### View Changes in a Commit

```bash
git show commit-hash
```

### View Changes to a File

```bash
git log -p filename.py
```

## Undoing Changes

### Discard Changes in Working Directory

```bash
git checkout -- filename.py
```

Or (newer syntax):
```bash
git restore filename.py
```

### Unstage a File

```bash
git reset filename.py
```

Or:
```bash
git restore --staged filename.py
```

### Undo Last Commit (Keep Changes)

```bash
git reset --soft HEAD~1
```

This removes the commit but keeps your changes staged.

### Undo Last Commit (Discard Changes)

```bash
git reset --hard HEAD~1
```

**Warning**: This permanently deletes changes. Use with caution!

### Undo Specific Commit (Create New Commit)

```bash
git revert commit-hash
```

This creates a new commit that reverses the changes, keeping history intact.

## Comparing Changes

### Compare Working Directory with Staging Area

```bash
git diff
```

### Compare Staging Area with Last Commit

```bash
git diff --staged
```

### Compare Two Commits

```bash
git diff commit-hash-1 commit-hash-2
```

### Compare Two Branches

```bash
git diff branch-1 branch-2
```

## Stashing Changes

Temporarily save changes without committing.

### Stash Current Changes

```bash
git stash
```

### List All Stashes

```bash
git stash list
```

### Restore Stashed Changes

```bash
git stash pop
```

Or to apply without removing from stash:
```bash
git stash apply
```

### Delete Specific Stash

```bash
git stash drop stash@{0}
```

## Merging Branches

### Merge Another Branch into Current Branch

```bash
git merge branch-name
```

### Abort Merge (If Conflicts Occur)

```bash
git merge --abort
```

## Rebasing (Advanced)

### Rebase Current Branch onto Another Branch

```bash
git rebase branch-name
```

**Note**: Only use rebase on local branches you haven't shared. For shared branches, use merge.

## Remote Operations

### View Remote Repositories

```bash
git remote -v
```

Shows all remote repositories and their URLs.

### Add a Remote Repository

```bash
git remote add origin git@github.com:username/repository.git
```

### Remove a Remote Repository

```bash
git remote remove origin
```

### Rename a Remote

```bash
git remote rename origin upstream
```

## Tags

Tags mark specific commits as important (versions, releases, etc.).

### Create a Tag

```bash
git tag v1.0.0
```

With a message:
```bash
git tag -a v1.0.0 -m "Version 1.0.0 released"
```

### List Tags

```bash
git tag
```

### Push Tags to Remote

```bash
git push origin v1.0.0
```

Push all tags:
```bash
git push origin --tags
```

### Delete a Tag

**Local**:
```bash
git tag -d v1.0.0
```

**Remote**:
```bash
git push origin --delete v1.0.0
```

## Useful Command Combinations

### View Recent Activity

```bash
git log --oneline --graph --all --decorate
```

### Clean Up Local Branches

```bash
git branch -d $(git branch --merged)
```

### Find a Commit That Broke Something

```bash
git bisect start
git bisect bad
git bisect good v1.0.0
```

### Search Commit History

```bash
git log -S "search-term"
```

### Alias Shortcuts

Create shortcuts for commonly used commands:

```bash
git config --global alias.co "checkout"
git config --global alias.br "branch"
git config --global alias.ci "commit"
git config --global alias.st "status"
git config --global alias.unstage "reset HEAD --"
git config --global alias.last "log -1 HEAD"
git config --global alias.visual "log --oneline --graph --all"
```

Then use:
```bash
git co feature-branch     # Instead of git checkout feature-branch
git br -a                  # Instead of git branch -a
git visual                 # Instead of git log --oneline --graph --all
```

## Command Quick Reference

| Task | Command |
|------|---------|
| Check status | `git status` |
| Stage files | `git add .` |
| Commit | `git commit -m "message"` |
| Push | `git push` |
| Pull | `git pull` |
| Create branch | `git checkout -b branch-name` |
| Switch branch | `git checkout branch-name` |
| View log | `git log --oneline` |
| Undo changes | `git checkout -- filename` |
| Stash changes | `git stash` |
| Merge branches | `git merge branch-name` |
