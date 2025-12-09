# Cloning Repositories and Basic Git Workflow

Learn how to clone repositories and work with Git locally.

## Cloning Repositories

Cloning creates a local copy of a remote repository on your machine.

### Prerequisites

- Git installed
- SSH key configured and added to GitHub (or use HTTPS with personal access token)
- Appropriate access permissions to the repository

### Cloning via SSH (Recommended)

1. **Find the Repository URL**
   - Go to the GitHub repository page
   - Click the green "Code" button
   - Select "SSH" tab
   - Copy the URL (e.g., `git@github.com:username/repository-name.git`)

2. **Clone the Repository**
   ```bash
   git clone git@github.com:username/repository-name.git
   ```

3. **Navigate to Repository**
   ```bash
   cd repository-name
   ```

### Cloning via HTTPS

If you don't want to use SSH, you can use HTTPS (requires personal access token):

1. **Find the Repository URL**
   - Go to the GitHub repository page
   - Click the green "Code" button
   - Select "HTTPS" tab
   - Copy the URL (e.g., `https://github.com/username/repository-name.git`)

2. **Clone the Repository**
   ```bash
   git clone https://github.com/username/repository-name.git
   ```

3. **Navigate to Repository**
   ```bash
   cd repository-name
   ```

### Cloning a Specific Branch

```bash
git clone --branch branch-name git@github.com:username/repository-name.git
```

Or:
```bash
git clone -b branch-name git@github.com:username/repository-name.git
```

### Cloning to a Specific Directory

```bash
git clone git@github.com:username/repository-name.git custom-directory-name
```

### Cloning a Fork

If you've forked a repository, you can clone your own fork:

```bash
git clone git@github.com:your-username/repository-name.git
```

## Basic Git Workflow

The typical workflow involves making changes, staging them, committing, and pushing to GitHub.

### Check Repository Status

```bash
git status
```

Shows which files are staged, modified, or untracked. Use this frequently to understand the current state of your repository.

### Stage Changes

**Stage a specific file**:
```bash
git add filename.py
```

**Stage all changes**:
```bash
git add .
```

**Stage all changes in a directory**:
```bash
git add path/to/directory/
```

### Commit Changes

```bash
git commit -m "Your commit message here"
```

**Good commit message practices**:
- Be descriptive and concise
- Use present tense: "Add feature" not "Added feature"
- Explain the "why", not just the "what"

Example:
```bash
git commit -m "Add authentication module for user login"
```

**Commit with more detailed message**:
```bash
git commit -m "Add authentication module" -m "This module handles user login and token validation using JWT"
```

### Push Changes to GitHub

**Push to default branch**:
```bash
git push
```

**Push to specific branch**:
```bash
git push origin branch-name
```

**Push all branches**:
```bash
git push --all
```

### Fetch Updates from GitHub

Fetches changes without merging them:

```bash
git fetch
```

### Pull Updates from GitHub

Fetches and automatically merges changes:

```bash
git pull
```

**Pull from specific branch**:
```bash
git pull origin branch-name
```

## A Typical Workflow Example

Here's a complete example of a typical day-to-day workflow:

```bash
# 1. Start your day - get latest changes
git pull

# 2. Create a new branch for your feature
git checkout -b feature/add-login

# 3. Make some changes to files
# (Edit files using your editor)

# 4. Check what you've changed
git status

# 5. Stage your changes
git add .

# 6. Commit your changes
git commit -m "Add login form validation"

# 7. Push to GitHub
git push origin feature/add-login

# 8. Create a Pull Request on GitHub
# (Done through GitHub web interface)
```

## Common Workflow Patterns

### Pattern 1: Working on Main Branch (Not Recommended for Teams)

```bash
git pull              # Get latest changes
# Make changes...
git add .
git commit -m "Fix bug in database query"
git push
```

### Pattern 2: Feature Branch Workflow (Recommended)

```bash
git pull              # Get latest main
git checkout -b feature/new-feature
# Make changes...
git add .
git commit -m "Add new feature"
git push origin feature/new-feature
# Create Pull Request on GitHub
```

### Pattern 3: Multiple Commits Before Push

```bash
git checkout -b feature/complex-feature
# Work session 1
git add module1.py
git commit -m "Add module 1"
# Work session 2
git add module2.py
git commit -m "Add module 2"
# Work session 3
git add module3.py
git commit -m "Add module 3"
# Push all commits together
git push origin feature/complex-feature
```

## Tips for Better Commits

### Atomic Commits

Each commit should represent one logical change:

```bash
# Bad - too many changes
git add .
git commit -m "Update everything"

# Good - focused changes
git add authentication.py
git commit -m "Add JWT token validation"

git add database.py
git commit -m "Add connection pooling"

git add ui.py
git commit -m "Fix button alignment on mobile"
```

### Meaningful Commit Messages

```bash
# Bad
git commit -m "fix stuff"
git commit -m "update"
git commit -m "changes"

# Good
git commit -m "Fix null pointer exception in user validation"
git commit -m "Update dependencies to latest versions"
git commit -m "Refactor database queries for performance"
```

### Amending the Last Commit

If you forgot to add a file or made a typo in the message:

```bash
git add forgotten-file.py
git commit --amend -m "Updated commit message"
```

**Warning**: Only amend commits you haven't pushed yet!
