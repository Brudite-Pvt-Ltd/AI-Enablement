# Common Git Issues and Solutions

Troubleshoot common problems you might encounter when using Git.

## Issue 1: "fatal: not a git repository"

**Cause**: You're not in a Git repository directory or Git is not installed.

**Solutions**:
- Verify you're in the correct directory: `pwd` (macOS/Linux) or `cd` (Windows)
- Navigate to your repository: `cd /path/to/repository`
- Initialize a new repository: `git init`
- Clone a repository: `git clone git@github.com:username/repository.git`

---

## Issue 2: SSH Key Permission Denied

**Cause**: SSH key not properly configured or added to GitHub.

**Solutions**:
1. Verify SSH connection:
   ```bash
   ssh -T git@github.com
   ```

2. Check if SSH key exists:
   ```bash
   ls -la ~/.ssh/  # macOS/Linux
   dir C:\Users\YourUsername\.ssh\  # Windows
   ```

3. Generate new SSH key (see SSH Keys section)

4. Ensure public key is added to GitHub settings

5. Check SSH key permissions:
   ```bash
   chmod 600 ~/.ssh/id_ed25519
   chmod 644 ~/.ssh/id_ed25519.pub
   ```

6. Verify SSH agent is running and key is added:
   ```bash
   ssh-add -l
   ```
   If not listed, add it:
   ```bash
   ssh-add ~/.ssh/id_ed25519
   ```

---

## Issue 3: "git config" Not Being Applied

**Cause**: Using local config when global is intended (or vice versa), or changes not saved properly.

**Solutions**:

1. **Check which config is being used**:
   ```bash
   git config --list
   ```

2. **View where config comes from**:
   ```bash
   git config --list --show-origin
   ```

3. **Set global config correctly**:
   ```bash
   git config --global user.name "Your Name"
   git config --global user.email "your@email.com"
   ```

4. **Verify global config is saved**:
   ```bash
   cat ~/.gitconfig  # macOS/Linux
   type C:\Users\YourUsername\.gitconfig  # Windows
   ```

5. **Reset to defaults if needed**:
   ```bash
   git config --global --unset user.name
   git config --global --unset user.email
   ```

---

## Issue 4: "fatal: destination path 'repository' already exists"

**Cause**: You're trying to clone into a directory that already exists.

**Solutions**:

1. **Clone into a different directory**:
   ```bash
   git clone git@github.com:username/repo.git my-custom-folder
   ```

2. **Remove existing directory and clone**:
   ```bash
   rm -rf repository
   git clone git@github.com:username/repository.git
   ```

3. **Clone into current directory**:
   ```bash
   git clone git@github.com:username/repository.git .
   ```
   (Note the dot at the end)

---

## Issue 5: Merge Conflicts

**Cause**: Two branches have conflicting changes in the same file.

**Solutions**:

1. **Identify conflicting files**:
   ```bash
   git status
   ```

2. **Open the conflicting file**
   - Look for conflict markers:
   ```
   <<<<<<< HEAD
   Your changes
   =======
   Their changes
   >>>>>>> branch-name
   ```

3. **Resolve conflicts manually**:
   - Edit the file to keep what you want
   - Remove the conflict markers
   - Choose which version to keep or combine both

4. **Stage the resolved file**:
   ```bash
   git add resolved-filename.py
   ```

5. **Complete the merge**:
   ```bash
   git commit -m "Resolve merge conflicts"
   ```

6. **Abort if needed**:
   ```bash
   git merge --abort
   ```

---

## Issue 6: Accidentally Committed to Wrong Branch

**Cause**: You committed changes to the main branch instead of a feature branch.

**Solutions**:

1. **Undo the commit (keep changes)**:
   ```bash
   git reset --soft HEAD~1
   ```

2. **Create the correct branch**:
   ```bash
   git checkout -b feature/correct-branch
   ```

3. **Commit to the correct branch**:
   ```bash
   git commit -m "Your commit message"
   ```

4. **Push to the correct branch**:
   ```bash
   git push origin feature/correct-branch
   ```

---

## Issue 7: "fatal: pathspec 'filename' did not match any files"

**Cause**: File doesn't exist or incorrect path used.

**Solutions**:

1. **Check if file exists**:
   ```bash
   ls -la filename.py  # macOS/Linux
   dir filename.py     # Windows
   ```

