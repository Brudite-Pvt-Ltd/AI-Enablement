# Git and GitHub Setup Guide - File Index

This is the index for the Git and GitHub Setup Guide. The original document has been broken down into 5 smaller, focused sections for easier reading and reference.

## Files Overview

### 1. **01-git-installation-setup.md**
Getting Git installed and configured on your machine:
- **Installation methods for Windows**:
  - Official installer (recommended)
  - Chocolatey, Windows Package Manager, Scoop
  - Verification steps

- **Installation methods for macOS**:
  - Homebrew (recommended)
  - Official installer
  - MacPorts
  - Xcode Command Line Tools

- **Installation methods for Linux**:
  - Ubuntu/Debian systems
  - Fedora/RHEL/CentOS
  - Arch Linux

- **Configuring Git**:
  - Setting global user name and email
  - Repository-specific configuration
  - Viewing configuration
  - Default branch name, text editor, line endings

### 2. **02-git-ssh-keys.md**
Setting up secure SSH authentication with GitHub:
- Why SSH keys are important
- Generating SSH keys (Ed25519 or RSA)
- Adding SSH keys to GitHub
- Testing SSH connection
- SSH Agent configuration (macOS, Linux, Windows)
- SSH key troubleshooting
- SSH vs HTTPS comparison

### 3. **03-git-cloning-workflow.md**
Cloning repositories and basic daily workflow:
- **Cloning repositories**:
  - SSH vs HTTPS methods
  - Cloning specific branches
  - Cloning to specific directories
  - Cloning forks

- **Basic Git Workflow**:
  - Checking repository status
  - Staging changes
  - Committing changes
  - Pushing to GitHub
  - Fetching and pulling updates

- **Common workflow patterns**:
  - Main branch workflow
  - Feature branch workflow (recommended)
  - Multiple commits before push

- **Tips for better commits**:
  - Atomic commits
  - Meaningful commit messages
  - Amending commits

### 4. **04-git-essential-commands.md**
Essential Git commands for everyday development:
- **Branch management**:
  - Creating, switching, deleting, renaming branches
  - Listing local and remote branches

- **Viewing history**:
  - Commit history (various formats)
  - Changes in specific commits
  - Changes to specific files

- **Undoing changes**:
  - Discarding changes
  - Unstaging files
  - Undoing commits (soft/hard reset)
  - Creating reverting commits

- **Comparing changes**:
  - Working directory vs staging
  - Staging vs last commit
  - Between commits and branches

- **Stashing changes**:
  - Temporary save without committing
  - Listing, restoring, deleting stashes

- **Merging and rebasing**:
  - Merging branches
  - Handling merge conflicts
  - Rebasing (advanced)

- **Remote operations and tags**:
  - Managing remote repositories
  - Creating and managing tags
  - Useful command combinations
  - Aliases and shortcuts

### 5. **05-git-troubleshooting.md**
Solutions to 15 common Git problems:
1. "fatal: not a git repository"
2. SSH Key Permission Denied
3. "git config" Not Being Applied
4. "fatal: destination path already exists"
5. Merge Conflicts
6. Accidentally Committed to Wrong Branch
7. "fatal: pathspec did not match any files"
8. Line Ending Issues
9. "Your local changes would be overwritten"
10. "fatal: You are not currently on a branch"
11. Accidentally Deleted a Branch
12. Large Files Tracked in Git
13. "fatal: cannot exec 'git-*': Permission denied"
14. Multiple SSH Keys Management
15. "fatal: the remote end hung up unexpectedly"

Plus prevention tips.

## How to Use This Guide

### For Complete Beginners
1. Start with **01-git-installation-setup.md** - Install Git on your machine
2. Follow **02-git-ssh-keys.md** - Set up secure authentication
3. Learn from **03-git-cloning-workflow.md** - Clone your first repository
4. Reference **04-git-essential-commands.md** - Learn important commands
5. Use **05-git-troubleshooting.md** - When you encounter problems

