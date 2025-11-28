# Git Best Practices & Core Commands

A comprehensive guide to Git version control and best practices for software development.

## Table of Contents
1. [Initializing Git](#initializing-git)
2. [Core Git Commands](#core-git-commands)
3. [Best Practices](#best-practices)
4. [Branching Strategy](#branching-strategy)
5. [Commit Guidelines](#commit-guidelines)

---

## Initializing Git

### First-Time Setup

Before using Git, configure your identity:

```bash
# Set your name and email (global configuration)
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Check current user configuration
git config user.name              # Check current user name
git config user.email             # Check current user email
git config --global user.name     # Check global user name
git config --global user.email    # Check global user email

# Optional: Set default branch name to 'main'
git config --global init.defaultBranch main

# Optional: Set default editor
git config --global core.editor "code --wait"  # VS Code
# or
git config --global core.editor "vim"  # Vim
```

### Initialize a New Repository

```bash
# Initialize a new Git repository in current directory
git init

# Initialize with a specific branch name
git init -b main

# Clone an existing repository
git clone <repository-url>
git clone <repository-url> <directory-name>  # Clone into specific directory
```

### Check Repository Status

```bash
# Check if directory is a Git repository
git status

# View Git configuration
git config --list
git config --global --list  # Global config only
```

---

## Core Git Commands

### Basic Workflow

```bash
# Check status of working directory
git status

# View changes in files
git diff                    # Unstaged changes
git diff --staged           # Staged changes
git diff HEAD               # All changes (staged + unstaged)

# Stage files
git add <file>              # Stage specific file
git add .                   # Stage all changes
git add *.js                # Stage files matching pattern
git add -A                  # Stage all changes (including deletions)

# Commit changes
git commit -m "Your commit message"
git commit -am "Message"    # Stage and commit in one step (modified files only)

# View commit history
git log                     # Full history
git log --oneline           # Compact one-line format
git log --graph --oneline   # Visual graph representation
git log -n 5                # Last 5 commits
```

### Branching

```bash
# List branches
git branch                  # Local branches
git branch -a               # All branches (local + remote)
git branch -r               # Remote branches only

# Create and switch branches
git branch <branch-name>    # Create new branch
git checkout <branch-name> # Switch to branch
git checkout -b <branch-name>  # Create and switch in one command
git switch <branch-name>    # Modern way to switch (Git 2.23+)
git switch -c <branch-name> # Create and switch (Git 2.23+)

# Delete branches
git branch -d <branch-name>      # Delete local branch (safe)
git branch -D <branch-name>      # Force delete local branch
git push origin --delete <branch-name>  # Delete remote branch
```

### Remote Repositories

```bash
# View remotes
git remote                  # List remotes
git remote -v               # List remotes with URLs

# Add remote
git remote add origin <url>
git remote add upstream <url>  # For forks

# Fetch and pull
git fetch origin            # Download changes without merging
git fetch --all             # Fetch from all remotes
git pull origin main        # Fetch and merge
git pull                    # Pull from tracked branch

# Push changes
git push origin main        # Push to specific branch
git push -u origin main     # Push and set upstream tracking
git push                    # Push to tracked branch
git push --all              # Push all branches
```

### Undoing Changes

```bash
# Unstage files (keep changes)
git reset HEAD <file>
git restore --staged <file>  # Modern alternative

# Discard changes in working directory
git checkout -- <file>
git restore <file>           # Modern alternative

# Amend last commit
git commit --amend          # Edit last commit message
git commit --amend --no-edit  # Add changes to last commit

# Reset commits (use with caution!)
git reset --soft HEAD~1     # Undo commit, keep changes staged
git reset --mixed HEAD~1    # Undo commit, keep changes unstaged (default)
git reset --hard HEAD~1     # Undo commit, discard all changes (DANGEROUS!)

# Revert a commit (safe, creates new commit)
git revert <commit-hash>
```

### Viewing Information

```bash
# View file history
git log <file>              # Commit history for file
git log -p <file>           # History with changes
git blame <file>            # See who changed each line

# View specific commit
git show <commit-hash>      # Show commit details and changes
git show HEAD               # Show latest commit

# Search commits
git log --grep="keyword"    # Search commit messages
git log --author="name"     # Filter by author
git log --since="2 weeks ago"  # Filter by date
```

---

## Best Practices

### 1. Commit Often, Commit Small
- Make frequent, small commits rather than large, infrequent ones
- Each commit should represent a logical unit of work
- Easier to review, debug, and revert if needed

### 2. Write Meaningful Commit Messages
- Use clear, descriptive messages
- Follow conventional commit format (see below)
- Explain **what** and **why**, not just **what**

### 3. Use Branches
- Never commit directly to `main`/`master` in production codebases
- Create feature branches for new work
- Keep `main` branch stable and deployable

### 4. Review Before Committing
```bash
# Always check what you're about to commit
git status
git diff --staged
```

### 5. Keep Repository Clean
- Use `.gitignore` to exclude unnecessary files
- Don't commit:
  - Build artifacts
  - Dependencies (use package managers)
  - IDE configuration files
  - Environment variables and secrets
  - OS-specific files

### 6. Pull Before Push
```bash
# Always pull latest changes before pushing
git pull origin main
# Resolve conflicts if any
git push origin main
```

### 7. Use Meaningful Branch Names
- `feature/user-authentication`
- `bugfix/login-error`
- `hotfix/security-patch`
- `refactor/api-endpoints`

### 8. Delete/remove git
- `rm -rf .git`

---

## Branching Strategy

### Git Flow (Recommended for Teams)

```
main/master          → Production-ready code
  └── develop        → Integration branch
       ├── feature/* → New features
       ├── bugfix/*  → Bug fixes
       └── hotfix/*  → Emergency production fixes
```

### Simple Workflow (For Small Projects)

```
main                 → Production-ready code
  └── feature/*     → Feature branches
```

### Common Branch Types

- **main/master**: Production code, always deployable
- **develop**: Development integration branch
- **feature/***: New features
- **bugfix/***: Bug fixes
- **hotfix/***: Critical production fixes
- **release/***: Preparing new releases

---

## Commit Guidelines

### Conventional Commits Format

```
<type>(<scope>): <subject>

<body>

<footer>
```

### Types:
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Code style changes (formatting, etc.)
- `refactor`: Code refactoring
- `test`: Adding or updating tests
- `chore`: Maintenance tasks
- `perf`: Performance improvements
- `ci`: CI/CD changes

### Examples:

```bash
git commit -m "feat(auth): add user login functionality"
git commit -m "fix(api): resolve timeout issue in user endpoint"
git commit -m "docs(readme): update installation instructions"
git commit -m "refactor(utils): simplify date formatting function"
```

### Good Commit Messages:
✅ `feat: add user registration with email verification`
✅ `fix: resolve memory leak in image processing`
✅ `docs: update API documentation for v2.0`

### Bad Commit Messages:
❌ `update`
❌ `fix bug`
❌ `changes`
❌ `WIP`

---

## Useful Git Aliases

Add these to your `~/.gitconfig` for faster workflows:

```bash
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.cm commit
git config --global alias.unstage 'reset HEAD --'
git config --global alias.last 'log -1 HEAD'
git config --global alias.visual '!gitk'
```

---

## Common Workflows

### Starting a New Feature

```bash
# 1. Ensure you're on main and it's up to date
git checkout main
git pull origin main

# 2. Create and switch to feature branch
git checkout -b feature/new-feature

# 3. Make changes and commit
git add .
git commit -m "feat: implement new feature"

# 4. Push branch to remote
git push -u origin feature/new-feature

# 5. Create Pull Request (via GitHub/GitLab UI)
# 6. After merge, clean up
git checkout main
git pull origin main
git branch -d feature/new-feature
```

### Fixing a Bug

```bash
# 1. Create bugfix branch from main
git checkout main
git pull origin main
git checkout -b bugfix/issue-description

# 2. Fix and commit
git add .
git commit -m "fix: resolve issue description"

# 3. Push and create PR
git push -u origin bugfix/issue-description
```

### Emergency Hotfix

```bash
# 1. Create hotfix from main
git checkout main
git checkout -b hotfix/critical-fix

# 2. Fix and commit
git add .
git commit -m "hotfix: critical security patch"

# 3. Push and merge immediately
git push -u origin hotfix/critical-fix
# Merge via PR, then tag release
```

---

## Resources

- [Official Git Documentation](https://git-scm.com/doc)
- [GitHub Git Cheat Sheet](https://education.github.com/git-cheat-sheet-education.pdf)
- [Atlassian Git Tutorials](https://www.atlassian.com/git/tutorials)
- [Conventional Commits](https://www.conventionalcommits.org/)

---

## Next Steps

Once you're comfortable with Git basics, explore:
- **CI/CD Integration**: Automate testing and deployment
- **Git Hooks**: Automate tasks on commit/push
- **Advanced Merging**: Resolve complex conflicts
- **Rebasing**: Clean up commit history