2. **Check file path**:
   ```bash
   git status  # Shows all tracked and untracked files
   ```

3. **Use correct path**:
   ```bash
   git add path/to/filename.py
   ```

4. **Add all files instead**:
   ```bash
   git add .
   ```

---

## Issue 8: "The file will have its original line endings in your working directory"

**Cause**: Line ending differences between Windows and Unix systems.

**Solutions**:

1. **Configure line ending handling globally**:
   ```bash
   # Windows
   git config --global core.autocrlf true
   
   # macOS/Linux
   git config --global core.autocrlf false
   ```

2. **For an existing repository**:
   ```bash
   git add -A
   git commit -m "Normalize line endings"
   ```

3. **Create a .gitattributes file**:
   ```
   * text=auto
   *.py text eol=lf
   *.js text eol=lf
   *.md text eol=lf
   ```

---

## Issue 9: "error: Your local changes to X would be overwritten by merge"

**Cause**: You have uncommitted changes that would conflict with a pull.

**Solutions**:

1. **Commit your changes**:
   ```bash
   git add .
   git commit -m "Work in progress"
   ```

2. **Stash your changes** (if you're not ready to commit):
   ```bash
   git stash
   git pull
   git stash pop
   ```

3. **Discard changes** (if you don't need them):
   ```bash
   git reset --hard HEAD
   git pull
   ```

---

## Issue 10: "fatal: You are not currently on a branch"

**Cause**: You're in a detached HEAD state.

**Solutions**:

1. **Check current status**:
   ```bash
   git status
   ```

2. **Return to a branch**:
   ```bash
   git checkout main
   ```

3. **Create a new branch from detached state**:
   ```bash
   git checkout -b new-branch-name
   ```

---

## Issue 11: Accidentally Deleted a Branch

**Cause**: You deleted a local branch with uncommitted changes.

**Solutions**:

1. **Find the branch in reflog**:
   ```bash
   git reflog
   ```

2. **Recreate the branch**:
   ```bash
   git checkout -b branch-name commit-hash
   ```

---

## Issue 12: Large Files Tracked in Git

**Cause**: Accidentally committing large files (videos, binaries, etc.).

**Solutions**:

1. **For future commits, add to .gitignore**:
   ```
   *.mp4
   *.zip
   *.bin
   ```

2. **Remove file from history** (advanced):
   ```bash
   git filter-branch --tree-filter 'rm -f large-file.mp4' HEAD
   ```

3. **Or use Git LFS** (Git Large File Storage):
   ```bash
   git lfs install
   git lfs track "*.mp4"
   git add .gitattributes
   ```

---

## Issue 13: "fatal: cannot exec 'git-*': Permission denied"

**Cause**: Git executable doesn't have proper permissions.

**Solutions**:

1. **Reinstall Git** with proper permissions
2. **Check Git installation**:
   ```bash
   which git  # macOS/Linux
   where git  # Windows
   ```

3. **Verify Git is in PATH**:
   ```bash
   git --version
   ```

---

## Issue 14: Multiple SSH Keys

**Cause**: Having multiple SSH keys and Git doesn't know which one to use.

**Solutions**:

1. **Create SSH config file** (`~/.ssh/config`):
   ```
   Host github.com
     HostName github.com
     User git
     IdentityFile ~/.ssh/id_ed25519
   
   Host github-work
     HostName github.com
     User git
     IdentityFile ~/.ssh/id_rsa_work
   ```

2. **Use different SSH keys for different repositories**:
   ```bash
   git clone git@github-work:username/repo.git
   ```

---

## Issue 15: "fatal: the remote end hung up unexpectedly"

**Cause**: Network issues or server problems.

**Solutions**:

1. **Try again**:
   ```bash
   git push
   ```

2. **Check network connection**:
   ```bash
   ping github.com
   ```

3. **Increase timeout**:
   ```bash
   git config --global http.postBuffer 157286400
   git config --global http.lowSpeedLimit 0
   git config --global http.lowSpeedTime 999999
   ```

4. **Use SSH instead of HTTPS** (more reliable)

---

## Prevention Tips

1. **Always pull before pushing** to avoid conflicts
2. **Commit frequently** with meaningful messages
3. **Use branches** for new features
4. **Review changes** before committing (`git diff`)
5. **Keep .gitignore updated** with files to exclude
6. **Back up important repositories** in multiple locations