### For Quick Reference
- **Installation help**: **01-git-installation-setup.md**
- **SSH problems**: **02-git-ssh-keys.md** or **05-git-troubleshooting.md** (Issue 2)
- **Command reference**: **04-git-essential-commands.md**
- **Workflow help**: **03-git-cloning-workflow.md**
- **Problem solving**: **05-git-troubleshooting.md**

### For Specific Tasks

| Task | File |
|------|------|
| Install Git | 01-git-installation-setup.md |
| Set up SSH | 02-git-ssh-keys.md |
| Clone a repo | 03-git-cloning-workflow.md |
| Create a branch | 04-git-essential-commands.md |
| Commit changes | 03-git-cloning-workflow.md |
| View history | 04-git-essential-commands.md |
| Fix merge conflict | 05-git-troubleshooting.md |
| Recover deleted branch | 05-git-troubleshooting.md |

## Key Concepts

### Installation
- Multiple methods available for each OS
- Verify installation with `git --version`

### Configuration
- Global settings apply to all repositories
- Local settings override global for specific repos
- Essential: name and email (for commits)

### SSH Keys
- Secure way to authenticate with GitHub
- Ed25519 is modern and recommended
- RSA 4096 for older systems
- Never share private key

### Workflow
1. Clone repository
2. Create feature branch
3. Make changes and commit
4. Push to GitHub
5. Create Pull Request (on GitHub web)

### Most Used Commands
```bash
git clone              # Copy repo to your machine
git checkout -b        # Create and switch to new branch
git add .             # Stage all changes
git commit -m ""      # Create a commit
git push              # Send commits to GitHub
git pull              # Get latest changes
git status            # Check repository status
git log --oneline     # View commit history
```

## Quick Start (5 Minutes)

1. **Install Git** (from 01-git-installation-setup.md)
   ```bash
   # macOS
   brew install git
   
   # Windows: Download installer from git-scm.com
   # Linux (Ubuntu)
   sudo apt install git
   ```

2. **Configure Git** (from 01-git-installation-setup.md)
   ```bash
   git config --global user.name "Your Name"
   git config --global user.email "your@email.com"
   ```

3. **Set up SSH** (from 02-git-ssh-keys.md)
   ```bash
   ssh-keygen -t ed25519 -C "your@email.com"
   # Add the public key to GitHub settings
   ssh -T git@github.com
   ```

4. **Clone a Repository** (from 03-git-cloning-workflow.md)
   ```bash
   git clone git@github.com:username/repository.git
   cd repository
   ```

5. **Start Working** (from 03-git-cloning-workflow.md)
   ```bash
   git checkout -b feature/my-feature
   # Make your changes...
   git add .
   git commit -m "Add my feature"
   git push origin feature/my-feature
   ```

## Common Workflow

```bash
# Start of day
git pull

# Create feature branch
git checkout -b feature/new-feature

# Make changes and commit
git add file1.py file2.py
git commit -m "Add new functionality"

# Push to GitHub
git push origin feature/new-feature

# Create Pull Request on GitHub web interface
# After review and approval, merge
# Back to main
git checkout main
git pull
```

## Troubleshooting Quick Links

- **Can't connect to GitHub**: See **02-git-ssh-keys.md**
- **Commit went to wrong branch**: See **05-git-troubleshooting.md** (Issue 6)
- **Have uncommitted changes blocking pull**: See **05-git-troubleshooting.md** (Issue 9)
- **Need to undo a commit**: See **04-git-essential-commands.md** (Undoing Changes)
- **Merge conflict**: See **05-git-troubleshooting.md** (Issue 5)

## Best Practices

✅ **DO**:
- Commit frequently with clear messages
- Create feature branches for new work
- Pull before pushing
- Use SSH instead of HTTPS
- Review changes before committing
- Keep .gitignore updated

❌ **DON'T**:
- Commit directly to main branch
- Use generic messages like "fix", "update"
- Force push to shared branches
- Store passwords in code
- Forget to pull before starting work
- Keep uncommitted changes for days

---

**Last Updated**: 2025
**Git Version**: 2.40+
**Platforms**: Windows, macOS, Linux
**Topics Covered**: Installation, SSH, Cloning, Workflows, Troubleshooting
