# Phase 3.7 - Git & Version Control - Theory Guide

## Complete Git Reference for Test Automation Engineers (10+ Years Experience)

**Document Version:** 2.0
**Last Updated:** January 2026
**Target Audience:** Senior Test Automation Engineers
**Proficiency Level:** Intermediate to Advanced

---

## Table of Contents

1. [Introduction to Version Control Systems](#1-introduction-to-version-control-systems)
2. [Git Basics - Installation and Setup](#2-git-basics---installation-and-setup)
3. [Git Repository Fundamentals](#3-git-repository-fundamentals)
4. [Core Git Commands](#4-core-git-commands)
5. [Branching and Merging](#5-branching-and-merging)
6. [Remote Repositories](#6-remote-repositories)
7. [Pushing and Pulling Changes](#7-pushing-and-pulling-changes)
8. [Pull Requests and Code Review](#8-pull-requests-and-code-review)
9. [Resolving Merge Conflicts](#9-resolving-merge-conflicts)
10. [Git Ignore and Best Practices](#10-git-ignore-and-best-practices)
11. [Git Workflow for Test Automation Projects](#11-git-workflow-for-test-automation-projects)
12. [Advanced Git Commands](#12-advanced-git-commands)

---

## 1. Introduction to Version Control Systems

### 1.1 What is Version Control?

Version Control Systems (VCS) are software tools that help track changes to files over time. They allow multiple developers to collaborate on projects, maintain historical versions, and manage code changes effectively.

**Key Benefits:**
- **History Tracking:** Complete record of all changes
- **Collaboration:** Multiple developers working simultaneously
- **Backup & Recovery:** Revert to previous versions
- **Branching:** Parallel development streams
- **Accountability:** Who changed what and when

### 1.2 Types of Version Control Systems

#### 1.2.1 Local Version Control Systems
```
+-------------------+
|   File System     |
|                   |
|  Version 1        |
|  Version 2        |
|  Version 3        |
+-------------------+
```

**Characteristics:**
- Simple database storing file changes
- Local to a single machine
- Example: RCS (Revision Control System)
- **Limitation:** No collaboration support

#### 1.2.2 Centralized Version Control Systems (CVCS)

```
        +-------------------+
        | Central Server    |
        | (All versions)    |
        +-------------------+
                 |
    +------------+------------+
    |            |            |
+-------+    +-------+    +-------+
| Dev 1 |    | Dev 2 |    | Dev 3 |
+-------+    +-------+    +-------+
```

**Examples:** SVN (Subversion), CVS, Perforce

**Advantages:**
- Everyone knows what others are doing
- Fine-grained access control
- Simple to understand

**Disadvantages:**
- Single point of failure
- Requires network connectivity
- Slower operations (network dependent)

#### 1.2.3 Distributed Version Control Systems (DVCS)

```
+-------------------+
| Remote Server     |
| (Full History)    |
+-------------------+
         |
    +----+----+----+
    |    |    |    |
+-------+-------+-------+-------+
| Dev 1 | Dev 2 | Dev 3 | Dev 4 |
| Full  | Full  | Full  | Full  |
| Repo  | Repo  | Repo  | Repo  |
+-------+-------+-------+-------+
```

**Examples:** Git, Mercurial, Bazaar

**Advantages:**
- Full local repository with complete history
- Fast operations (local)
- Works offline
- Multiple backup locations
- Flexible workflows

**Disadvantages:**
- Steeper learning curve
- More disk space required
- Initial clone can be slow

### 1.3 Why Git for Test Automation?

**Scenario:** Test Automation Framework Development

```
Test Automation Project Structure:
├── src/
│   ├── test/
│   │   ├── java/
│   │   │   ├── tests/
│   │   │   ├── pages/
│   │   │   └── utils/
├── config/
│   ├── dev.properties
│   ├── qa.properties
│   └── prod.properties
├── test-data/
├── reports/
└── pom.xml
```

**Why Git is Essential:**

1. **Framework Evolution Tracking**
   - Track changes to page objects
   - Monitor test case additions/modifications
   - Version configuration files

2. **Team Collaboration**
   - Multiple QA engineers working on different test modules
   - Code reviews for test scripts
   - Parallel feature testing

3. **Environment Management**
   - Different branches for different environments (dev, qa, staging)
   - Feature-based test development
   - Hotfix branches for critical test issues

4. **CI/CD Integration**
   - Jenkins/GitLab CI pulls latest tests from Git
   - Automated test execution on commits
   - Version-controlled test reports

5. **Test Data Management**
   - Version control for test data files
   - Track changes to test inputs
   - Rollback to previous test datasets

### 1.4 Git vs Other VCS - Comparison

| Feature | Git | SVN | Perforce |
|---------|-----|-----|----------|
| Repository Type | Distributed | Centralized | Centralized |
| Offline Work | Yes | No | Limited |
| Branching | Fast & Easy | Slow | Moderate |
| Merging | Excellent | Basic | Good |
| Speed | Fast | Moderate | Fast |
| Learning Curve | Steep | Gentle | Moderate |
| Large Binary Files | Poor | Good | Excellent |
| Market Adoption | Very High | Moderate | Low |

**Real-World Example for Test Automation:**

```bash
# Scenario: You're on a flight working on test automation

# With Git (DVCS):
git checkout -b feature/mobile-tests
# Write test cases offline
git add src/test/java/mobile/
git commit -m "Add mobile login tests"
git log  # Review your changes
# Later, when online
git push origin feature/mobile-tests

# With SVN (CVCS):
# Cannot work effectively offline
# Need network for most operations
# Limited branching capabilities
```

### 1.5 Common Version Control Terminology

| Term | Definition | Example |
|------|------------|---------|
| **Repository (Repo)** | Storage location for your project | selenium-automation-framework |
| **Commit** | Snapshot of changes | "Added login page tests" |
| **Branch** | Parallel version of repository | feature/api-tests |
| **Merge** | Combining different branches | Merge feature → main |
| **Clone** | Copy of remote repository | git clone <url> |
| **Fork** | Personal copy of someone's repo | Fork company framework |
| **Pull Request (PR)** | Request to merge changes | PR: Add Cucumber tests |
| **Conflict** | Overlapping changes | Two devs edit same test |
| **HEAD** | Current commit reference | Points to latest commit |
| **Origin** | Default remote repository name | origin/main |
| **Staging Area (Index)** | Pre-commit holding area | Files ready to commit |
| **Working Directory** | Your local project folder | /workspace/test-automation |

### 1.6 Interview Questions - Version Control Basics

**Q1: What is the difference between Centralized and Distributed VCS?**

**Answer:**
```
Centralized VCS (SVN):
- Single central server holds complete history
- Clients check out working copies
- Requires network for most operations
- Single point of failure

Distributed VCS (Git):
- Every developer has full repository copy
- Complete history available locally
- Most operations are local and fast
- Multiple backup locations
- Better for distributed teams

Example in Test Automation:
With Git, QA engineers can work on test scripts during
network outages, commit locally, and sync later.
```

**Q2: Why is Git preferred over SVN in modern test automation projects?**

**Answer:**
```
1. Speed: Local operations are extremely fast
2. Branching: Easy feature branch creation for test modules
3. Offline Work: Write and commit tests without network
4. CI/CD Integration: Better support in modern CI tools
5. Code Review: Pull Request workflow for test reviews
6. Open Source: Large community and extensive tooling

Real Example:
Creating a branch for new test suite in Git: < 1 second
Same operation in SVN: 10-30 seconds (network dependent)
```

**Q3: Explain the concept of "distributed" in DVCS.**

**Answer:**
```
Distributed means every developer has a complete copy
of the repository including full history.

Visualization:
Central Model (SVN):    Distributed Model (Git):
     [Server]           [Server] ← Full History
        ↕                  ↕
   [Developer]         [Developer] ← Full History
   (Partial)           [Developer] ← Full History
                       [Developer] ← Full History

Benefits for Test Automation:
- Each QA engineer has full test suite history
- Can work independently without server dependency
- Experimental test changes can be made safely
- Multiple teams can work on different features
```

---

## 2. Git Basics - Installation and Setup

### 2.1 Installing Git

#### 2.1.1 Windows Installation

**Method 1: Git for Windows (Recommended)**

```bash
# Download from: https://git-scm.com/download/win
# Run the installer with these recommended settings:

1. Select Components:
   ✓ Git Bash Here
   ✓ Git GUI Here
   ✓ Associate .git* configuration files
   ✓ Associate .sh files to be run with Bash

2. Choosing the default editor:
   Select: Visual Studio Code (or your preferred editor)

3. Adjusting PATH environment:
   Select: Git from the command line and also from 3rd-party software

4. Choosing HTTPS transport backend:
   Select: Use the OpenSSL library

5. Configuring line ending conversions:
   Select: Checkout Windows-style, commit Unix-style line endings

6. Configuring terminal emulator:
   Select: Use MinTTY (default terminal of MSYS2)

7. Choose default behavior of 'git pull':
   Select: Default (fast-forward or merge)
```

**Verify Installation:**
```bash
# Open Command Prompt or Git Bash
git --version

# Expected Output:
git version 2.43.0.windows.1
```

**Method 2: Chocolatey Package Manager**

```bash
# Install Chocolatey first (PowerShell as Administrator)
Set-ExecutionPolicy Bypass -Scope Process -Force; `
[System.Net.ServicePointManager]::SecurityProtocol = `
[System.Net.ServicePointManager]::SecurityProtocol -bor 3072; `
iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))

# Install Git
choco install git -y

# Verify
git --version
```

#### 2.1.2 macOS Installation

**Method 1: Homebrew (Recommended)**

```bash
# Install Homebrew (if not installed)
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install Git
brew install git

# Verify
git --version

# Expected Output:
git version 2.43.0
```

**Method 2: Xcode Command Line Tools**

```bash
# Install Xcode Command Line Tools
xcode-select --install

# Verify
git --version
```

**Method 3: Download from Official Site**

```bash
# Download from: https://git-scm.com/download/mac
# Install the .dmg package

# Verify in Terminal
git --version
```

#### 2.1.3 Linux Installation

**Ubuntu/Debian:**

```bash
# Update package index
sudo apt update

# Install Git
sudo apt install git -y

# Verify
git --version

# Expected Output:
git version 2.43.0
```

**CentOS/RHEL/Fedora:**

```bash
# CentOS/RHEL 7
sudo yum install git -y

# CentOS/RHEL 8+/Fedora
sudo dnf install git -y

# Verify
git --version
```

**Arch Linux:**

```bash
sudo pacman -S git

# Verify
git --version
```

### 2.2 Initial Git Configuration

Git configuration works at three levels:

```
System Level (--system)
    ↓
User Level (--global)  ← Most commonly used
    ↓
Repository Level (--local)
```

#### 2.2.1 Essential Configuration

```bash
# Set your name (appears in commits)
git config --global user.name "John Doe"

# Set your email (appears in commits)
git config --global user.email "john.doe@company.com"

# Set default branch name to 'main'
git config --global init.defaultBranch main

# Set default text editor
git config --global core.editor "code --wait"  # VS Code
# OR
git config --global core.editor "vim"          # Vim
# OR
git config --global core.editor "notepad"      # Notepad (Windows)

# Enable color output
git config --global color.ui auto

# Set line ending handling (Windows)
git config --global core.autocrlf true

# Set line ending handling (macOS/Linux)
git config --global core.autocrlf input
```

**Verify Configuration:**

```bash
# View all configurations
git config --list

# Expected Output:
user.name=John Doe
user.email=john.doe@company.com
init.defaultbranch=main
core.editor=code --wait
color.ui=auto
core.autocrlf=true
```

```bash
# View specific configuration
git config user.name
# Output: John Doe

git config user.email
# Output: john.doe@company.com
```

#### 2.2.2 Advanced Configuration for Test Automation

```bash
# Set default merge strategy
git config --global merge.conflictstyle diff3

# Set up diff tool (for comparing test files)
git config --global diff.tool vscode
git config --global difftool.vscode.cmd "code --wait --diff $LOCAL $REMOTE"

# Set up merge tool (for resolving conflicts)
git config --global merge.tool vscode
git config --global mergetool.vscode.cmd "code --wait $MERGED"

# Disable fast-forward merges (cleaner history)
git config --global merge.ff false

# Set default pull behavior
git config --global pull.rebase false

# Enable credential caching (save authentication)
# Windows
git config --global credential.helper wincred

# macOS
git config --global credential.helper osxkeychain

# Linux
git config --global credential.helper cache
git config --global credential.helper 'cache --timeout=3600'

# Set up aliases (shortcuts for common commands)
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.unstage 'reset HEAD --'
git config --global alias.last 'log -1 HEAD'
git config --global alias.visual 'log --oneline --graph --decorate --all'
```

**Usage of Aliases:**

```bash
# Instead of typing full commands
git status
git checkout main
git branch

# Use aliases
git st
git co main
git br
```

#### 2.2.3 Configuration Files Location

**System Level:**
```
Windows: C:\Program Files\Git\etc\gitconfig
macOS/Linux: /etc/gitconfig
```

**User Level (Global):**
```
Windows: C:\Users\<YourName>\.gitconfig
macOS/Linux: ~/.gitconfig or ~/.config/git/config
```

**Repository Level (Local):**
```
<repository>/.git/config
```

**View Configuration File:**

```bash
# Edit global configuration
git config --global --edit

# This opens .gitconfig file in your default editor
# Example content:
[user]
    name = John Doe
    email = john.doe@company.com
[init]
    defaultBranch = main
[core]
    editor = code --wait
    autocrlf = true
[color]
    ui = auto
[alias]
    st = status
    co = checkout
    br = branch
    ci = commit
```

### 2.3 Setting Up SSH Keys (Recommended for GitHub/GitLab)

SSH keys provide secure authentication without entering passwords repeatedly.

#### 2.3.1 Generate SSH Key

```bash
# Check for existing SSH keys
ls -al ~/.ssh

# Generate new SSH key
ssh-keygen -t ed25519 -C "john.doe@company.com"

# If ed25519 not supported, use RSA
ssh-keygen -t rsa -b 4096 -C "john.doe@company.com"

# Prompts:
Enter file in which to save the key (/Users/you/.ssh/id_ed25519): [Press Enter]
Enter passphrase (empty for no passphrase): [Type passphrase or Press Enter]
Enter same passphrase again: [Type passphrase again or Press Enter]

# Output:
Your identification has been saved in /Users/you/.ssh/id_ed25519
Your public key has been saved in /Users/you/.ssh/id_ed25519.pub
The key fingerprint is:
SHA256:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx john.doe@company.com
```

#### 2.3.2 Add SSH Key to SSH Agent

```bash
# Start SSH agent in background
eval "$(ssh-agent -s)"

# Output:
Agent pid 12345

# Add SSH private key to agent
ssh-add ~/.ssh/id_ed25519

# For macOS, also add to keychain
ssh-add --apple-use-keychain ~/.ssh/id_ed25519
```

#### 2.3.3 Add SSH Key to GitHub

```bash
# Copy SSH public key to clipboard

# macOS
pbcopy < ~/.ssh/id_ed25519.pub

# Windows (Git Bash)
clip < ~/.ssh/id_ed25519.pub

# Linux
cat ~/.ssh/id_ed25519.pub
# Copy the output manually
```

**Steps on GitHub:**
1. Go to GitHub → Settings → SSH and GPG keys
2. Click "New SSH key"
3. Title: "Work Laptop - Test Automation"
4. Paste key into "Key" field
5. Click "Add SSH key"

#### 2.3.4 Test SSH Connection

```bash
# Test GitHub connection
ssh -T git@github.com

# Output:
Hi username! You've successfully authenticated, but GitHub does not provide shell access.

# Test GitLab connection
ssh -T git@gitlab.com

# Output:
Welcome to GitLab, @username!

# Test Bitbucket connection
ssh -T git@bitbucket.org

# Output:
authenticated via ssh key.
```

### 2.4 Git GUI Clients (Optional but Recommended)

#### 2.4.1 Popular Git GUI Tools

**1. GitKraken (Cross-platform)**
```
Website: https://www.gitkraken.com/
Pros:
- Beautiful interface
- Visual commit graph
- Built-in merge conflict editor
- Integrations with GitHub, GitLab, Bitbucket
- Great for understanding Git visually

Free for public repositories
```

**2. SourceTree (Windows/macOS)**
```
Website: https://www.sourcetreeapp.com/
Pros:
- Free for all repositories
- Atlassian product (integrates with Bitbucket)
- Good visualization
- Simplifies complex Git operations

Best for: Bitbucket users
```

**3. GitHub Desktop (Cross-platform)**
```
Website: https://desktop.github.com/
Pros:
- Simple and clean
- Great for beginners
- Seamless GitHub integration
- Free and open-source

Best for: GitHub users
```

**4. Git Extensions (Windows/Linux)**
```
Website: https://gitextensions.github.io/
Pros:
- Free and open-source
- Windows Shell integration
- Visual Studio integration
- Powerful features

Best for: Windows developers
```

**5. VS Code Built-in Git (Cross-platform)**
```
Built into Visual Studio Code
Pros:
- No separate installation
- Integrated with code editor
- Extensions available (GitLens, Git Graph)
- Perfect for daily use

Best for: VS Code users
```

#### 2.4.2 Recommended Setup for Test Automation Engineers

```bash
# Recommended tool combination:
1. Command Line (Git Bash/Terminal) - For daily operations
2. VS Code with GitLens extension - For code review and diffs
3. GitKraken or SourceTree - For complex operations and visualization

# Install GitLens in VS Code
code --install-extension eamodio.gitlens

# Install Git Graph in VS Code
code --install-extension mhutchie.git-graph
```

### 2.5 Practice Exercise - Initial Setup

**Exercise 1: Complete Git Installation and Configuration**

```bash
# Step 1: Verify Git installation
git --version

# Step 2: Configure user information
git config --global user.name "Your Name"
git config --global user.email "your.email@company.com"

# Step 3: Set default editor
git config --global core.editor "code --wait"

# Step 4: Configure line endings
# Windows users
git config --global core.autocrlf true
# macOS/Linux users
git config --global core.autocrlf input

# Step 5: Set up useful aliases
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.lg "log --oneline --graph --all --decorate"

# Step 6: Verify all configurations
git config --list

# Step 7: View your .gitconfig file
git config --global --edit
```

**Exercise 2: SSH Key Setup (Optional but Recommended)**

```bash
# Step 1: Generate SSH key
ssh-keygen -t ed25519 -C "your.email@company.com"

# Step 2: Start SSH agent
eval "$(ssh-agent -s)"

# Step 3: Add key to agent
ssh-add ~/.ssh/id_ed25519

# Step 4: Display public key
cat ~/.ssh/id_ed25519.pub

# Step 5: Add key to GitHub/GitLab
# (Copy the output and add to your account)

# Step 6: Test connection
ssh -T git@github.com
```

### 2.6 Common Mistakes to Avoid

**Mistake 1: Not Configuring User Information**
```bash
# Wrong: Committing without configuration
# Results in commits with incorrect author info

# Correct: Always configure before first commit
git config --global user.name "Your Name"
git config --global user.email "your.email@company.com"
```

**Mistake 2: Using Wrong Email Address**
```bash
# Wrong: Using personal email for work projects
git config --global user.email "personal@gmail.com"

# Correct: Use appropriate email for context
# For work projects
git config user.email "work@company.com"

# For personal projects
git config user.email "personal@gmail.com"
```

**Mistake 3: Ignoring Line Ending Settings**
```bash
# This causes issues in cross-platform teams

# Windows developers should use:
git config --global core.autocrlf true

# macOS/Linux developers should use:
git config --global core.autocrlf input
```

**Mistake 4: Not Setting Up SSH Keys**
```bash
# Results in: Entering password for every push/pull
# Solution: Set up SSH keys as shown in Section 2.3
```

### 2.7 Best Practices - Initial Setup

1. **Use Consistent Email Across Team**
   ```bash
   # All team members should use company email
   git config --global user.email "yourname@company.com"
   ```

2. **Set Up SSH Keys for All Remote Repositories**
   ```bash
   # Saves time and improves security
   # Follow Section 2.3 for setup
   ```

3. **Configure Merge Strategy Early**
   ```bash
   # Prevents accidental fast-forward merges
   git config --global merge.ff false
   ```

4. **Use Aliases for Efficiency**
   ```bash
   # Save time with commonly used commands
   git config --global alias.st status
   git config --global alias.co checkout
   ```

5. **Document Your Git Configuration**
   ```bash
   # Keep a backup of your .gitconfig
   cp ~/.gitconfig ~/.gitconfig.backup
   ```

### 2.8 Interview Questions - Git Setup

**Q1: What is the difference between --global and --local config?**

**Answer:**
```bash
# --global: User-level configuration
# Applies to all repositories for current user
# Location: ~/.gitconfig
git config --global user.name "John Doe"

# --local: Repository-level configuration
# Applies only to current repository
# Location: <repo>/.git/config
# Overrides global settings
git config --local user.email "john@project-specific.com"

# Example: Different email for work and personal projects
# Global (default for all repos)
git config --global user.email "personal@gmail.com"

# Local (specific work project)
cd /work/project
git config --local user.email "john@company.com"
```

**Q2: Why should we use SSH keys instead of HTTPS?**

**Answer:**
```
Benefits of SSH:
1. No password prompts for every operation
2. More secure (key-based authentication)
3. Easier automation (CI/CD pipelines)
4. Better for scripts and automation

HTTPS Use Cases:
1. Firewall restrictions (some networks block SSH port 22)
2. Quick one-time operations
3. Cloning public repositories

For test automation projects:
SSH is recommended because automated test runs
in CI/CD need passwordless authentication.
```

**Q3: How would you configure Git differently for a test automation project?**

**Answer:**
```bash
# Standard configuration
git config --global user.name "John Doe"
git config --global user.email "john@company.com"

# Test automation specific
# 1. Set up diff tool for comparing test files
git config --global diff.tool vscode

# 2. Configure merge tool for conflict resolution
git config --global merge.tool vscode

# 3. Set up aliases for test-related operations
git config --global alias.test-status "status src/test/"
git config --global alias.test-diff "diff src/test/"

# 4. Configure core.excludesfile for test artifacts
git config --global core.excludesfile ~/.gitignore_global

# In ~/.gitignore_global:
# test-output/
# reports/
# screenshots/
# *.log
# target/
# build/
```

---

## 3. Git Repository Fundamentals

### 3.1 Understanding Git Repository Structure

A Git repository is a directory that contains your project files and Git's tracking information.

```
Project Directory (selenium-framework)
│
├── .git/                    ← Git's internal directory (DO NOT MODIFY)
│   ├── config               ← Repository configuration
│   ├── description          ← Repository description
│   ├── HEAD                 ← Points to current branch
│   ├── hooks/               ← Git hooks (automation scripts)
│   ├── objects/             ← Git object database
│   ├── refs/                ← References (branches, tags)
│   └── index                ← Staging area
│
├── src/                     ← Your project files
│   ├── test/
│   └── main/
├── pom.xml
└── README.md
```

**Key Components:**

| Component | Purpose | Should Touch? |
|-----------|---------|---------------|
| .git/config | Repo settings | Yes (via git config) |
| .git/objects/ | Commits, files | No (Git manages) |
| .git/HEAD | Current branch | No (Git manages) |
| .git/index | Staging area | No (Git manages) |
| Working Directory | Your files | Yes (daily work) |

### 3.2 Initializing a New Repository (git init)

#### 3.2.1 Creating a New Repository

```bash
# Create project directory
mkdir selenium-automation-framework
cd selenium-automation-framework

# Initialize Git repository
git init

# Output:
Initialized empty Git repository in /path/to/selenium-automation-framework/.git/
```

**What Happens:**
1. Creates `.git` directory
2. Sets up repository structure
3. Creates default branch (main/master)
4. Repository is now ready to track changes

**Verify Repository Creation:**

```bash
# Check repository status
git status

# Output:
On branch main

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```

```bash
# List all files including hidden .git directory
ls -la

# Output:
total 0
drwxr-xr-x   3 user  staff    96 Jan 28 10:00 .
drwxr-xr-x  25 user  staff   800 Jan 28 10:00 ..
drwxr-xr-x  10 user  staff   320 Jan 28 10:00 .git
```

#### 3.2.2 Initialize with Specific Branch Name

```bash
# Modern approach - Initialize with 'main' as default branch
git init -b main

# Or set globally first
git config --global init.defaultBranch main
git init

# Output:
Initialized empty Git repository in /path/to/project/.git/
```

#### 3.2.3 Real-World Example - Test Automation Project Setup

```bash
# Step 1: Create project structure
mkdir selenium-cucumber-framework
cd selenium-cucumber-framework

# Step 2: Initialize Git repository
git init -b main

# Step 3: Create initial project structure
mkdir -p src/test/java/com/automation/{tests,pages,utils}
mkdir -p src/test/resources/{features,testdata}
mkdir -p config
mkdir -p reports

# Step 4: Create initial files
touch README.md
touch .gitignore
touch pom.xml

# Step 5: Check status
git status

# Output:
On branch main

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        .gitignore
        README.md
        pom.xml
        config/
        reports/
        src/

nothing added to commit but untracked files present (use "git add" to track)
```

### 3.3 Cloning an Existing Repository (git clone)

Cloning creates a complete copy of a remote repository on your local machine.

#### 3.3.1 Basic Clone Operation

```bash
# Clone using HTTPS
git clone https://github.com/username/selenium-framework.git

# Output:
Cloning into 'selenium-framework'...
remote: Enumerating objects: 127, done.
remote: Counting objects: 100% (127/127), done.
remote: Compressing objects: 100% (89/89), done.
remote: Total 127 (delta 45), reused 115 (delta 35), pack-reused 0
Receiving objects: 100% (127/127), 45.67 KiB | 1.52 MiB/s, done.
Resolving deltas: 100% (45/45), done.
```

```bash
# Clone using SSH (Recommended)
git clone git@github.com:username/selenium-framework.git

# Clone to specific directory name
git clone https://github.com/username/selenium-framework.git my-framework

# Clone specific branch
git clone -b develop https://github.com/username/selenium-framework.git

# Clone with depth (shallow clone - faster)
git clone --depth 1 https://github.com/username/selenium-framework.git
```

#### 3.3.2 What Gets Cloned?

```
Remote Repository (GitHub)          Local Clone
┌─────────────────────┐            ┌─────────────────────┐
│ All commits         │   ────────>│ All commits         │
│ All branches        │            │ All branches        │
│ All tags            │            │ All tags            │
│ Complete history    │            │ Complete history    │
│ Project files       │            │ Project files       │
└─────────────────────┘            └─────────────────────┘
```

**After cloning, you have:**
- Complete repository history
- All branches (tracked remotely)
- Default branch checked out
- Configured 'origin' remote pointing to source

#### 3.3.3 Verify Clone

```bash
# Navigate into cloned directory
cd selenium-framework

# Check remote configuration
git remote -v

# Output:
origin  https://github.com/username/selenium-framework.git (fetch)
origin  https://github.com/username/selenium-framework.git (push)
```

```bash
# Check branches
git branch -a

# Output:
* main
  remotes/origin/HEAD -> origin/main
  remotes/origin/main
  remotes/origin/develop
  remotes/origin/feature/api-tests
```

```bash
# View commit history
git log --oneline -5

# Output:
a1b2c3d (HEAD -> main, origin/main) Add API test cases
e4f5g6h Update README with setup instructions
i7j8k9l Add Page Object Model structure
m0n1o2p Initial framework setup
q3r4s5t Initial commit
```

#### 3.3.4 Real-World Scenario - Joining Test Automation Team

```bash
# Scenario: You join a new team and need to set up test framework

# Step 1: Clone the team's test automation repository
git clone git@github.com:company/test-automation-framework.git
cd test-automation-framework

# Step 2: Verify clone
git remote -v
git branch -a
git log --oneline -3

# Step 3: Check project structure
ls -la

# Output:
total 48
drwxr-xr-x  12 user  staff   384 Jan 28 10:30 .
drwxr-xr-x  25 user  staff   800 Jan 28 10:30 ..
drwxr-xr-x  13 user  staff   416 Jan 28 10:30 .git
-rw-r--r--   1 user  staff   156 Jan 28 10:30 .gitignore
-rw-r--r--   1 user  staff  2345 Jan 28 10:30 README.md
drwxr-xr-x   5 user  staff   160 Jan 28 10:30 config
-rw-r--r--   1 user  staff  5678 Jan 28 10:30 pom.xml
drwxr-xr-x   8 user  staff   256 Jan 28 10:30 src
drwxr-xr-x   4 user  staff   128 Jan 28 10:30 test-data

# Step 4: Install dependencies (Maven project)
mvn clean install

# Step 5: Create your feature branch
git checkout -b feature/login-tests

# Now ready to work on tests!
```

### 3.4 Checking Repository Status (git status)

`git status` is the most frequently used Git command - it shows the state of your working directory and staging area.

#### 3.4.1 Understanding Git Status Output

```bash
git status

# Output interpretation:
On branch main                          ← Current branch
Your branch is up to date with 'origin/main'.  ← Sync status

Changes to be committed:                ← Staged files (ready to commit)
  (use "git restore --staged <file>..." to unstage)
        new file:   src/test/LoginTest.java
        modified:   pom.xml

Changes not staged for commit:          ← Modified but not staged
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md
        modified:   config/test.properties

Untracked files:                        ← New files not tracked by Git
  (use "git add <file>..." to include in what will be committed)
        src/test/utils/TestHelper.java
        test-data/users.csv

nothing to commit, working tree clean   ← OR this if no changes
```

#### 3.4.2 File States in Git

```
Untracked  ────git add───>  Staged  ────git commit───>  Committed
    │                         │                              │
    │                         │                              │
    │<────git rm --cached────┘                              │
    │                                                         │
    └─────────────────────git reset HEAD~1──────────────────┘

Modified ────git add───> Staged (Modified) ────git commit───> Committed
    │                           │
    │                           │
    └───────git restore─────────┘
```

**File State Examples:**

```bash
# Create new file (Untracked)
echo "public class NewTest {}" > src/test/NewTest.java
git status
# Shows: Untracked files: src/test/NewTest.java

# Stage the file (Staged)
git add src/test/NewTest.java
git status
# Shows: Changes to be committed: new file: src/test/NewTest.java

# Modify staged file (Staged + Modified)
echo "// Added comment" >> src/test/NewTest.java
git status
# Shows file in BOTH "Changes to be committed" AND "Changes not staged"

# Commit the file (Committed)
git commit -m "Add NewTest"
git status
# Shows: nothing to commit, working tree clean
```

#### 3.4.3 Status Output Variations

**Clean Working Tree:**
```bash
git status

# Output:
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

**Ahead of Remote:**
```bash
git status

# Output:
On branch main
Your branch is ahead of 'origin/main' by 2 commits.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean
```

**Behind Remote:**
```bash
git status

# Output:
On branch main
Your branch is behind 'origin/main' by 3 commits, and can be fast-forwarded.
  (use "git pull" to update your local branch)

nothing to commit, working tree clean
```

**Diverged from Remote:**
```bash
git status

# Output:
On branch main
Your branch and 'origin/main' have diverged,
and have 2 and 3 different commits each, respectively.
  (use "git pull" to merge the remote branch into yours)

nothing to commit, working tree clean
```

#### 3.4.4 Short Status Format

```bash
# Concise status output
git status -s

# Output:
 M README.md              ← Modified, not staged
M  pom.xml                ← Staged (ready to commit)
MM src/test/LoginTest.java  ← Staged AND modified again
A  src/test/NewTest.java   ← Staged (new file)
?? test-data/users.csv     ← Untracked

# Status codes:
?? = Untracked
A  = Added (staged)
M  = Modified (in working directory)
M  (left) = Staged
MM = Staged and also modified
D  = Deleted
R  = Renamed
```

#### 3.4.5 Practical Usage in Test Automation

**Scenario 1: Before Starting Work**
```bash
# Always check status before starting
cd test-automation-framework
git status

# Ensure working tree is clean
# Output should show: nothing to commit, working tree clean

# If not clean, either commit or stash changes
git stash
# Now start new work
```

**Scenario 2: Before Committing Tests**
```bash
# You've written new test cases
git status

# Output:
Changes not staged for commit:
  modified:   src/test/java/tests/LoginTests.java
  modified:   src/test/java/pages/LoginPage.java

Untracked files:
  src/test/resources/testdata/login-data.csv

# Review what will be committed
git add src/test/
git status

# Output:
Changes to be committed:
  new file:   src/test/resources/testdata/login-data.csv
  modified:   src/test/java/pages/LoginPage.java
  modified:   src/test/java/tests/LoginTests.java

# Good to commit now
git commit -m "Add login test cases with data-driven approach"
```

**Scenario 3: After Pull Request Review**
```bash
# Reviewer requested changes
git status

# Output:
On branch feature/api-tests
Your branch is up to date with 'origin/feature/api-tests'.

Changes not staged for commit:
  modified:   src/test/java/tests/APITests.java

# Make requested changes
# ... edit files ...

# Check what changed
git status
git diff src/test/java/tests/APITests.java

# Stage and commit
git add src/test/java/tests/APITests.java
git commit -m "Address PR review comments - improve assertions"
git push
```

### 3.5 Repository Information Commands

#### 3.5.1 View Remote Repositories

```bash
# List configured remotes
git remote

# Output:
origin

# List remotes with URLs
git remote -v

# Output:
origin  git@github.com:company/test-automation.git (fetch)
origin  git@github.com:company/test-automation.git (push)
```

#### 3.5.2 View Repository Configuration

```bash
# View all repository configuration
git config --list --local

# Output:
core.repositoryformatversion=0
core.filemode=true
core.bare=false
core.logallrefupdates=true
remote.origin.url=git@github.com:company/test-automation.git
remote.origin.fetch=+refs/heads/*:refs/remotes/origin/*
branch.main.remote=origin
branch.main.merge=refs/heads/main
```

#### 3.5.3 View Repository Log

```bash
# Simple log
git log --oneline -5

# Output:
a1b2c3d (HEAD -> main) Add API test cases
e4f5g6h Update test data
i7j8k9l Fix login page selectors
m0n1o2p Add Cucumber scenarios
q3r4s5t Initial commit
```

### 3.6 Common Mistakes to Avoid

**Mistake 1: Modifying .git Directory Manually**
```bash
# NEVER do this:
rm -rf .git/objects/
# This will corrupt your repository!

# If you need to fix issues, use Git commands
git fsck  # Check repository integrity
```

**Mistake 2: Initializing Git in Wrong Directory**
```bash
# Wrong: Initializing in home directory
cd ~
git init  # DON'T DO THIS!

# This tracks entire home directory in Git

# If this happens, remove .git
cd ~
rm -rf .git
```

**Mistake 3: Not Checking Status Before Committing**
```bash
# Bad practice:
git add .
git commit -m "Updates"
git push

# Good practice:
git status           # Check what changed
git diff             # Review changes
git add <specific-files>
git status           # Verify staging
git commit -m "Meaningful message"
git push
```

**Mistake 4: Cloning into Existing Directory**
```bash
# This causes confusion
mkdir my-project
cd my-project
# ... work on files ...
git clone https://github.com/company/framework.git
# Now you have my-project/framework/ which is confusing

# Better approach:
git clone https://github.com/company/framework.git my-project
# OR
git clone https://github.com/company/framework.git
# Use the default directory name
```

### 3.7 Best Practices

**1. Always Check Status Before and After Operations**
```bash
git status          # Before any Git operation
# ... perform operation ...
git status          # Verify result
```

**2. Use Meaningful Repository Names**
```bash
# Good:
selenium-automation-framework
api-test-suite-rest-assured
mobile-appium-tests

# Bad:
test
automation
my-project
```

**3. Initialize Repository with README**
```bash
git init -b main
echo "# Test Automation Framework" > README.md
git add README.md
git commit -m "Initial commit with README"
```

**4. Set Up .gitignore Immediately**
```bash
git init -b main
# Create .gitignore before any commits
cat > .gitignore << EOF
target/
build/
*.log
test-output/
reports/
screenshots/
EOF
git add .gitignore
git commit -m "Add gitignore"
```

**5. Verify Clone Before Working**
```bash
git clone <url>
cd <repo>
git remote -v        # Verify remote
git branch -a        # Check branches
git log --oneline -3 # View recent commits
git status           # Ensure clean state
```

### 3.8 Interview Questions - Repository Fundamentals

**Q1: What is the difference between git init and git clone?**

**Answer:**
```
git init:
- Creates a new repository from scratch
- Used when starting a new project
- Creates empty .git directory
- No remote repository configured initially

git clone:
- Creates a copy of existing repository
- Downloads complete history
- Automatically sets up 'origin' remote
- Checks out default branch

Example:
# Starting new test framework
git init selenium-framework

# Joining existing project
git clone git@github.com:company/test-framework.git
```

**Q2: What does .git directory contain and why is it important?**

**Answer:**
```
.git directory contains:
- objects/: All commits, files, trees (Git database)
- refs/: Branch and tag references
- HEAD: Current branch pointer
- config: Repository configuration
- index: Staging area
- hooks/: Custom automation scripts

Importance:
- Entire repository history
- All branches and tags
- Configuration settings
- Deleting .git removes all Git history

Warning: Never manually modify .git contents
```

**Q3: Explain the different file states in Git.**

**Answer:**
```
File States:
1. Untracked: New files not in Git
2. Unmodified: Tracked, no changes since last commit
3. Modified: Tracked, changed but not staged
4. Staged: Marked for next commit

Transitions:
Untracked ──git add──> Staged
Modified  ──git add──> Staged
Staged    ──git commit──> Unmodified
Unmodified ──edit──> Modified

Example:
# Create file (Untracked)
echo "test" > test.java

# Stage file (Staged)
git add test.java

# Commit file (Unmodified)
git commit -m "Add test"

# Edit file (Modified)
echo "more" >> test.java

# Stage changes (Staged)
git add test.java

# Commit (Unmodified)
git commit -m "Update test"
```

---

## 4. Core Git Commands

### 4.1 Adding Files to Staging Area (git add)

The staging area (also called index) is where you prepare changes before committing.

```
Working Directory  ──git add──>  Staging Area  ──git commit──>  Repository
   (Modified)                      (Staged)                      (Committed)
```

#### 4.1.1 Basic Add Operations

```bash
# Add single file
git add src/test/java/LoginTest.java

# Add multiple specific files
git add src/test/java/LoginTest.java src/test/java/HomeTest.java

# Add all files in directory
git add src/test/java/

# Add all files in current directory and subdirectories
git add .

# Add all files in repository (from repo root)
git add --all
# OR
git add -A

# Add all modified and deleted files (excludes untracked)
git add -u
```

#### 4.1.2 Interactive Adding

```bash
# Interactive mode - choose what to add
git add -i

# Output:
           staged     unstaged path
  1:    unchanged        +2/-0 src/test/LoginTest.java
  2:    unchanged        +5/-1 src/test/HomeTest.java

*** Commands ***
  1: status       2: update       3: revert       4: add untracked
  5: patch        6: diff         7: quit         8: help
What now>
```

```bash
# Add parts of file (patch mode)
git add -p src/test/LoginTest.java

# Output:
diff --git a/src/test/LoginTest.java b/src/test/LoginTest.java
index e1b2c3d..f4g5h6i 100644
--- a/src/test/LoginTest.java
+++ b/src/test/LoginTest.java
@@ -10,6 +10,12 @@ public class LoginTest {
     // existing code
 }

+@Test
+public void testLoginWithValidCredentials() {
+    // test implementation
+}
+
Stage this hunk [y,n,q,a,d,s,e,?]?

# Options:
# y - stage this hunk
# n - do not stage this hunk
# q - quit; do not stage this or remaining hunks
# a - stage this and all remaining hunks
# d - do not stage this or remaining hunks
# s - split into smaller hunks
# e - manually edit hunk
```

#### 4.1.3 Practical Test Automation Examples

**Example 1: Adding New Test Suite**

```bash
# Created new test files
git status

# Output:
Untracked files:
  src/test/java/tests/CheckoutTests.java
  src/test/java/pages/CheckoutPage.java
  src/test/resources/testdata/checkout-data.csv

# Add all test-related files
git add src/test/

# Verify what's staged
git status

# Output:
Changes to be committed:
  new file:   src/test/java/pages/CheckoutPage.java
  new file:   src/test/java/tests/CheckoutTests.java
  new file:   src/test/resources/testdata/checkout-data.csv
```

**Example 2: Selective Staging**

```bash
# Modified multiple files
git status -s

# Output:
 M pom.xml
 M README.md
 M src/test/LoginTest.java
 M src/test/CheckoutTest.java
?? config/dev.properties

# Only want to commit test files, not config
git add src/test/*.java

git status

# Output:
Changes to be committed:
  modified:   src/test/CheckoutTest.java
  modified:   src/test/LoginTest.java

Changes not staged for commit:
  modified:   README.md
  modified:   pom.xml

Untracked files:
  config/dev.properties
```

**Example 3: Adding with Pattern Matching**

```bash
# Add all Java files
git add '*.java'

# Add all files in pages package
git add '**/pages/*.java'

# Add all CSV test data files
git add '**/*.csv'

# Add all property files except local ones
git add '*.properties'
git reset config/local.properties  # Remove local config
```

#### 4.1.4 Undoing Add Operations

```bash
# Unstage specific file
git restore --staged src/test/LoginTest.java

# Unstage all files
git restore --staged .

# Or using reset (older method)
git reset HEAD src/test/LoginTest.java
git reset HEAD  # Unstage all
```

**Example:**
```bash
# Accidentally staged wrong file
git add config/credentials.properties

git status
# Output:
Changes to be committed:
  new file:   config/credentials.properties

# Unstage it
git restore --staged config/credentials.properties

# Add to .gitignore to prevent future accidents
echo "config/credentials.properties" >> .gitignore
git add .gitignore
```

### 4.2 Committing Changes (git commit)

A commit is a snapshot of your repository at a specific point in time.

#### 4.2.1 Basic Commit Operations

```bash
# Commit with inline message
git commit -m "Add login test cases"

# Output:
[main a1b2c3d] Add login test cases
 3 files changed, 87 insertions(+), 12 deletions(-)
 create mode 100644 src/test/LoginTest.java
```

```bash
# Commit with detailed message (opens editor)
git commit

# In editor:
Add comprehensive login test cases

- Implemented positive login scenarios
- Added negative test cases for invalid credentials
- Created data-driven tests using CSV
- Updated LoginPage with new locators

Resolves: JIRA-1234
```

```bash
# Commit all tracked modified files (skip staging)
git commit -am "Update test data"

# Output:
[main e4f5g6h] Update test data
 2 files changed, 15 insertions(+), 3 deletions(-)
```

```bash
# Amend last commit (change message or add files)
git commit --amend -m "Add login test cases and update page objects"

# Or amend without changing message
git commit --amend --no-edit
```

#### 4.2.2 Commit Message Best Practices

**Good Commit Messages:**

```bash
# Format:
# <type>: <subject>
#
# <body>
#
# <footer>

# Examples:
git commit -m "feat: Add API authentication tests"

git commit -m "fix: Correct login button locator in LoginPage"

git commit -m "refactor: Extract common wait methods to BasePage"

git commit -m "test: Add end-to-end checkout flow tests"

git commit -m "docs: Update README with setup instructions"

git commit -m "chore: Update Selenium to version 4.15.0"
```

**Commit Message Types:**
- `feat`: New feature or test
- `fix`: Bug fix
- `refactor`: Code restructuring
- `test`: Adding or updating tests
- `docs`: Documentation changes
- `chore`: Maintenance tasks
- `style`: Code formatting
- `perf`: Performance improvements

**Detailed Commit Example:**

```bash
git commit

# In editor:
feat: Add comprehensive checkout flow tests

Implemented end-to-end tests for the checkout process:
- Happy path: Guest checkout with valid details
- Registered user checkout
- Payment method validation
- Shipping address validation
- Order confirmation verification

Test data stored in: src/test/resources/testdata/checkout.csv
Page objects updated: CheckoutPage, PaymentPage, OrderConfirmationPage

Resolves: JIRA-4567
Related to: JIRA-4560, JIRA-4561
```

#### 4.2.3 Real-World Commit Workflow

**Scenario: Adding New Test Cases**

```bash
# Step 1: Check current status
git status

# Step 2: Review changes
git diff src/test/java/tests/LoginTests.java

# Step 3: Stage specific files
git add src/test/java/tests/LoginTests.java
git add src/test/java/pages/LoginPage.java
git add src/test/resources/testdata/login-data.csv

# Step 4: Review staged changes
git diff --staged

# Step 5: Commit with meaningful message
git commit -m "test: Add data-driven login tests

- Implement parameterized tests for various login scenarios
- Add positive and negative test cases
- Include boundary value tests for username/password
- Update LoginPage with additional helper methods

Test coverage increased from 65% to 85%
Resolves: JIRA-7890"

# Output:
[feature/login-tests a1b2c3d] test: Add data-driven login tests
 3 files changed, 156 insertions(+), 23 deletions(-)
```

#### 4.2.4 Viewing Commit Information

```bash
# Show last commit details
git show

# Output:
commit a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0
Author: John Doe <john.doe@company.com>
Date:   Mon Jan 28 14:30:00 2026 -0800

    test: Add data-driven login tests

    - Implement parameterized tests for various login scenarios
    - Add positive and negative test cases
    - Include boundary value tests for username/password
    - Update LoginPage with additional helper methods

    Test coverage increased from 65% to 85%
    Resolves: JIRA-7890

diff --git a/src/test/LoginTests.java b/src/test/LoginTests.java
index e1b2c3d..f4g5h6i 100644
--- a/src/test/LoginTests.java
+++ b/src/test/LoginTests.java
... (shows full diff)
```

```bash
# Show specific commit
git show e4f5g6h

# Show commit with file names only
git show --name-only a1b2c3d

# Output:
commit a1b2c3d...
Author: John Doe <john.doe@company.com>
Date:   Mon Jan 28 14:30:00 2026 -0800

    test: Add data-driven login tests

src/test/LoginTests.java
src/test/LoginPage.java
src/test/resources/testdata/login-data.csv
```

### 4.3 Viewing Commit History (git log)

#### 4.3.1 Basic Log Commands

```bash
# Standard log output
git log

# Output:
commit a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0 (HEAD -> main)
Author: John Doe <john.doe@company.com>
Date:   Mon Jan 28 14:30:00 2026 -0800

    test: Add data-driven login tests

commit e4f5g6h7i8j9k0l1m2n3o4p5q6r7s8t9u0v1w2x3
Author: Jane Smith <jane.smith@company.com>
Date:   Mon Jan 28 10:15:00 2026 -0800

    fix: Update element locators in HomePage

... (continues)
```

```bash
# Compact one-line format
git log --oneline

# Output:
a1b2c3d (HEAD -> main) test: Add data-driven login tests
e4f5g6h fix: Update element locators in HomePage
i7j8k9l feat: Add API test suite
m0n1o2p refactor: Improve wait strategy
q3r4s5t docs: Update README
```

```bash
# Show last N commits
git log -5              # Last 5 commits
git log --oneline -3    # Last 3 commits (compact)

# Show commits with file changes
git log --stat

# Output:
commit a1b2c3d...
Author: John Doe <john.doe@company.com>
Date:   Mon Jan 28 14:30:00 2026 -0800

    test: Add data-driven login tests

 src/test/LoginTests.java                 | 87 +++++++++++++++++++++++++++++++
 src/test/LoginPage.java                  | 23 ++++++---
 src/test/resources/testdata/login-data.csv | 15 ++++++
 3 files changed, 120 insertions(+), 5 deletions(-)
```

#### 4.3.2 Advanced Log Formatting

```bash
# Custom format
git log --pretty=format:"%h - %an, %ar : %s"

# Output:
a1b2c3d - John Doe, 2 hours ago : test: Add data-driven login tests
e4f5g6h - Jane Smith, 5 hours ago : fix: Update element locators
i7j8k9l - Bob Johnson, 1 day ago : feat: Add API test suite

# Format placeholders:
# %h  = Short commit hash
# %H  = Full commit hash
# %an = Author name
# %ae = Author email
# %ar = Author date (relative)
# %ad = Author date
# %s  = Commit message subject
# %b  = Commit message body
```

```bash
# Visual branch graph
git log --oneline --graph --all --decorate

# Output:
* a1b2c3d (HEAD -> main, origin/main) test: Add data-driven login tests
* e4f5g6h fix: Update element locators
| * i7j8k9l (feature/api-tests) feat: Add API authentication
| * m0n1o2p test: Add API response validation
|/
* q3r4s5t refactor: Improve wait strategy
* u5v6w7x Initial commit
```

#### 4.3.3 Filtering Commits

**By Author:**
```bash
# Commits by specific author
git log --author="John Doe"

# Commits by email
git log --author="john.doe@company.com"

# Multiple authors
git log --author="John\|Jane"
```

**By Date:**
```bash
# Commits since specific date
git log --since="2026-01-01"

# Commits in last 2 weeks
git log --since="2 weeks ago"

# Commits in date range
git log --since="2026-01-01" --until="2026-01-31"

# Commits in last 3 days
git log --since="3 days ago"
```

**By File:**
```bash
# Commits affecting specific file
git log src/test/LoginTests.java

# Commits with changes to files in directory
git log src/test/pages/

# Show patches for file
git log -p src/test/LoginTests.java
```

**By Commit Message:**
```bash
# Commits with "login" in message
git log --grep="login"

# Case-insensitive search
git log --grep="API" --grep="api" -i

# Commits with "fix" or "bug"
git log --grep="fix\|bug"
```

**By Code Changes:**
```bash
# Commits that added or removed specific code
git log -S "WebDriverWait"

# Commits that changed occurrences of pattern
git log -G "driver\.findElement"
```

#### 4.3.4 Practical Examples for Test Automation

**Example 1: Review Test Changes This Week**

```bash
git log --since="1 week ago" --oneline --author="$(git config user.name)" -- src/test/

# Output:
a1b2c3d test: Add data-driven login tests
e4f5g6h test: Add checkout flow end-to-end tests
i7j8k9l test: Update assertion messages
m0n1o2p fix: Correct test data file path
```

**Example 2: Find Who Modified Page Object**

```bash
git log --oneline src/test/pages/LoginPage.java

# Output:
a1b2c3d test: Add data-driven login tests
q3r4s5t fix: Update login button locator
u5v6w7x refactor: Improve page object structure
y7z8a9b feat: Initial LoginPage implementation
```

```bash
# Show actual changes
git log -p --oneline src/test/pages/LoginPage.java

# Shows commit + diff for each change
```

**Example 3: View Test Suite Evolution**

```bash
git log --oneline --graph --all src/test/

# Shows visual history of test directory
```

**Example 4: Find Commits Between Releases**

```bash
# Commits between tags
git log v1.0.0..v1.1.0 --oneline

# Output:
a1b2c3d test: Add data-driven login tests
e4f5g6h feat: Add API test suite
i7j8k9l fix: Resolve flaky tests
m0n1o2p refactor: Improve test data management
```

### 4.4 Comparing Changes (git diff)

#### 4.4.1 Basic Diff Operations

```bash
# Show unstaged changes (working directory vs. staging area)
git diff

# Output:
diff --git a/src/test/LoginTests.java b/src/test/LoginTests.java
index e1b2c3d..f4g5h6i 100644
--- a/src/test/LoginTests.java
+++ b/src/test/LoginTests.java
@@ -15,7 +15,12 @@ public class LoginTests {

     @Test
     public void testValidLogin() {
-        loginPage.login("user", "pass");
+        loginPage.enterUsername("user");
+        loginPage.enterPassword("pass");
+        loginPage.clickLoginButton();
+
+        HomePage homePage = new HomePage(driver);
+        Assert.assertTrue(homePage.isUserLoggedIn());
     }
 }
```

```bash
# Show staged changes (staging area vs. last commit)
git diff --staged
# OR
git diff --cached

# Show all changes (staged + unstaged)
git diff HEAD
```

#### 4.4.2 Comparing Specific Files

```bash
# Diff for specific file
git diff src/test/LoginTests.java

# Diff for multiple files
git diff src/test/LoginTests.java src/test/HomePage.java

# Diff for directory
git diff src/test/pages/

# Diff with specific commit
git diff a1b2c3d src/test/LoginTests.java

# Diff between two commits
git diff e4f5g6h a1b2c3d src/test/LoginTests.java
```

#### 4.4.3 Diff Output Formats

```bash
# Word-by-word diff
git diff --word-diff

# Output:
@Test
public void testValidLogin() {
    loginPage.[-login("user", "pass");-]
    {+enterUsername("user");+}
    {+loginPage.enterPassword("pass");+}
    {+loginPage.clickLoginButton();+}
}
```

```bash
# Show only file names
git diff --name-only

# Output:
src/test/LoginTests.java
src/test/pages/LoginPage.java
src/test/resources/testdata/users.csv
```

```bash
# Show file names with status
git diff --name-status

# Output:
M       src/test/LoginTests.java
M       src/test/pages/LoginPage.java
A       src/test/resources/testdata/users.csv

# Status codes:
# A = Added
# M = Modified
# D = Deleted
# R = Renamed
```

```bash
# Statistical summary
git diff --stat

# Output:
 src/test/LoginTests.java               | 23 ++++++++++++++++++---
 src/test/pages/LoginPage.java          |  8 +++++++-
 src/test/resources/testdata/users.csv  | 15 ++++++++++++++
 3 files changed, 42 insertions(+), 4 deletions(-)
```

#### 4.4.4 Comparing Branches

```bash
# Diff between branches
git diff main..feature/login-tests

# Diff specific file between branches
git diff main..feature/login-tests -- src/test/LoginTests.java

# Show what's in feature branch but not in main
git diff main...feature/login-tests

# Files changed between branches
git diff --name-only main..feature/login-tests
```

#### 4.4.5 Practical Diff Examples

**Example 1: Review Changes Before Committing**

```bash
# Made changes to test files
git status -s

# Output:
 M src/test/LoginTests.java
 M src/test/pages/LoginPage.java

# Review what changed
git diff

# If satisfied, stage changes
git add src/test/
git diff --staged  # Review staged changes
git commit -m "Refactor login test implementation"
```

**Example 2: Compare Test File with Previous Version**

```bash
# View test file 3 commits ago vs. now
git log --oneline -5 src/test/LoginTests.java

# Output:
a1b2c3d test: Add data-driven tests
e4f5g6h refactor: Improve test structure
i7j8k9l fix: Update assertions

# Compare current with 3 commits ago
git diff HEAD~3 src/test/LoginTests.java

# Or specific commit
git diff i7j8k9l src/test/LoginTests.java
```

**Example 3: Check What Changed in Pull Request**

```bash
# Switch to PR branch
git checkout feature/api-tests

# Compare with main branch
git diff main --name-status

# Output:
A       src/test/api/AuthenticationTests.java
A       src/test/api/UserAPITests.java
M       pom.xml
A       src/test/utils/APIHelper.java

# View detailed changes
git diff main --stat
git diff main
```

**Example 4: Using External Diff Tool**

```bash
# Configure VS Code as diff tool (if not done already)
git config --global diff.tool vscode
git config --global difftool.vscode.cmd "code --wait --diff \$LOCAL \$REMOTE"

# Use diff tool
git difftool src/test/LoginTests.java

# Opens VS Code with side-by-side comparison
```

### 4.5 Removing and Renaming Files

#### 4.5.1 Removing Files

```bash
# Remove file from Git and working directory
git rm src/test/OldTest.java

# Output:
rm 'src/test/OldTest.java'

git status
# Output:
Changes to be committed:
  deleted:    src/test/OldTest.java
```

```bash
# Remove file from Git but keep in working directory
git rm --cached src/test/LocalTest.java

# Useful for files that should be untracked
# Often used with .gitignore

# Example:
git rm --cached config/local.properties
echo "config/local.properties" >> .gitignore
git add .gitignore
git commit -m "Untrack local properties file"
```

```bash
# Remove directory
git rm -r test-output/

# Remove all .log files
git rm '*.log'
```

#### 4.5.2 Renaming/Moving Files

```bash
# Rename file
git mv src/test/LoginTests.java src/test/LoginTestSuite.java

git status
# Output:
Changes to be committed:
  renamed:    src/test/LoginTests.java -> src/test/LoginTestSuite.java
```

```bash
# Move file to different directory
git mv src/test/Utils.java src/test/utils/Utils.java

# Move multiple files
git mv src/test/*.java src/test/legacy/
```

**Why use `git mv` instead of regular `mv`?**

```bash
# Without git mv (manual approach):
mv src/test/old.java src/test/new.java
git rm src/test/old.java
git add src/test/new.java

# With git mv (single command):
git mv src/test/old.java src/test/new.java
# Git automatically stages the rename
```

### 4.6 Common Mistakes to Avoid

**Mistake 1: Committing Without Reviewing Changes**
```bash
# Bad:
git add .
git commit -m "updates"

# Good:
git status
git diff
git add <specific-files>
git diff --staged
git commit -m "Meaningful message"
```

**Mistake 2: Poor Commit Messages**
```bash
# Bad:
git commit -m "fix"
git commit -m "update"
git commit -m "changes"
git commit -m "asdfasdf"

# Good:
git commit -m "fix: Correct login button locator in LoginPage"
git commit -m "test: Add checkout flow end-to-end tests"
git commit -m "refactor: Extract common waits to BasePage"
```

**Mistake 3: Committing Large Binary Files**
```bash
# Bad: Committing test reports/screenshots
git add test-output/
git add screenshots/

# Good: Use .gitignore
echo "test-output/" >> .gitignore
echo "screenshots/" >> .gitignore
git add .gitignore
```

**Mistake 4: Not Using Staging Area Properly**
```bash
# Bad: Committing all changes together
git commit -am "Various updates"

# Good: Separate logical commits
git add src/test/LoginTests.java
git commit -m "test: Add login tests"

git add src/test/pages/LoginPage.java
git commit -m "refactor: Update LoginPage selectors"
```

### 4.7 Best Practices

**1. Commit Often, Perfect Later**
```bash
# Make small, frequent commits
git add src/test/LoginTests.java
git commit -m "WIP: Add basic login test structure"

# Continue working
git add src/test/LoginTests.java
git commit -m "test: Implement positive login scenario"

# Add more
git add src/test/LoginTests.java
git commit -m "test: Add negative login scenarios"

# Later, can squash commits if needed (covered in Advanced section)
```

**2. Write Descriptive Commit Messages**
```bash
# Follow conventional commits format
<type>(<scope>): <subject>

<body>

<footer>

# Examples:
feat(api): Add REST API test suite
fix(login): Correct password field locator
test(checkout): Add end-to-end checkout tests
refactor(pages): Extract common page methods to BasePage
docs(readme): Update setup instructions
```

**3. Review Before Committing**
```bash
# Always review your changes
git status           # What will be committed?
git diff            # What changed?
git diff --staged   # What's staged?
git log --oneline -3  # Recent commits for message style
```

**4. Use .gitignore Properly**
```bash
# Create comprehensive .gitignore early
cat > .gitignore << EOF
# Build outputs
target/
build/
out/

# IDE files
.idea/
.vscode/
*.iml
.classpath
.project
.settings/

# Test outputs
test-output/
reports/
screenshots/
logs/
*.log

# OS files
.DS_Store
Thumbs.db

# Local configuration
config/local.properties
*.local

# Dependencies
node_modules/
.gradle/
EOF

git add .gitignore
git commit -m "chore: Add comprehensive gitignore"
```

**5. Atomic Commits**
```bash
# Each commit should be a single logical change

# Bad: Mixed concerns in one commit
git add .
git commit -m "Add tests and fix bugs and update docs"

# Good: Separate commits
git add src/test/LoginTests.java
git commit -m "test: Add login test suite"

git add src/pages/LoginPage.java
git commit -m "fix: Correct login button locator"

git add README.md
git commit -m "docs: Add test execution instructions"
```

### 4.8 Interview Questions - Core Commands

**Q1: What is the difference between git add and git commit?**

**Answer:**
```
git add:
- Moves changes to staging area (index)
- Prepares files for commit
- Can be selective (specific files)
- Changes not yet in repository history

git commit:
- Creates snapshot in repository
- Moves staged changes to repository
- Generates unique commit hash
- Permanent record in Git history

Workflow:
Working Directory ─git add→ Staging Area ─git commit→ Repository

Example:
# Edit file (Working Directory)
echo "test" > test.java

# Stage file (Staging Area)
git add test.java

# Commit (Repository)
git commit -m "Add test file"
```

**Q2: Explain the staging area (index). Why do we need it?**

**Answer:**
```
Staging Area (Index):
- Intermediate area between working directory and repository
- Allows selective commits
- Review changes before committing
- Organize commits logically

Benefits:
1. Selective Staging: Commit only relevant files
2. Partial Staging: Commit parts of files (git add -p)
3. Review: Check what will be committed
4. Flexibility: Unstage files if needed

Example:
# Modified 5 files, but only want to commit 2
git add file1.java file2.java
git commit -m "Logical change 1"

git add file3.java file4.java
git commit -m "Logical change 2"

# file5.java still in working directory
```

**Q3: How do you undo changes at different stages?**

**Answer:**
```
Scenario 1: Undo Working Directory Changes (Not Staged)
git restore file.java
# OR (older method)
git checkout -- file.java

Scenario 2: Unstage Files (Staged but Not Committed)
git restore --staged file.java
# OR
git reset HEAD file.java

Scenario 3: Undo Last Commit (Keep Changes)
git reset --soft HEAD~1

Scenario 4: Undo Last Commit (Discard Changes)
git reset --hard HEAD~1  # DANGEROUS!

Scenario 5: Revert Commit (Create New Commit)
git revert <commit-hash>

Example Workflow:
# Modified file
echo "wrong" > test.java

# Staged by mistake
git add test.java

# Unstage
git restore --staged test.java

# Discard changes
git restore test.java
```

**Q4: What makes a good commit message?**

**Answer:**
```
Good Commit Message Structure:

Short summary (50 chars or less)

Detailed explanation (72 chars per line)
- What changed
- Why the change was made
- Any side effects or considerations

Refs: JIRA-123

Example:
feat: Add data-driven login tests

Implement parameterized tests for login functionality:
- Add positive test cases with valid credentials
- Include negative scenarios (invalid user/pass)
- Test boundary values and special characters
- Extract test data to CSV file

This increases login test coverage from 60% to 95%
and improves test maintainability.

Resolves: JIRA-1234
Related to: JIRA-1230

Best Practices:
1. Use imperative mood ("Add feature" not "Added feature")
2. First line is summary, separated by blank line
3. Explain "why" not just "what"
4. Reference issue tracking numbers
5. Use conventional commit format (feat, fix, test, etc.)
```

---

## 5. Branching and Merging

### 5.1 Understanding Branches

Branches allow parallel development without affecting the main codebase.

```
main branch:      A---B---C---D---E
                       \
feature branch:         F---G---H
```

**Why Branches Matter in Test Automation:**
- Develop new test suites without breaking existing tests
- Multiple QA engineers work on different features
- Isolate experimental tests
- Maintain separate branches for different environments

### 5.1.1 Branch Basics

```bash
# List all local branches
git branch

# Output:
  feature/api-tests
  feature/login-tests
* main

# * indicates current branch

# List all branches (local + remote)
git branch -a

# Output:
  feature/api-tests
  feature/login-tests
* main
  remotes/origin/HEAD -> origin/main
  remotes/origin/main
  remotes/origin/develop
  remotes/origin/feature/checkout-tests
```

```bash
# List with last commit info
git branch -v

# Output:
  feature/api-tests   a1b2c3d Add API authentication tests
  feature/login-tests e4f5g6h Update login test data
* main                i7j8k9l Merge pull request #45

# List merged branches
git branch --merged

# Output:
  feature/old-tests
* main

# List unmerged branches
git branch --no-merged

# Output:
  feature/api-tests
  feature/login-tests
```

### 5.2 Creating and Switching Branches

#### 5.2.1 Creating Branches

```bash
# Create new branch (but stay on current branch)
git branch feature/checkout-tests

# Verify branch created
git branch

# Output:
  feature/checkout-tests
* main

# Create and switch to new branch
git checkout -b feature/payment-tests

# Output:
Switched to a new branch 'feature/payment-tests'

# Modern syntax (Git 2.23+)
git switch -c feature/payment-tests
```

#### 5.2.2 Switching Branches

```bash
# Switch to existing branch
git checkout main

# Output:
Switched to branch 'main'
Your branch is up to date with 'origin/main'.

# Modern syntax
git switch main
```

```bash
# Switch with uncommitted changes (auto-stash)
git switch -c feature/new-tests
# OR
git checkout -b feature/new-tests

# If there are conflicts, Git will warn you
error: Your local changes to the following files would be overwritten by checkout:
        src/test/LoginTests.java
Please commit your changes or stash them before you switch branches.
```

#### 5.2.3 Branch Naming Conventions

**Good Branch Names:**
```bash
# Feature branches
feature/api-authentication-tests
feature/checkout-flow-tests
feature/mobile-login-tests

# Bug fix branches
fix/login-button-locator
fix/flaky-checkout-test
hotfix/critical-payment-issue

# Test-specific branches
test/smoke-suite
test/regression-suite
test/performance-tests

# Environment branches
develop
staging
qa
production
```

**Branch Name Patterns:**
```
<type>/<description>

Types:
- feature/  : New functionality or tests
- fix/      : Bug fixes
- hotfix/   : Critical fixes
- test/     : Test suites
- refactor/ : Code improvements
- docs/     : Documentation
- chore/    : Maintenance tasks
```

### 5.3 Practical Branching Workflow

**Scenario: Adding New Test Suite**

```bash
# Step 1: Start from main branch
git checkout main
git pull origin main  # Get latest changes

# Step 2: Create feature branch
git checkout -b feature/api-tests

# Output:
Switched to a new branch 'feature/api-tests'

# Step 3: Verify you're on correct branch
git branch

# Output:
* feature/api-tests
  main

# Step 4: Create test files
mkdir -p src/test/java/api
touch src/test/java/api/UserAPITests.java
touch src/test/java/api/AuthAPITests.java

# Step 5: Work on tests
# ... write test code ...

# Step 6: Stage and commit
git status
git add src/test/java/api/
git commit -m "test: Add initial API test structure"

# Step 7: Continue development
# ... more commits ...

git add src/test/java/api/UserAPITests.java
git commit -m "test: Implement user CRUD API tests"

git add src/test/java/api/AuthAPITests.java
git commit -m "test: Add authentication API tests"

# Step 8: View branch history
git log --oneline

# Output:
a1b2c3d (HEAD -> feature/api-tests) test: Add authentication API tests
e4f5g6h test: Implement user CRUD API tests
i7j8k9l test: Add initial API test structure
m0n1o2p (origin/main, main) Previous commit from main

# Step 9: Push branch to remote (covered in later section)
git push -u origin feature/api-tests
```

### 5.4 Merging Branches

Merging combines changes from different branches.

```
Before merge:
main:    A---B---C
              \
feature:       D---E---F

After merge:
main:    A---B---C-------M
              \         /
feature:       D---E---F
```

#### 5.4.1 Fast-Forward Merge

```
Before:
main:    A---B---C
              \
feature:       D---E

After (fast-forward):
main:    A---B---C---D---E
                         ^
                      feature
```

```bash
# Create and work on feature branch
git checkout -b feature/quick-fix
echo "fix" > fix.txt
git add fix.txt
git commit -m "Apply quick fix"

# Switch back to main
git checkout main

# Merge (fast-forward)
git merge feature/quick-fix

# Output:
Updating e4f5g6h..a1b2c3d
Fast-forward
 fix.txt | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 fix.txt

# Branch pointer simply moved forward
```

#### 5.4.2 Three-Way Merge

```
Before:
main:    A---B---C---D
              \
feature:       E---F---G

After (three-way merge):
main:    A---B---C---D---M
              \         /
feature:       E---F---G
```

```bash
# Scenario: Main branch has moved forward

# On feature branch
git checkout feature/api-tests
git log --oneline

# Output:
a1b2c3d (HEAD -> feature/api-tests) test: Add auth API tests
e4f5g6h test: Add user API tests
i7j8k9l (origin/main, main) Common ancestor

# Main branch has new commits
git checkout main
git log --oneline

# Output:
m0n1o2p (HEAD -> main) fix: Update config
q3r4s5t refactor: Improve utils
i7j8k9l Common ancestor

# Merge feature into main
git merge feature/api-tests

# Output:
Merge made by the 'ort' strategy.
 src/test/java/api/AuthAPITests.java | 45 +++++++++++++++++++++
 src/test/java/api/UserAPITests.java | 67 ++++++++++++++++++++++++++++++++
 2 files changed, 112 insertions(+)

# View merge commit
git log --oneline --graph -5

# Output:
*   u5v6w7x (HEAD -> main) Merge branch 'feature/api-tests'
|\
| * a1b2c3d (feature/api-tests) test: Add auth API tests
| * e4f5g6h test: Add user API tests
* | m0n1o2p fix: Update config
* | q3r4s5t refactor: Improve utils
|/
* i7j8k9l Common ancestor
```

#### 5.4.3 Merge with No Fast-Forward

Forces creation of merge commit even when fast-forward is possible.

```bash
# Create and work on feature
git checkout -b feature/login-tests
git commit -am "test: Add login tests"

# Merge with no fast-forward
git checkout main
git merge --no-ff feature/login-tests -m "Merge feature: Add login tests"

# Output:
Merge made by the 'ort' strategy.
 src/test/LoginTests.java | 50 ++++++++++++++++++++++++++++++++
 1 file changed, 50 insertions(+)

# Creates merge commit even though fast-forward was possible
# This preserves feature branch history
```

**Why use --no-ff?**
```
With fast-forward (harder to see feature boundaries):
main: A---B---C---D---E

With --no-ff (clear feature boundaries):
main: A---B-------M
           \     /
feature:    C---D---E
```

#### 5.4.4 Squash Merge

Combines all feature branch commits into single commit on main.

```bash
# Feature branch has multiple commits
git checkout feature/api-tests
git log --oneline

# Output:
a1b2c3d test: Fix API test assertions
e4f5g6h test: Add error handling tests
i7j8k9l test: Implement user API tests
m0n1o2p test: Add API test framework

# Squash merge into main
git checkout main
git merge --squash feature/api-tests

# Output:
Squash commit -- not updating HEAD
Automatic merge went well; stopped before committing as requested

# All changes staged, now commit
git commit -m "feat: Add comprehensive API test suite

- User CRUD operations
- Authentication tests
- Error handling
- Request/response validation"

# Result: Single clean commit instead of 4
git log --oneline -2

# Output:
q3r4s5t (HEAD -> main) feat: Add comprehensive API test suite
u5v6w7x Previous commit
```

**When to use squash merge:**
- Feature branch has many WIP commits
- Want clean linear history
- Commits on feature branch are not meaningful for main branch

### 5.5 Deleting Branches

#### 5.5.1 Safe Delete (Merged Branches)

```bash
# Delete merged branch
git branch -d feature/api-tests

# Output:
Deleted branch feature/api-tests (was a1b2c3d).

# If branch not merged, Git warns you
git branch -d feature/unfinished-work

# Output:
error: The branch 'feature/unfinished-work' is not fully merged.
If you are sure you want to delete it, run 'git branch -D feature/unfinished-work'.
```

#### 5.5.2 Force Delete (Unmerged Branches)

```bash
# Force delete unmerged branch
git branch -D feature/experimental-tests

# Output:
Deleted branch feature/experimental-tests (was e4f5g6h).

# Use with caution - loses commits if not merged elsewhere!
```

#### 5.5.3 Delete Remote Branch

```bash
# Delete branch on remote repository
git push origin --delete feature/api-tests

# Output:
To github.com:company/test-automation.git
 - [deleted]         feature/api-tests

# Alternative syntax
git push origin :feature/api-tests
```

### 5.6 Branch Management Best Practices

**1. Keep Branches Short-Lived**
```bash
# Bad: Long-lived feature branch (merge conflicts likely)
# Created: 2025-01-01
# Last update: 2026-01-28 (still open!)

# Good: Short-lived feature branches
# Created: 2026-01-28
# Merged: 2026-01-30 (2 days)
```

**2. Sync with Main Regularly**
```bash
# While working on feature branch
git checkout feature/my-tests

# Regularly pull latest main changes
git fetch origin
git merge origin/main

# Or rebase (covered in Advanced section)
git rebase origin/main
```

**3. Delete Merged Branches**
```bash
# After merge, delete local branch
git branch -d feature/completed-tests

# Delete remote branch
git push origin --delete feature/completed-tests

# Clean up stale remote-tracking branches
git fetch --prune
```

**4. Use Descriptive Branch Names**
```bash
# Bad
git checkout -b test
git checkout -b fix
git checkout -b temp

# Good
git checkout -b feature/api-authentication-tests
git checkout -b fix/login-button-locator-issue
git checkout -b test/smoke-suite-refactor
```

### 5.7 Real-World Branching Strategies

#### 5.7.1 Feature Branch Workflow

```
main (always deployable)
  |
  |--- feature/api-tests
  |
  |--- feature/ui-tests
  |
  |--- fix/flaky-test
```

```bash
# Workflow:
1. Create feature branch from main
2. Work on feature
3. Create Pull Request
4. Code review
5. Merge to main
6. Delete feature branch

# Example:
git checkout main
git pull origin main
git checkout -b feature/checkout-tests

# ... develop tests ...

git push -u origin feature/checkout-tests
# Create Pull Request on GitHub/GitLab
# After approval and merge:
git checkout main
git pull origin main
git branch -d feature/checkout-tests
```

#### 5.7.2 GitFlow Workflow

```
main (production)
  |
develop (integration)
  |
  |--- feature/xxx
  |
  |--- release/1.0
  |
hotfix/critical-bug
```

**Not commonly used in test automation** but understanding is valuable.

#### 5.7.3 Trunk-Based Development

```
main (trunk)
  |
  |--- short-lived branches (< 2 days)
       merged frequently
```

**Common in modern test automation:**
- Small, frequent merges
- Continuous Integration friendly
- Reduced merge conflicts

### 5.8 Common Mistakes to Avoid

**Mistake 1: Working Directly on Main**
```bash
# Bad
git checkout main
# ... make changes ...
git commit -m "Updates"

# Good
git checkout main
git pull origin main
git checkout -b feature/my-changes
# ... make changes ...
git commit -m "Meaningful message"
```

**Mistake 2: Not Syncing Before Creating Branch**
```bash
# Bad: Create branch from outdated main
git checkout main  # (Not up to date)
git checkout -b feature/new-tests

# Good: Sync first
git checkout main
git pull origin main  # Get latest
git checkout -b feature/new-tests
```

**Mistake 3: Leaving Branches Unmerged**
```bash
# Check for old unmerged branches
git branch --no-merged

# Clean up unnecessary branches
git branch -D old-experimental-branch
```

**Mistake 4: Merge Conflicts Due to Large Branches**
```bash
# Bad: Work on feature for weeks without syncing
# Results in massive merge conflicts

# Good: Sync regularly
git checkout feature/long-running
git fetch origin
git merge origin/main  # Or rebase
# Resolve small conflicts incrementally
```

### 5.9 Interview Questions - Branching

**Q1: What is the difference between git merge and git rebase?**

**Answer:**
```
git merge:
- Combines branches with a merge commit
- Preserves complete history
- Non-destructive operation
- Creates branching in history graph

git rebase:
- Re-applies commits on top of another base
- Creates linear history
- Rewrites commit history
- Cleaner, but destructive

Example:

Before:
main:    A---B---C
          \
feature:   D---E

After merge:
main:    A---B---C---M
          \         /
feature:   D---E---

After rebase:
main:    A---B---C---D'---E'

Use merge when: Working with shared branches
Use rebase when: Cleaning up local branches before pushing
```

**Q2: Explain fast-forward vs. three-way merge.**

**Answer:**
```
Fast-Forward Merge:
- No divergent changes
- Main branch hasn't moved forward
- Simply moves pointer forward
- No merge commit created

Three-Way Merge:
- Both branches have new commits
- Uses three commits: common ancestor + two branch tips
- Creates merge commit
- Preserves both histories

Example:

Fast-Forward:
Before:  main: A---B
                  \
         feature:  C---D

After:   main: A---B---C---D (pointer moved)

Three-Way:
Before:  main: A---B---E
                  \
         feature:  C---D

After:   main: A---B---E---M (merge commit)
                  \       /
         feature:  C---D

Force three-way merge with: git merge --no-ff
```

**Q3: When should you delete a branch?**

**Answer:**
```
Delete Branch When:
1. Feature merged to main
2. Pull request approved and merged
3. Work abandoned/no longer needed
4. Branch experiment unsuccessful

Don't Delete When:
1. Still in active development
2. Not yet merged
3. Waiting for code review
4. Contains important commits not elsewhere

Commands:
# Safe delete (only if merged)
git branch -d feature/completed

# Force delete (even if not merged)
git branch -D feature/abandoned

# Delete remote branch
git push origin --delete feature/completed

Example Workflow:
# After PR merged
git checkout main
git pull origin main
git branch -d feature/my-tests
git push origin --delete feature/my-tests

# Clean up remote-tracking branches
git fetch --prune
```

**Q4: How do you handle a long-running feature branch?**

**Answer:**
```
Strategies:
1. Regularly sync with main
2. Break into smaller sub-branches
3. Frequent intermediate merges
4. Use feature flags for partial features

Example:

# Strategy 1: Regular sync
git checkout feature/long-running
git fetch origin
git merge origin/main  # Sync frequently

# Strategy 2: Sub-branches
main
  |
feature/api-tests (long-running)
  |
  |--- feature/api-tests-auth
  |--- feature/api-tests-users
  |--- feature/api-tests-products

# Each sub-branch merged to feature/api-tests
# Finally feature/api-tests merged to main

# Strategy 3: Keep working, merge main regularly
git checkout feature/big-feature
# ... week 1 work ...
git fetch origin
git merge origin/main  # Resolve small conflicts

# ... week 2 work ...
git fetch origin
git merge origin/main  # Resolve small conflicts

# Better than one big merge at the end!
```

---

## 6. Remote Repositories

### 6.1 Understanding Remote Repositories

Remote repositories are versions of your project hosted on the internet or network.

```
Local Repository          Remote Repository (GitHub/GitLab)
┌─────────────────┐      ┌──────────────────────┐
│  .git/          │      │  Full repository     │
│  Working Dir    │◄────►│  All branches        │
│  Staging Area   │      │  All commits         │
└─────────────────┘      │  Tags                │
                         └──────────────────────┘
     git push  ────────►
     git pull/fetch  ◄───
```

**Popular Remote Repository Hosts:**
- **GitHub** - Most popular, great for open source
- **GitLab** - Full DevOps platform, CI/CD built-in
- **Bitbucket** - Atlassian product, integrates with Jira
- **Azure DevOps** - Microsoft's offering
- **Self-hosted** - GitLab/Gitea on own servers

### 6.2 Working with Remotes

#### 6.2.1 Viewing Remotes

```bash
# List remote repositories
git remote

# Output:
origin

# List with URLs
git remote -v

# Output:
origin  git@github.com:company/test-automation.git (fetch)
origin  git@github.com:company/test-automation.git (push)

# Show detailed remote information
git remote show origin

# Output:
* remote origin
  Fetch URL: git@github.com:company/test-automation.git
  Push  URL: git@github.com:company/test-automation.git
  HEAD branch: main
  Remote branches:
    develop                tracked
    feature/api-tests      tracked
    feature/ui-tests       tracked
    main                   tracked
  Local branch configured for 'git pull':
    main merges with remote main
  Local ref configured for 'git push':
    main pushes to main (up to date)
```

#### 6.2.2 Adding Remotes

```bash
# Add new remote
git remote add origin git@github.com:company/test-automation.git

# Add additional remote (e.g., fork)
git remote add upstream git@github.com:original-owner/test-automation.git

# Verify
git remote -v

# Output:
origin    git@github.com:company/test-automation.git (fetch)
origin    git@github.com:company/test-automation.git (push)
upstream  git@github.com:original-owner/test-automation.git (fetch)
upstream  git@github.com:original-owner/test-automation.git (push)
```

#### 6.2.3 Changing Remote URL

```bash
# Change from HTTPS to SSH
git remote set-url origin git@github.com:company/test-automation.git

# Verify change
git remote -v

# Output:
origin  git@github.com:company/test-automation.git (fetch)
origin  git@github.com:company/test-automation.git (push)

# Change specific URL (fetch or push separately)
git remote set-url --push origin git@github.com:company/test-automation.git
```

#### 6.2.4 Removing Remotes

```bash
# Remove remote
git remote remove upstream

# Or using 'rm'
git remote rm upstream

# Verify
git remote -v
```

### 6.3 Fetching Changes

`git fetch` downloads commits, files, and refs from remote repository but doesn't merge them.

```
Remote:  A---B---C---D---E
                     ^
                   origin/main

Local:   A---B---C
             ^
            main

After fetch:
Remote:  A---B---C---D---E
                     ^
                   origin/main

Local:   A---B---C (main)
             \
              D---E (origin/main in local)
```

```bash
# Fetch from origin (default remote)
git fetch

# Output:
remote: Enumerating objects: 15, done.
remote: Counting objects: 100% (15/15), done.
remote: Compressing objects: 100% (8/8), done.
remote: Total 12 (delta 5), reused 10 (delta 3), pack-reused 0
Unpacking objects: 100% (12/12), 2.45 KiB | 418.00 KiB/s, done.
From github.com:company/test-automation
   a1b2c3d..e4f5g6h  main       -> origin/main
 * [new branch]      feature/new-tests -> origin/feature/new-tests
```

```bash
# Fetch from specific remote
git fetch upstream

# Fetch all remotes
git fetch --all

# Fetch and prune deleted remote branches
git fetch --prune
# OR
git fetch -p

# Output:
From github.com:company/test-automation
 - [deleted]         (none)     -> origin/feature/old-tests
   a1b2c3d..e4f5g6h  main       -> origin/main
```

**After fetching, review changes:**

```bash
# View what's new on remote main
git log --oneline main..origin/main

# Output:
e4f5g6h test: Add new API test suite
i7j8k9l fix: Update test configuration
m0n1o2p refactor: Improve page objects

# Compare current branch with remote
git diff main origin/main

# Merge fetched changes when ready
git merge origin/main
```

### 6.4 Pulling Changes

`git pull` = `git fetch` + `git merge`

```bash
# Pull from current branch's remote
git pull

# Output:
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 1), reused 3 (delta 1), pack-reused 0
Unpacking objects: 100% (3/3), 721 bytes | 240.00 KiB/s, done.
From github.com:company/test-automation
   a1b2c3d..e4f5g6h  main     -> origin/main
Updating a1b2c3d..e4f5g6h
Fast-forward
 src/test/APITests.java | 25 +++++++++++++++++++++++++
 1 file changed, 25 insertions(+)
```

```bash
# Pull from specific remote and branch
git pull origin main

# Pull with rebase instead of merge
git pull --rebase

# Pull all branches
git pull --all
```

**Pull vs Fetch:**

```
git fetch:
- Downloads changes
- Doesn't modify working directory
- Safe to run anytime
- Review before integrating

git pull:
- Downloads AND merges changes
- Modifies working directory
- Can cause conflicts
- Use when ready to integrate

Workflow:
# Safe approach
git fetch origin
git log --oneline main..origin/main  # Review changes
git diff main origin/main             # See differences
git merge origin/main                 # Merge when ready

# Quick approach (when confident)
git pull origin main
```

### 6.5 Real-World Remote Scenarios

**Scenario 1: Daily Workflow - Start of Day**

```bash
# 1. Start your day
cd ~/projects/test-automation

# 2. Check current status
git status

# 3. Get latest changes
git checkout main
git pull origin main

# Output:
Already on 'main'
Your branch is up to date with 'origin/main'.
Updating a1b2c3d..e4f5g6h
Fast-forward
 src/test/LoginTests.java | 15 +++++++++++++++
 pom.xml                  |  2 +-
 2 files changed, 16 insertions(+), 1 deletion(-)

# 4. Create feature branch
git checkout -b feature/checkout-tests

# 5. Start working
# ... develop tests ...
```

**Scenario 2: Sync Feature Branch with Latest Main**

```bash
# You're working on feature branch
git checkout feature/api-tests

# Main branch has been updated by teammates
# Sync your feature branch with latest main

# Option 1: Merge (preserves history)
git fetch origin
git merge origin/main

# Option 2: Rebase (linear history)
git fetch origin
git rebase origin/main

# Option 3: Pull main into feature
git pull origin main
```

**Scenario 3: Check Teammate's Work**

```bash
# Teammate pushed new feature branch
# You want to review it locally

# Fetch all remote branches
git fetch --all

# List remote branches
git branch -r

# Output:
  origin/HEAD -> origin/main
  origin/main
  origin/develop
  origin/feature/api-tests
  origin/feature/teammate-work

# Checkout teammate's branch
git checkout -b teammate-work origin/feature/teammate-work

# Or shorter (Git creates tracking branch automatically)
git checkout feature/teammate-work

# Review code
git log --oneline
git diff main..feature/teammate-work

# Run tests locally
mvn clean test

# Provide feedback or approve
```

### 6.6 Remote Tracking Branches

When you clone a repository, Git automatically creates tracking relationships.

```bash
# View tracking relationships
git branch -vv

# Output:
* feature/api-tests  a1b2c3d [origin/feature/api-tests: ahead 2] test: Add API tests
  main               e4f5g6h [origin/main] fix: Update config

# Explanation:
# [origin/feature/api-tests: ahead 2]
#  - Tracking origin/feature/api-tests
#  - Local branch is 2 commits ahead
```

**Set up tracking branch:**

```bash
# When creating new branch
git checkout -b feature/new-tests origin/feature/new-tests

# Set upstream for existing branch
git branch --set-upstream-to=origin/feature/new-tests feature/new-tests

# Or shorter
git branch -u origin/feature/new-tests

# Now you can use 'git pull' and 'git push' without specifying remote/branch
git pull   # Automatically pulls from origin/feature/new-tests
git push   # Automatically pushes to origin/feature/new-tests
```

### 6.7 Best Practices - Remote Repositories

**1. Always Pull Before Starting Work**
```bash
git checkout main
git pull origin main
git checkout -b feature/new-work
```

**2. Fetch Regularly**
```bash
# Set up Git to fetch automatically (optional)
git config --global fetch.prune true

# Fetch multiple times a day
git fetch --prune
```

**3. Use SSH Keys for Authentication**
```bash
# Instead of HTTPS (requires password)
https://github.com/company/repo.git

# Use SSH (no password prompts)
git@github.com:company/repo.git

# Change existing remote to SSH
git remote set-url origin git@github.com:company/repo.git
```

**4. Keep Remote Branches Clean**
```bash
# Delete merged remote branches
git push origin --delete feature/completed-tests

# Prune local references to deleted remote branches
git fetch --prune

# Or configure automatic pruning
git config --global fetch.prune true
```

**5. Verify Remote Before Push**
```bash
# Always verify which remote you're pushing to
git remote -v

# Especially important in fork workflows
git push origin feature/my-tests  # To your fork
git push upstream feature/my-tests  # To original repo (may not have permission!)
```

### 6.8 Common Mistakes to Avoid

**Mistake 1: Not Pulling Before Starting Work**
```bash
# Bad:
git checkout -b feature/new-tests
# ... work ...
git push
# ERROR: remote has changes you don't have

# Good:
git checkout main
git pull origin main
git checkout -b feature/new-tests
```

**Mistake 2: Pulling Into Dirty Working Directory**
```bash
# Bad:
# ... working on files ...
git pull  # May cause conflicts or fail

# Good:
git status  # Check for uncommitted changes
git stash   # Stash changes if needed
git pull
git stash pop  # Restore changes
```

**Mistake 3: Deleting Remote Branch Accidentally**
```bash
# Be careful with this command
git push origin --delete feature/important-work

# If done accidentally, branch can be recovered if someone
# else has it locally or if you know the commit hash
git checkout -b feature/important-work <commit-hash>
git push origin feature/important-work
```

**Mistake 4: Not Understanding Fetch vs Pull**
```bash
# Don't blindly pull
git pull  # Merges immediately, might cause conflicts

# Better: Fetch first, review, then merge
git fetch
git log --oneline main..origin/main
git merge origin/main
```

### 6.9 Interview Questions - Remote Repositories

**Q1: What is the difference between git fetch and git pull?**

**Answer:**
```
git fetch:
- Downloads objects and refs from remote
- Updates remote-tracking branches
- Doesn't modify working directory
- Safe to run anytime
- Allows review before integration

git pull:
- git fetch + git merge combined
- Downloads AND integrates changes
- Modifies working directory
- Can cause merge conflicts
- Less safe but more convenient

Example:
# Fetch (safe)
git fetch origin
git log main..origin/main  # Review changes
git merge origin/main      # Merge when ready

# Pull (convenient)
git pull origin main       # Fetch + merge in one command

When to use each:
- fetch: When you want to see what's new before merging
- pull: When you're confident and want quick update
```

**Q2: What is origin in Git?**

**Answer:**
```
origin:
- Default name for remote repository
- Created automatically when cloning
- Just a naming convention (can be renamed)
- Points to the repository you cloned from

Example:
git clone git@github.com:company/repo.git
# Git automatically creates remote named 'origin'

git remote -v
# origin  git@github.com:company/repo.git (fetch)
# origin  git@github.com:company/repo.git (push)

You can have multiple remotes:
origin    -> Your fork
upstream  -> Original repository
backup    -> Backup server

git remote add upstream git@github.com:original/repo.git
```

**Q3: How do you sync a fork with the original repository?**

**Answer:**
```
Scenario: You forked a test automation framework
You need to get latest changes from original repo

# Step 1: Add original repo as 'upstream'
git remote add upstream git@github.com:original/test-framework.git

# Step 2: Verify remotes
git remote -v
# origin    git@github.com:yourname/test-framework.git (your fork)
# upstream  git@github.com:original/test-framework.git (original)

# Step 3: Fetch from upstream
git fetch upstream

# Step 4: Checkout your main branch
git checkout main

# Step 5: Merge upstream changes
git merge upstream/main

# Step 6: Push to your fork
git push origin main

Complete workflow:
git fetch upstream
git checkout main
git merge upstream/main
git push origin main

# Or using rebase for cleaner history
git fetch upstream
git checkout main
git rebase upstream/main
git push origin main --force-with-lease
```

---

*[Note: The file continues with sections 7-12. This is part 1 of the comprehensive guide.]*

## 7. Pushing and Pulling Changes

### 7.1 Understanding Push Operations

Pushing uploads your local commits to a remote repository.

```
Local Repository          Remote Repository
A---B---C---D            A---B
    ^                        ^
   main                     main

After push:
A---B---C---D            A---B---C---D
    ^                        ^
   main                     main
```

### 7.1.1 Basic Push Operations

```bash
# Push current branch to remote
git push origin main

# Output:
Enumerating objects: 8, done.
Counting objects: 100% (8/8), done.
Delta compression using up to 8 threads
Compressing objects: 100% (5/5), done.
Writing objects: 100% (5/5), 1.23 KiB | 1.23 MiB/s, done.
Total 5 (delta 3), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (3/3), completed with 2 local objects.
To github.com:company/test-automation.git
   a1b2c3d..e4f5g6h  main -> main
```

```bash
# Push and set upstream (first time pushing new branch)
git push -u origin feature/api-tests

# Output:
Total 0 (delta 0), reused 0 (delta 0), pack-reused 0
remote: 
remote: Create a pull request for 'feature/api-tests' on GitHub by visiting:
remote:      https://github.com/company/test-automation/pull/new/feature/api-tests
remote:
To github.com:company/test-automation.git
 * [new branch]      feature/api-tests -> feature/api-tests
Branch 'feature/api-tests' set up to track remote branch 'feature/api-tests' from 'origin'.

# Now subsequent pushes can use just:
git push
```

```bash
# Push all branches
git push --all origin

# Push tags
git push --tags

# Push everything (branches + tags)
git push --all --tags origin
```

### 7.1.2 Force Push (Use with Extreme Caution!)

```bash
# Force push (overwrites remote history)
git push --force origin feature/my-branch

# SAFER: Force push with lease (fails if remote has commits you don't have)
git push --force-with-lease origin feature/my-branch
```

**When to use force push:**
- After rebasing local branch
- Amended commit already pushed
- Only on personal feature branches
- Never on shared branches (main, develop)

**Example:**

```bash
# You amended a commit that was already pushed
git commit --amend -m "Better commit message"

# Normal push fails
git push origin feature/api-tests

# Output:
To github.com:company/test-automation.git
 ! [rejected]        feature/api-tests -> feature/api-tests (non-fast-forward)
error: failed to push some refs to 'github.com:company/test-automation.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes...

# Force push with lease (safer)
git push --force-with-lease origin feature/api-tests

# Output:
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 8 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 356 bytes | 356.00 KiB/s, done.
Total 3 (delta 2), reused 0 (delta 0), pack-reused 0
To github.com:company/test-automation.git
 + e4f5g6h...a1b2c3d feature/api-tests -> feature/api-tests (forced update)
```

### 7.1.3 Push Workflow for Test Automation

**Scenario: Daily Development**

```bash
# Morning: Start work
git checkout main
git pull origin main
git checkout -b feature/checkout-tests

# Work on tests
# Create CheckoutTests.java
vim src/test/java/tests/CheckoutTests.java
# Create CheckoutPage.java
vim src/test/java/pages/CheckoutPage.java

# First commit
git add src/test/java/tests/CheckoutTests.java
git commit -m "test: Add basic checkout test structure"

# Second commit
git add src/test/java/pages/CheckoutPage.java
git commit -m "feat: Add CheckoutPage object model"

# Push to remote (first time)
git push -u origin feature/checkout-tests

# Continue working
# Add more tests
vim src/test/java/tests/CheckoutTests.java
git add src/test/java/tests/CheckoutTests.java
git commit -m "test: Add payment validation tests"

# Subsequent pushes (upstream already set)
git push

# End of day
git status  # Ensure everything committed
git push    # Push any remaining commits
```

### 7.2 Pull Strategies

#### 7.2.1 Pull with Merge (Default)

```bash
# Pull and merge
git pull origin main

# Creates merge commit if there are divergent changes
```

```
Before pull:
Local:   A---B---C
                 ^
                main

Remote:  A---B---D---E
                     ^
                   origin/main

After pull (merge):
Local:   A---B---C-------M
             \         /
              D---E---
                      ^
                     main
```

#### 7.2.2 Pull with Rebase

```bash
# Pull with rebase
git pull --rebase origin main

# Or configure default pull strategy
git config --global pull.rebase true
```

```
Before pull:
Local:   A---B---C
                 ^
                main

Remote:  A---B---D---E
                     ^
                   origin/main

After pull (rebase):
Local:   A---B---D---E---C'
                     ^
                    main
```

**Rebase creates cleaner linear history but rewrites commits.**

#### 7.2.3 Choosing Pull Strategy

**Use Merge When:**
- Working on shared branch
- Want to preserve exact history
- Team uses merge-based workflow

**Use Rebase When:**
- Working on personal feature branch
- Want clean linear history
- Preparing branch for pull request

**Example Workflow:**

```bash
# On feature branch, prefer rebase
git checkout feature/api-tests
git pull --rebase origin main  # Rebase feature on latest main

# On main branch, use merge
git checkout main
git pull origin main  # Merge (default)
```

### 7.3 Handling Push/Pull Errors

#### 7.3.1 Error: "Updates were rejected"

```bash
git push origin main

# Output:
To github.com:company/test-automation.git
 ! [rejected]        main -> main (non-fast-forward)
error: failed to push some refs to 'github.com:company/test-automation.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
```

**Solution:**

```bash
# Pull first to get remote changes
git pull origin main

# If there are merge conflicts, resolve them
# Then push
git push origin main
```

#### 7.3.2 Error: "fatal: refusing to merge unrelated histories"

```bash
git pull origin main

# Output:
fatal: refusing to merge unrelated histories
```

**Solution:**

```bash
# Allow merging unrelated histories
git pull origin main --allow-unrelated-histories

# Then resolve any conflicts and commit
```

#### 7.3.3 Error: "You have divergent branches"

```bash
git status

# Output:
On branch main
Your branch and 'origin/main' have diverged,
and have 2 and 3 different commits each, respectively.
  (use "git pull" to merge the remote branch into yours)
```

**Solutions:**

```bash
# Option 1: Merge remote changes
git pull origin main
# Resolve conflicts if any
git push origin main

# Option 2: Rebase on remote
git pull --rebase origin main
# Resolve conflicts if any
git push origin main

# Option 3: Reset to remote (LOSES local commits!)
git fetch origin
git reset --hard origin/main
```

### 7.4 Working with Multiple Remotes

Useful when working with forks or multiple backup locations.

```bash
# Add original repository as upstream
git remote add upstream git@github.com:original-owner/test-framework.git

# Add backup remote
git remote add backup git@gitlab.com:company/test-framework.git

# View all remotes
git remote -v

# Output:
origin    git@github.com:yourname/test-framework.git (fetch)
origin    git@github.com:yourname/test-framework.git (push)
upstream  git@github.com:original-owner/test-framework.git (fetch)
upstream  git@github.com:original-owner/test-framework.git (push)
backup    git@gitlab.com:company/test-framework.git (fetch)
backup    git@gitlab.com:company/test-framework.git (push)
```

**Workflow:**

```bash
# Fetch from all remotes
git fetch --all

# Push to multiple remotes
git push origin main
git push backup main

# Pull from upstream (original repo)
git pull upstream main

# Push to your fork
git push origin main
```

### 7.5 Best Practices - Push/Pull

**1. Pull Before Push**
```bash
# Always pull latest changes before pushing
git pull origin main
# Resolve any conflicts
git push origin main
```

**2. Commit Before Pull**
```bash
# Don't pull into dirty working directory
git status  # Check for uncommitted changes

# If you have changes, commit or stash
git stash
git pull origin main
git stash pop
```

**3. Use Meaningful Branch Names When Pushing**
```bash
# Good: Clear what the branch contains
git push origin feature/api-authentication-tests

# Bad: Unclear branch name
git push origin test123
```

**4. Verify Before Force Push**
```bash
# Never force push to shared branches
# Only force push to your own feature branches

# Prefer --force-with-lease over --force
git push --force-with-lease origin feature/my-branch

# This fails if remote has commits you don't have locally
```

**5. Set Up Branch Tracking**
```bash
# First push: set upstream
git push -u origin feature/new-tests

# Subsequent pushes: simple
git push
git pull
```

### 7.6 Real-World Push/Pull Scenarios

**Scenario 1: Collaborative Feature Development**

```bash
# You and teammate both work on same feature branch

# You push changes
git push origin feature/api-tests

# Teammate also pushed changes meanwhile
# Your next push fails

git push origin feature/api-tests

# Output:
To github.com:company/test-automation.git
 ! [rejected]        feature/api-tests -> feature/api-tests (non-fast-forward)

# Solution: Pull first
git pull origin feature/api-tests

# If merge conflicts occur
# Output:
Auto-merging src/test/java/tests/APITests.java
CONFLICT (content): Merge conflict in src/test/java/tests/APITests.java
Automatic merge failed; fix conflicts and then commit the result.

# Resolve conflicts (covered in Section 9)
# ... fix conflicts ...
git add src/test/java/tests/APITests.java
git commit -m "Merge teammate changes"

# Now push
git push origin feature/api-tests
```

**Scenario 2: Syncing Fork with Original Repository**

```bash
# You forked a test framework

# Add original repo as upstream
git remote add upstream git@github.com:original/test-framework.git

# Fetch from upstream
git fetch upstream

# Checkout your main
git checkout main

# Merge upstream changes
git merge upstream/main

# Push to your fork
git push origin main

# Your fork is now synced with original repository
```

**Scenario 3: Pushing from CI/CD Pipeline**

```bash
# In CI/CD script (e.g., Jenkins, GitLab CI)

#!/bin/bash
# Run tests
mvn clean test

# Generate test reports
mvn surefire-report:report

# Commit test results to a separate branch
git checkout -b test-results/build-${BUILD_NUMBER}
git add target/surefire-reports/
git commit -m "Test results for build ${BUILD_NUMBER}"

# Push to remote
git push origin test-results/build-${BUILD_NUMBER}

# Or push to specific reports repository
git push reports-repo main
```

### 7.7 Common Mistakes to Avoid

**Mistake 1: Force Pushing to Shared Branches**
```bash
# NEVER do this:
git push --force origin main
git push --force origin develop

# This rewrites history for everyone and causes chaos
```

**Mistake 2: Not Checking Status Before Push**
```bash
# Bad:
git push

# Good:
git status  # What will be pushed?
git log --oneline origin/main..main  # Commits to be pushed
git push
```

**Mistake 3: Pulling Into Uncommitted Changes**
```bash
# Bad:
# ... working on files ...
git pull  # May cause conflicts or lose changes

# Good:
git status
git commit -am "WIP: Work in progress"
git pull
# OR
git stash
git pull
git stash pop
```

**Mistake 4: Pushing Sensitive Information**
```bash
# Bad: Committing and pushing credentials
git add config/database.properties  # Contains passwords!
git commit -m "Add config"
git push origin main

# If this happens:
# 1. Immediately remove file
git rm config/database.properties
echo "config/database.properties" >> .gitignore
git commit -m "Remove sensitive file"
git push origin main

# 2. Rotate credentials (passwords, API keys)
# 3. Consider tools like git-filter-repo to remove from history
```

### 7.8 Interview Questions - Push/Pull

**Q1: What happens when you git push?**

**Answer:**
```
Push Process:
1. Git compares local branch with remote branch
2. Calculates missing commits on remote
3. Compresses and packages objects
4. Sends data to remote repository
5. Remote validates and stores data
6. Remote updates branch references
7. Local tracking branch is updated

Example:
git push origin main

Steps:
1. Check local main vs origin/main
2. Find commits only in local
3. Package commits, trees, blobs
4. Transfer to GitHub/GitLab server
5. Server updates origin/main
6. Success message displayed

Requirements for successful push:
- Fast-forward merge (or force push)
- Valid authentication
- Sufficient permissions
- Network connectivity
```

**Q2: Explain the difference between git pull and git fetch + git merge.**

**Answer:**
```
git pull = git fetch + git merge

git fetch:
- Downloads commits from remote
- Updates remote-tracking branches (origin/main)
- Doesn't modify working directory
- Safe operation

git merge:
- Integrates changes into current branch
- Modifies working directory
- Can cause conflicts
- Creates merge commit if needed

git pull:
- Combines both operations
- Less control over merge process
- Convenient but sometimes risky

Example:

Using git pull:
git pull origin main
# Fetches and merges in one step

Equivalent manual process:
git fetch origin
git merge origin/main

Advantage of separate commands:
git fetch origin
git log --oneline main..origin/main  # Review changes
git diff main origin/main             # See differences
git merge origin/main                 # Merge when ready

When to use:
- pull: Quick update when you trust the changes
- fetch + merge: When you want to review before integrating
```

**Q3: When should you use --force-with-lease vs --force?**

**Answer:**
```
--force:
- Unconditionally overwrites remote branch
- Dangerous: Can lose commits made by others
- Use only when absolutely sure

--force-with-lease:
- Overwrites only if remote hasn't changed
- Safer: Fails if someone else pushed
- Preferred for force pushing

Example:

Scenario: Rebased commits

# Rebased local commits
git rebase -i origin/main

# Push fails (history rewritten)
git push origin feature/my-branch
# ! [rejected] non-fast-forward

# Option 1: Force (DANGEROUS)
git push --force origin feature/my-branch
# Overwrites anything on remote!

# Option 2: Force with lease (SAFER)
git push --force-with-lease origin feature/my-branch
# Fails if remote has commits you don't have locally

Protection:
--force-with-lease prevents:
1. Overwriting teammate's commits
2. Losing work pushed from another machine
3. Race conditions in team environment

Rules:
- NEVER use --force on shared branches (main, develop)
- Use --force-with-lease on personal feature branches
- Only after rebase or amend operations
- Coordinate with team if multiple people on same branch
```

---

## 8. Pull Requests and Code Review

### 8.1 Understanding Pull Requests

A Pull Request (PR) or Merge Request (MR in GitLab) is a request to merge changes from one branch into another.

```
Your Branch:     feature/api-tests
                      |
                      | Create PR
                      ▼
Target Branch:   main ◄──── Review ◄──── Approve ◄──── Merge
```

**Benefits:**
- Code review before merging
- Discussion and collaboration
- CI/CD automated testing
- Quality gate enforcement
- Knowledge sharing

### 8.1.1 Pull Request Workflow

```
1. Create Feature Branch
   └─> 2. Develop & Commit
        └─> 3. Push to Remote
             └─> 4. Create Pull Request
                  └─> 5. Code Review
                       └─> 6. Address Feedback
                            └─> 7. Approval
                                 └─> 8. Merge
                                      └─> 9. Delete Branch
```

### 8.2 Creating a Pull Request

#### 8.2.1 Via GitHub Web Interface

```bash
# Step 1: Push your feature branch
git push -u origin feature/api-tests

# Output:
remote: Create a pull request for 'feature/api-tests' on GitHub by visiting:
remote:      https://github.com/company/test-automation/pull/new/feature/api-tests
```

**On GitHub:**
1. Navigate to repository
2. Click "Compare & pull request" button
3. Fill in PR details:
   - Title: Clear, concise summary
   - Description: What, why, how
   - Reviewers: Add team members
   - Labels: bug, feature, test, etc.
   - Milestone: Sprint or release
4. Click "Create pull request"

#### 8.2.2 Pull Request Template Example

```markdown
## Description
Brief description of changes.

## Type of Change
- [ ] Bug fix
- [x] New feature (test suite)
- [ ] Breaking change
- [ ] Documentation update

## What was added/changed
- Added comprehensive API authentication tests
- Implemented REST Assured framework integration
- Created reusable API helper methods
- Added test data management for API tests

## Test Plan
- [x] All tests pass locally
- [x] Added new test cases
- [x] Updated existing tests if needed
- [x] No failing tests in test report

## Test Coverage
- Previous coverage: 65%
- Current coverage: 78%
- New tests added: 15

## Related Issues
Closes #234
Related to #230, #235

## Screenshots (if applicable)
[Test execution report screenshot]

## Checklist
- [x] Code follows project style guidelines
- [x] Self-review completed
- [x] Comments added for complex logic
- [x] Documentation updated
- [x] No console errors
- [x] Tests added/updated
```

#### 8.2.3 Using GitHub CLI

```bash
# Install GitHub CLI
# macOS
brew install gh

# Windows
choco install gh

# Login
gh auth login

# Create PR from command line
gh pr create --title "Add API authentication tests" --body "Implements REST Assured tests for authentication endpoints"

# Or interactive mode
gh pr create

# Output:
? Where should we push the 'feature/api-tests' branch? yourname/test-automation

Creating pull request for yourname:feature/api-tests into main in company/test-automation

? Title Add API authentication tests
? Body <Received>
? What's next? Submit
https://github.com/company/test-automation/pull/123

# List your PRs
gh pr list

# Output:
#123  Add API authentication tests  feature/api-tests

# View PR details
gh pr view 123

# Check PR status
gh pr status
```

### 8.3 Code Review Process

#### 8.3.1 Reviewer Responsibilities

```bash
# Checkout PR locally for review
git fetch origin
git checkout feature/api-tests

# Or using GitHub CLI
gh pr checkout 123

# Review code changes
git diff main..feature/api-tests

# Review file by file
git diff main..feature/api-tests -- src/test/java/tests/APITests.java

# Run tests locally
mvn clean test

# Check test reports
open target/surefire-reports/index.html
```

**Review Checklist:**

```markdown
Code Quality:
- [ ] Code is readable and well-structured
- [ ] Follows project coding standards
- [ ] No code smells or anti-patterns
- [ ] Appropriate use of design patterns

Test Quality:
- [ ] Tests are independent and isolated
- [ ] No flaky tests
- [ ] Good assertions (not just presence checks)
- [ ] Test data is properly managed
- [ ] Tests clean up after themselves

Best Practices:
- [ ] No hardcoded values
- [ ] Proper waits (explicit, not implicit)
- [ ] Page Object Model followed
- [ ] Reusable methods extracted
- [ ] No duplicate code

Documentation:
- [ ] Clear test method names
- [ ] Comments for complex logic
- [ ] README updated if needed
- [ ] Test data documented

Performance:
- [ ] Tests run in reasonable time
- [ ] No unnecessary waits
- [ ] Proper use of test annotations (@BeforeClass, etc.)
```

#### 8.3.2 Providing Review Comments

**Good Review Comments:**

```markdown
# Constructive
"Consider extracting this locator to the Page Object. This will improve maintainability if the selector changes."

# Specific
"Line 45: This assertion only checks for element presence. Should we also verify the text content?"

# Question-based
"What happens if the API returns a 500 error here? Should we add negative test cases?"

# Suggestion with example
"Instead of Thread.sleep(), consider using WebDriverWait:
```java
WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
wait.until(ExpectedConditions.elementToBeClickable(loginButton));
```
"

# Praise good work
"Great job implementing data-driven tests! This significantly improves our test coverage."
```

**Poor Review Comments:**

```markdown
# Too vague
"This is wrong"
"Fix this"

# Not constructive
"This code is terrible"

# No explanation
"Change this"

# Overly critical
"Why would you do it this way? This makes no sense."
```

#### 8.3.3 Addressing Review Feedback

```bash
# Make requested changes
vim src/test/java/tests/APITests.java

# Commit changes
git add src/test/java/tests/APITests.java
git commit -m "Address PR review: Improve API error handling tests"

# Push updates
git push origin feature/api-tests

# PR automatically updates with new commits

# Add comment in PR
# "Updated error handling tests as suggested. Please review."

# Mark conversations as resolved after addressing
```

### 8.4 Pull Request Best Practices

#### 8.4.1 Size and Scope

```bash
# Bad: Huge PR with multiple unrelated changes
# 50 files changed, 5000 lines added
feature/massive-update
├── API tests
├── UI tests
├── Framework refactoring
├── Configuration changes
└── Documentation updates

# Good: Focused PRs
# 5 files changed, 200 lines added
feature/api-authentication-tests
├── APIAuthTests.java
├── APIHelper.java
└── auth-test-data.json

feature/login-ui-tests
├── LoginTests.java
└── LoginPage.java
```

**Guidelines:**
- Keep PRs small (< 400 lines changed)
- One feature/fix per PR
- Break large features into multiple PRs
- Easier to review, faster approval

#### 8.4.2 PR Title and Description

**Good Titles:**

```
feat: Add API authentication tests using REST Assured
fix: Correct login button locator in LoginPage
test: Add checkout flow end-to-end tests
refactor: Extract common waits to BasePage
docs: Update README with Cucumber setup instructions
```

**Good Description:**

```markdown
## Summary
Implements comprehensive API authentication tests using REST Assured framework.

## Changes
- Added `APIAuthTests.java` with token-based auth tests
- Created `APIHelper.java` with reusable REST client methods
- Added test data files for various auth scenarios
- Updated `pom.xml` with REST Assured dependencies

## Why
Our API layer lacked automated tests. This PR adds tests for:
- Login endpoint
- Token generation
- Token validation
- Session management
- Logout functionality

## Test Results
All 15 tests passing:
- Positive scenarios: 8
- Negative scenarios: 5
- Edge cases: 2

## How to Test
```bash
mvn clean test -Dtest=APIAuthTests
```

## Screenshots
[Test execution report]

## Related Issues
Closes #234 - Add API test automation
Related to #230 - REST Assured integration
```

#### 8.4.3 Draft Pull Requests

```bash
# Create draft PR (work in progress)
gh pr create --draft --title "WIP: API authentication tests"

# Or on GitHub: Click "Create draft pull request"

# When ready for review
gh pr ready 123

# Or on GitHub: Click "Ready for review"
```

**When to use Draft PRs:**
- Early feedback on approach
- Show work progress to team
- CI/CD runs to catch issues early
- Not ready for full review yet

### 8.5 Merging Strategies

#### 8.5.1 Merge Commit (Default)

```
Before:
main:    A---B---C
          \
feature:   D---E---F

After:
main:    A---B---C-------M
          \             /
feature:   D---E---F---
```

**Pros:**
- Preserves complete history
- Shows when feature was merged
- Easy to revert entire feature

**Cons:**
- Cluttered history with merge commits
- Harder to follow linear history

#### 8.5.2 Squash and Merge

```
Before:
main:    A---B---C
          \
feature:   D---E---F

After:
main:    A---B---C---DEF
```

**Pros:**
- Clean linear history
- One commit per feature
- Clear main branch history

**Cons:**
- Loses individual commit history
- Can't cherry-pick individual commits

**Best for:** Feature branches with many WIP commits

#### 8.5.3 Rebase and Merge

```
Before:
main:    A---B---C
          \
feature:   D---E---F

After:
main:    A---B---C---D'---E'---F'
```

**Pros:**
- Linear history
- Preserves individual commits
- No merge commits

**Cons:**
- Rewrites history
- More complex for beginners

**Best for:** Clean feature branches with meaningful commits

#### 8.5.4 Choosing Merge Strategy

**Project Decision:**

```bash
# Configure default for repository (in GitHub settings)
# Settings → Options → Pull Requests

# Allow merge commits
☑ Allow merge commits

# Allow squash merging
☑ Allow squash merging

# Allow rebase merging
☑ Allow rebase merging

# Automatically delete head branches
☑ Automatically delete head branches after pull requests are merged
```

**Team Guidelines:**

```markdown
Our Test Automation Framework Merge Strategy:

1. Feature branches → main: Squash and merge
   - Keeps main history clean
   - One commit per feature

2. Hotfix branches → main: Merge commit
   - Preserves urgency indication
   - Clear what was emergency fix

3. Release branches → main: Merge commit
   - Preserves release boundaries
   - Easy to track releases

4. Personal preference on feature branches during development:
   - Rebase frequently to stay updated with main
   - Squash merge when PR approved
```

### 8.6 Protected Branches and Branch Rules

```bash
# GitHub Settings → Branches → Add rule

# Protection settings for 'main':
☑ Require pull request reviews before merging
  - Required approvals: 1

☑ Require status checks to pass before merging
  - tests/junit
  - code-coverage
  - lint

☑ Require conversation resolution before merging

☑ Require linear history

☑ Include administrators

☑ Restrict who can push to matching branches
  - Only: qa-leads team
```

**Benefits:**
- Prevents direct pushes to main
- Enforces code review
- Runs CI/CD before merge
- Maintains quality standards

### 8.7 CI/CD Integration with PRs

**Example: GitHub Actions workflow**

```yaml
# .github/workflows/pr-tests.yml
name: PR Test Suite

on:
  pull_request:
    branches: [ main, develop ]

jobs:
  test:
    runs-on: ubuntu-latest
    
    steps:
    - name: Checkout code
      uses: actions/checkout@v3
    
    - name: Set up JDK 11
      uses: actions/setup-java@v3
      with:
        java-version: '11'
        distribution: 'adopt'
    
    - name: Run tests
      run: mvn clean test
    
    - name: Generate test report
      if: always()
      uses: dorny/test-reporter@v1
      with:
        name: Test Results
        path: target/surefire-reports/*.xml
        reporter: java-junit
    
    - name: Check code coverage
      run: mvn jacoco:report
    
    - name: Comment PR with results
      uses: actions/github-script@v6
      with:
        script: |
          github.rest.issues.createComment({
            issue_number: context.issue.number,
            owner: context.repo.owner,
            repo: context.repo.repo,
            body: '✅ All tests passed! 🎉'
          })
```

**PR automatically shows:**
- ✅ All checks passed
- ❌ Tests failed (with details)
- 📊 Code coverage report
- ⚠️ Quality gate failed

### 8.8 Common PR Mistakes to Avoid

**Mistake 1: PRs Too Large**
```
Bad: 50 files, 3000 lines changed
Good: 5 files, 200 lines changed

Break large features into smaller PRs
```

**Mistake 2: Poor PR Description**
```
Bad: "Updates"
Bad: "Fixed stuff"
Bad: "Changes"

Good: Clear title and description explaining what, why, how
```

**Mistake 3: Not Responding to Review Comments**
```
Bad: Ignore review comments
Bad: Mark as resolved without changes

Good: Address each comment or explain why not
Good: Have discussion in PR thread
```

**Mistake 4: Merging Without Approval**
```
Bad: Merge your own PR without review
Bad: Bypass required approvals

Good: Wait for required approvals
Good: Request re-review after major changes
```

**Mistake 5: Not Updating PR After Main Changes**
```
Bad: PR created weeks ago, main moved forward, merge conflicts

Good: Regularly update PR with latest main
git checkout feature/my-branch
git merge main  # Or rebase
git push
```

### 8.9 Interview Questions - Pull Requests

**Q1: What is a Pull Request and why is it important?**

**Answer:**
```
Pull Request (PR):
- Request to merge changes from one branch into another
- Facilitates code review before integration
- Platform for discussion and collaboration

Importance:
1. Code Quality: Review catches bugs and issues
2. Knowledge Sharing: Team learns from each other's code
3. Documentation: PR describes changes and reasoning
4. Accountability: Clear record of who changed what
5. CI/CD Integration: Automated testing before merge

Example Workflow:
1. Developer creates feature branch
2. Implements API tests
3. Pushes branch and creates PR
4. Teammates review code
5. Automated tests run
6. Developer addresses feedback
7. PR approved and merged
8. Feature branch deleted

In Test Automation:
- Review test coverage
- Check test quality (assertions, data management)
- Verify no flaky tests introduced
- Ensure tests follow framework standards
- Share testing approaches and techniques
```

**Q2: What makes a good Pull Request?**

**Answer:**
```
Good PR Characteristics:

1. Size:
- Small and focused (< 400 lines changed)
- Single feature or fix
- Easy to review in 15-30 minutes

2. Description:
- Clear title following conventions
- Detailed description of changes
- Why the change was made
- How to test the changes
- Related issues/tickets

3. Code Quality:
- Follows coding standards
- Well-tested (includes tests)
- No unrelated changes
- Clean commit history

4. Communication:
- Responds to review comments
- Asks questions when unclear
- Updates PR based on feedback
- Links to relevant documentation

Example Good PR:

Title:
"feat: Add API authentication tests using REST Assured"

Description:
"Implements comprehensive API auth tests covering:
- Login endpoint testing
- Token generation and validation
- Session management
- Error handling

Test coverage increased from 60% to 75%
All tests passing in CI/CD

Closes #234
Related to #230, #235

Test execution:
mvn clean test -Dtest=APIAuthTests"

5. Testing:
- All tests pass locally
- CI/CD checks pass
- No decrease in code coverage
- Added tests for new functionality
```

**Q3: Explain the difference between merge strategies: Merge commit, Squash, and Rebase.**

**Answer:**
```
1. Merge Commit:
Creates merge commit preserving all history

Before:
main:    A---B---C
          \
feature:   D---E---F

After:
main:    A---B---C-------M
          \             /
feature:   D---E---F---

Pros:
- Preserves complete history
- Shows feature branch structure
- Easy to revert entire feature

Cons:
- Cluttered history
- Many merge commits

Use when: You want to preserve detailed history

2. Squash and Merge:
Combines all commits into one

After:
main:    A---B---C---DEF

Pros:
- Clean linear history
- One commit per feature
- Clear main branch

Cons:
- Loses individual commit details
- Can't cherry-pick specific commits

Use when: Feature has many WIP commits

3. Rebase and Merge:
Replays commits on top of main

After:
main:    A---B---C---D'---E'---F'

Pros:
- Linear history
- Preserves meaningful commits
- No merge commits

Cons:
- Rewrites history
- More complex

Use when: Clean commit history on feature branch

Example Decision:
Project standard:
- API test features: Squash (many iterations)
- Framework improvements: Merge commit (preserve decisions)
- Quick fixes: Rebase (keep linear)
```

---

## 9. Resolving Merge Conflicts

### 9.1 Understanding Merge Conflicts

Merge conflicts occur when Git can't automatically merge changes because the same part of a file was modified differently in two branches.

```
main branch:        A---B---C (modified login.java line 10)
                     \
feature branch:       D---E (also modified login.java line 10 differently)

When merging E into C: CONFLICT!
```

**Common Conflict Scenarios:**
1. Two people edit same lines
2. One deletes file, another modifies it
3. Both branches rename same file differently
4. Binary file conflicts

### 9.1.1 When Conflicts Occur

```bash
# Merge feature into main
git checkout main
git merge feature/api-tests

# Output:
Auto-merging src/test/java/tests/LoginTests.java
CONFLICT (content): Merge conflict in src/test/java/tests/LoginTests.java
Auto-merging src/test/java/pages/LoginPage.java
CONFLICT (content): Merge conflict in src/test/java/pages/LoginPage.java
Automatic merge failed; fix conflicts and then commit the result.
```

```bash
# Check status
git status

# Output:
On branch main
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
        both modified:   src/test/java/pages/LoginPage.java
        both modified:   src/test/java/tests/LoginTests.java

no changes added to commit (use "git add" and/or "git commit -a")
```

### 9.2 Anatomy of a Merge Conflict

**Conflict Markers in File:**

```java
// src/test/java/tests/LoginTests.java

public class LoginTests {
    
    @Test
    public void testLogin() {
<<<<<<< HEAD (Current Change - main branch)
        loginPage.enterUsername("admin@test.com");
        loginPage.enterPassword("Admin@123");
        loginPage.clickLoginButton();
        
        Assert.assertTrue(homePage.isWelcomeMessageDisplayed());
=======
(Incoming Change - feature/api-tests branch)
        loginPage.login("admin@test.com", "Admin@123");
        
        HomePage home = new HomePage(driver);
        Assert.assertTrue(home.isUserLoggedIn());
        Assert.assertEquals(home.getUserName(), "Admin User");
>>>>>>> feature/api-tests
    }
}
```

**Conflict Markers Explained:**

```
<<<<<<< HEAD
  Your current branch changes (main)
=======
  Incoming branch changes (feature/api-tests)
>>>>>>> feature/api-tests
```

### 9.3 Resolving Conflicts

#### 9.3.1 Manual Resolution

```java
// Option 1: Keep HEAD (current branch) changes
@Test
public void testLogin() {
    loginPage.enterUsername("admin@test.com");
    loginPage.enterPassword("Admin@123");
    loginPage.clickLoginButton();
    
    Assert.assertTrue(homePage.isWelcomeMessageDisplayed());
}

// Option 2: Keep incoming (feature branch) changes
@Test
public void testLogin() {
    loginPage.login("admin@test.com", "Admin@123");
    
    HomePage home = new HomePage(driver);
    Assert.assertTrue(home.isUserLoggedIn());
    Assert.assertEquals(home.getUserName(), "Admin User");
}

// Option 3: Combine both (best solution)
@Test
public void testLogin() {
    // Use the cleaner login method from feature branch
    loginPage.login("admin@test.com", "Admin@123");
    
    // Keep all assertions from both branches
    HomePage home = new HomePage(driver);
    Assert.assertTrue(home.isWelcomeMessageDisplayed());
    Assert.assertTrue(home.isUserLoggedIn());
    Assert.assertEquals(home.getUserName(), "Admin User");
}

// Option 4: Complete rewrite (if both are wrong)
@Test
public void testLogin() {
    // Better approach combining best of both
    loginPage.performLogin("admin@test.com", "Admin@123");
    
    // Comprehensive assertions
    Assert.assertTrue(homePage.isDisplayed());
    Assert.assertTrue(homePage.hasWelcomeMessage());
    Assert.assertEquals(homePage.getLoggedInUser(), "Admin User");
}
```

#### 9.3.2 Conflict Resolution Steps

```bash
# Step 1: Identify conflicted files
git status

# Output:
Unmerged paths:
  both modified:   src/test/java/pages/LoginPage.java
  both modified:   src/test/java/tests/LoginTests.java

# Step 2: Open conflicted file in editor
code src/test/java/tests/LoginTests.java

# Step 3: Resolve conflicts (remove markers, choose/combine changes)
# ... edit file ...

# Step 4: Mark as resolved
git add src/test/java/tests/LoginTests.java

# Step 5: Repeat for all conflicted files
code src/test/java/pages/LoginPage.java
# ... resolve ...
git add src/test/java/pages/LoginPage.java

# Step 6: Verify all conflicts resolved
git status

# Output:
On branch main
All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)

Changes to be committed:
        modified:   src/test/java/pages/LoginPage.java
        modified:   src/test/java/tests/LoginTests.java

# Step 7: Complete the merge
git commit

# Default merge message opens in editor:
Merge branch 'feature/api-tests'

# Conflicts:
#       src/test/java/pages/LoginPage.java
#       src/test/java/tests/LoginTests.java

# Can add more details about resolution
Merge branch 'feature/api-tests'

Resolved conflicts in login test implementation:
- Combined login method from feature branch with assertions from main
- Kept improved page object methods
- Maintained all test coverage from both branches

# Step 8: Verify merge successful
git log --oneline --graph -5
```

### 9.4 Using Merge Tools

#### 9.4.1 Configure Merge Tool

```bash
# VS Code as merge tool
git config --global merge.tool vscode
git config --global mergetool.vscode.cmd 'code --wait $MERGED'

# Or other tools
# Meld
git config --global merge.tool meld

# P4Merge
git config --global merge.tool p4merge

# KDiff3
git config --global merge.tool kdiff3

# Beyond Compare
git config --global merge.tool bc3
```

#### 9.4.2 Launch Merge Tool

```bash
# When conflicts occur
git merge feature/api-tests
# CONFLICT in src/test/LoginTests.java

# Launch merge tool
git mergetool

# Output:
Merging:
src/test/java/tests/LoginTests.java

Normal merge conflict for 'src/test/java/tests/LoginTests.java':
  {local}: modified file
  {remote}: modified file

# VS Code opens with 3-way merge view:
# - LEFT: Your changes (HEAD)
# - RIGHT: Incoming changes (feature branch)
# - BOTTOM: Result (edit this)
```

**VS Code Merge View:**

```
╔═══════════════════════════════════════════════════════════════╗
║ Current Change (main)          │ Incoming Change (feature)    ║
║════════════════════════════════│══════════════════════════════║
║ loginPage.enterUsername(...);  │ loginPage.login(...);        ║
║ loginPage.enterPassword(...);  │ HomePage home = new ...;     ║
║ loginPage.clickLoginButton();  │ Assert.assertTrue(...);      ║
║ Assert.assertTrue(...);        │ Assert.assertEquals(...);    ║
║════════════════════════════════│══════════════════════════════║
║                                                                ║
║ [Accept Current] [Accept Incoming] [Accept Both] [Compare]    ║
║                                                                ║
║════════════════════════════════════════════════════════════════
║ Result:                                                        ║
║ loginPage.login("admin@test.com", "Admin@123");                ║
║ HomePage home = new HomePage(driver);                          ║
║ Assert.assertTrue(home.isWelcomeMessageDisplayed());           ║
║ Assert.assertTrue(home.isUserLoggedIn());                      ║
║ Assert.assertEquals(home.getUserName(), "Admin User");         ║
╚═══════════════════════════════════════════════════════════════╝
```

```bash
# After resolving in merge tool, close it

# Mark as resolved
git add src/test/java/tests/LoginTests.java

# Complete merge
git commit
```

### 9.5 Preventing Merge Conflicts

#### 9.5.1 Best Practices

**1. Pull Frequently**
```bash
# Daily routine
git checkout main
git pull origin main

# While on feature branch
git checkout feature/my-tests
git merge main  # Or rebase
```

**2. Small, Focused Changes**
```bash
# Bad: Large feature branch over weeks
feature/massive-changes (created 4 weeks ago, 100 files changed)

# Good: Small, incremental PRs
feature/login-tests (2 days, 5 files)
feature/api-auth (3 days, 6 files)
```

**3. Communicate with Team**
```markdown
Team Chat:
"I'm working on LoginPage.java for the next 2 hours. 
Please avoid modifying it to prevent conflicts."

OR

"Refactoring BasePage tomorrow. If you need to modify it, 
let's coordinate."
```

**4. Divide Work Strategically**
```
Team of 3 QA Engineers:

Engineer 1: UI Login Tests
  - LoginTests.java
  - LoginPage.java

Engineer 2: API Tests
  - APITests.java
  - APIHelper.java

Engineer 3: Framework Utilities
  - TestBase.java
  - ConfigReader.java

Minimal overlap = Minimal conflicts
```

**5. Use Feature Flags**
```java
// Instead of long-lived branches, use feature flags
public class TestConfig {
    public static boolean isNewLoginFlowEnabled() {
        return Boolean.parseBoolean(
            System.getProperty("feature.newLoginFlow", "false")
        );
    }
}

@Test
public void testLogin() {
    if (TestConfig.isNewLoginFlowEnabled()) {
        // New implementation
        loginPage.performLogin(user, pass);
    } else {
        // Old implementation (stable)
        loginPage.enterCredentialsAndLogin(user, pass);
    }
}

// Merge to main without conflicts
// Enable flag when ready: -Dfeature.newLoginFlow=true
```

#### 9.5.2 Rebase to Keep Branch Updated

```bash
# Instead of merge, use rebase for cleaner history

# Your feature branch
git checkout feature/api-tests

# Rebase on latest main
git fetch origin
git rebase origin/main

# If conflicts, resolve them
# ... fix conflicts ...
git add <resolved-files>
git rebase --continue

# Force push (rewritten history)
git push --force-with-lease origin feature/api-tests
```

**Rebase vs Merge for Updates:**

```
Merge main into feature:
main:    A---B---C---D
          \         \
feature:   E---F-----M

Rebase feature on main:
main:    A---B---C---D
                      \
feature:               E'---F'

Rebase advantages:
- Linear history
- Conflicts resolved incrementally
- Cleaner eventual merge to main
```

### 9.6 Aborting a Merge

```bash
# If conflicts too complex or not ready to resolve
git merge --abort

# Returns to pre-merge state
git status
# Output:
On branch main
nothing to commit, working tree clean

# All conflict markers removed
# Working directory restored
```

```bash
# During rebase, can also abort
git rebase --abort
```

### 9.7 Real-World Conflict Scenarios

**Scenario 1: Page Object Locator Conflict**

```java
// Conflict in LoginPage.java

<<<<<<< HEAD (main branch)
private By usernameField = By.id("username");
private By passwordField = By.id("password");
private By loginButton = By.cssSelector("button[type='submit']");
=======
(feature/ui-updates branch)
private By usernameField = By.xpath("//input[@name='email']");
private By passwordField = By.xpath("//input[@name='pwd']");
private By loginButton = By.xpath("//button[contains(text(),'Login')]");
>>>>>>> feature/ui-updates
```

**Resolution:**

```java
// Application changed: Check actual UI
// Open application in browser
// Inspect elements

// Decision: Use new locators from feature branch
// But improve them with better selectors
private By usernameField = By.cssSelector("input[name='email']");
private By passwordField = By.cssSelector("input[name='pwd']");
private By loginButton = By.cssSelector("button.login-btn");

// Add to commit message:
// "Resolved locator conflict - application UI changed
// Used CSS selectors for better performance"
```

**Scenario 2: Test Data Conflict**

```yaml
# test-data.yaml conflict

<<<<<<< HEAD
login_users:
  - username: admin@test.com
    password: Admin@123
  - username: user@test.com
    password: User@123
=======
test_users:
  admin:
    email: admin@example.com
    password: SecurePass123
  regular:
    email: user@example.com
    password: UserPass456
>>>>>>> feature/test-data-refactor
```

**Resolution:**

```yaml
# Keep new structure from feature branch (better organized)
# But preserve test data from main branch
test_users:
  admin:
    email: admin@test.com
    password: Admin@123
  regular:
    email: user@test.com
    password: User@123

# Update all tests to use new structure
# Commit message: "Merge test data - kept new structure with original credentials"
```

**Scenario 3: Configuration File Conflict**

```properties
# config.properties conflict

<<<<<<< HEAD
browser=chrome
headless=false
timeout=30
base.url=https://qa.example.com
=======
browser=firefox
headless=true
wait.timeout=60
base.url=https://test.example.com
screenshot.on.failure=true
>>>>>>> feature/firefox-support
```

**Resolution:**

```properties
# Combine both - keep environment from main, add new features from feature
browser=chrome
headless=false
timeout=30
wait.timeout=60
base.url=https://qa.example.com
screenshot.on.failure=true

# Note: Browser can be overridden via command line
# mvn test -Dbrowser=firefox
```

### 9.8 Advanced Conflict Resolution

#### 9.8.1 Cherry-Pick with Conflicts

```bash
# Cherry-pick specific commit
git cherry-pick a1b2c3d

# Output:
error: could not apply a1b2c3d... Add API test
hint: after resolving the conflicts, mark the corrected paths
hint: with 'git add <paths>' or 'git rm <paths>'
hint: and commit the result with 'git commit'

# Resolve conflicts
# ... fix conflicts ...
git add <resolved-files>

# Continue cherry-pick
git cherry-pick --continue
```

#### 9.8.2 Rebase Interactive with Conflicts

```bash
# Interactive rebase
git rebase -i HEAD~5

# Pick commits, squash, etc.
# If conflicts occur during rebase

# Output:
CONFLICT (content): Merge conflict in src/test/LoginTests.java
error: could not apply e4f5g6h... Update login test

# Resolve conflict
vim src/test/LoginTests.java
# ... fix ...
git add src/test/LoginTests.java

# Continue rebase
git rebase --continue

# Repeat until rebase complete
```

#### 9.8.3 Using Theirs/Ours Strategy

```bash
# Accept all changes from feature branch (theirs)
git checkout --theirs src/test/LoginTests.java
git add src/test/LoginTests.java

# Accept all changes from current branch (ours)
git checkout --ours src/test/HomePage.java
git add src/test/HomePage.java

# Useful for:
# - Binary files
# - Generated files
# - When you know one version is correct
```

### 9.9 Common Mistakes to Avoid

**Mistake 1: Deleting Conflict Markers Without Resolving**
```java
// Bad: Just removed markers without thinking
@Test
public void testLogin() {
    // Incomplete code - won't compile!
}

// Good: Properly resolved
@Test
public void testLogin() {
    loginPage.login("admin@test.com", "Admin@123");
    Assert.assertTrue(homePage.isUserLoggedIn());
}
```

**Mistake 2: Not Testing After Resolution**
```bash
# Bad:
git add .
git commit -m "Resolved conflicts"
# Didn't test!

# Good:
# ... resolve conflicts ...
mvn clean test  # Run tests!
git add .
git commit -m "Resolved merge conflicts - all tests passing"
```

**Mistake 3: Accepting Wrong Version**
```bash
# Bad: Blindly accepting theirs or ours
git checkout --theirs .  # Accept all from feature
git add .
git commit
# May have lost important changes!

# Good: Review each conflict individually
git status  # See conflicted files
# Open each file, understand both changes, choose wisely
```

### 9.10 Interview Questions - Merge Conflicts

**Q1: What is a merge conflict and why does it occur?**

**Answer:**
```
Merge Conflict:
Git cannot automatically determine how to merge changes because
the same part of code was modified differently in two branches.

Occurs When:
1. Same lines edited in both branches
2. File deleted in one branch, modified in other
3. File renamed differently in both branches
4. Binary file conflicts

Example:
Branch A: Changed line 10 in LoginPage.java
Branch B: Also changed line 10 in LoginPage.java (differently)

When merging: Git doesn't know which version to keep

Prevention:
- Pull frequently from main
- Small, focused branches
- Good team communication
- Strategic work division

Not a Conflict:
- Changes in different files: Auto-merges
- Changes in different parts of same file: Auto-merges
- Only becomes conflict when same lines modified
```

**Q2: How do you resolve a merge conflict?**

**Answer:**
```
Resolution Steps:

1. Identify Conflicted Files:
   git status
   # Shows "both modified" files

2. Open Conflicted File:
   Look for conflict markers:
   <<<<<<< HEAD
   Your changes
   =======
   Incoming changes
   >>>>>>> branch-name

3. Choose Resolution:
   Option A: Keep your changes (HEAD)
   Option B: Keep incoming changes
   Option C: Combine both (most common)
   Option D: Write new solution

4. Remove Conflict Markers:
   Delete <<<<<<, =======, >>>>>> lines

5. Mark as Resolved:
   git add <resolved-file>

6. Complete Merge:
   git commit

7. Verify:
   Run tests to ensure code works

Example:
# Conflict in LoginTests.java
vim src/test/LoginTests.java

# Remove markers, keep best solution
git add src/test/LoginTests.java
git commit -m "Resolved merge conflict in LoginTests"

# Verify
mvn clean test

Tools:
- VS Code merge editor
- Git mergetool
- IntelliJ merge tool
- Meld, Beyond Compare, etc.
```

**Q3: What's the difference between merge and rebase in terms of conflict resolution?**

**Answer:**
```
Merge:
- Conflicts resolved once
- All conflicts at merge point
- Creates merge commit
- Preserves history

Example:
git merge feature/api-tests
# Resolve all conflicts at once
git commit

Rebase:
- Conflicts resolved per commit
- Multiple resolution points possible
- No merge commit
- Linear history

Example:
git rebase main
# Conflict in commit 1: resolve, git add, git rebase --continue
# Conflict in commit 2: resolve, git add, git rebase --continue
# Conflict in commit 3: resolve, git add, git rebase --continue

Comparison:

Merge:
main:    A---B---C-------M
          \             /
feature:   D---E---F---

One conflict resolution at point M

Rebase:
main:    A---B---C
                  \
feature:           D'---E'---F'

Possible conflicts at D', E', F'
Each commit may need conflict resolution

When to Use:
Merge: Simpler, one-time conflict resolution
Rebase: Clean history, but may need multiple conflict resolutions

Best Practice:
- Use rebase for local feature branches
- Use merge for integrating to main
```

---

## 10. Git Ignore and Best Practices

### 10.1 Understanding .gitignore

`.gitignore` specifies intentionally untracked files that Git should ignore.

**Purpose:**
- Exclude build artifacts
- Ignore IDE-specific files
- Keep secrets out of repository
- Reduce repository size
- Avoid committing local configurations

### 10.1.1 Creating .gitignore

```bash
# Create .gitignore in repository root
touch .gitignore

# Edit with your favorite editor
vim .gitignore
```

**Basic Syntax:**

```gitignore
# Comment

# Ignore specific file
config/credentials.properties

# Ignore all files with extension
*.log
*.class

# Ignore directory
target/
build/
node_modules/

# Ignore files in any directory
**/temp/

# Negative pattern (don't ignore)
!important.log

# Ignore files only in root
/root-only-ignore.txt

# Ignore all txt files in doc/ directory
doc/**/*.txt
```

### 10.2 .gitignore for Test Automation Projects

#### 10.2.1 Java/Maven/TestNG Project

```gitignore
# Compiled class files
*.class
*.jar
*.war
*.ear

# Maven
target/
pom.xml.tag
pom.xml.releaseBackup
pom.xml.versionsBackup
pom.xml.next
release.properties
dependency-reduced-pom.xml

# TestNG
test-output/
testng-results.xml

# Surefire/Failsafe
**/surefire-reports/
**/failsafe-reports/

# Test Reports
reports/
test-reports/
allure-results/
allure-report/
extent-reports/

# Screenshots
screenshots/
screen-captures/
*.png
*.jpg
!test-data/expected/*.png

# Logs
*.log
logs/

# IDE - IntelliJ IDEA
.idea/
*.iml
*.ipr
*.iws
out/

# IDE - Eclipse
.classpath
.project
.settings/
.metadata/

# IDE - VS Code
.vscode/

# OS Files
.DS_Store
Thumbs.db
desktop.ini

# Local configuration
config/local.properties
**/local-*.properties

# Environment files
.env
.env.local

# Temporary files
*.tmp
*.bak
*.swp
*~

# Video recordings
recordings/
videos/
*.mp4
*.avi

# Coverage reports
coverage/
jacoco/
```

#### 10.2.2 JavaScript/Node.js/Cypress Project

```gitignore
# Dependencies
node_modules/
jspm_packages/
package-lock.json
yarn.lock

# Cypress
cypress/videos/
cypress/screenshots/
cypress/results/

# Test Reports
mochawesome-report/
junit-results/

# Logs
npm-debug.log*
yarn-debug.log*
yarn-error.log*
lerna-debug.log*

# IDE
.idea/
.vscode/
*.sublime-workspace

# OS Files
.DS_Store
Thumbs.db

# Environment
.env
.env.local
.env.test

# Build
dist/
build/

# Coverage
coverage/
.nyc_output/
```

#### 10.2.3 Python/Pytest Project

```gitignore
# Byte-compiled / optimized
__pycache__/
*.py[cod]
*$py.class

# Virtual Environment
venv/
env/
ENV/
.venv

# Test Reports
.pytest_cache/
htmlcov/
.coverage
coverage.xml
pytest-report.html

# IDE
.idea/
.vscode/
*.sublime-workspace

# OS
.DS_Store
Thumbs.db

# Logs
*.log

# Environment
.env
config/local.yml
```

### 10.3 Global .gitignore

For files you want to ignore across all projects.

```bash
# Create global gitignore
touch ~/.gitignore_global

# Configure Git to use it
git config --global core.excludesfile ~/.gitignore_global
```

**~/.gitignore_global:**

```gitignore
# OS Files
.DS_Store
.DS_Store?
._*
.Spotlight-V100
.Trashes
ehthumbs.db
Thumbs.db

# IDE
.idea/
.vscode/
*.sublime-workspace
*.swp
*.swo
*~

# Logs
*.log

# Temporary
*.tmp
*.bak
```

### 10.4 Checking Ignored Files

```bash
# Check if file is ignored
git check-ignore -v config/local.properties

# Output:
.gitignore:42:config/local.properties    config/local.properties

# Shows which rule in .gitignore ignores the file

# List all ignored files
git status --ignored

# Output:
On branch main
Ignored files:
  (use "git add -f <file>..." to include in what will be committed)
        .DS_Store
        config/local.properties
        screenshots/
        target/
        test-output/
```

### 10.5 Untracking Already Committed Files

```bash
# If you accidentally committed files that should be ignored

# Step 1: Add to .gitignore
echo "config/credentials.properties" >> .gitignore

# Step 2: Remove from Git (keep in working directory)
git rm --cached config/credentials.properties

# Step 3: Commit
git commit -m "chore: Untrack credentials file"

# Step 4: Push
git push origin main

# For directories
git rm -r --cached test-output/
git commit -m "chore: Untrack test-output directory"
```

**Remove from entire history (if contains secrets):**

```bash
# Use BFG Repo-Cleaner or git-filter-repo

# Install git-filter-repo
pip install git-filter-repo

# Remove file from entire history
git filter-repo --path config/credentials.properties --invert-paths

# Force push (rewrites history)
git push origin --force --all
git push origin --force --tags

# WARNING: This rewrites history!
# Coordinate with team before doing this
```

### 10.6 Template .gitignore for Test Automation

```gitignore
#################################################################
# Test Automation Framework - Git Ignore File
# Project: Selenium-Cucumber Test Suite
# Last Updated: 2026-01-28
#################################################################

#=================================================================
# Build Outputs
#=================================================================
target/
build/
dist/
out/
*.class
*.jar
*.war
*.ear

#=================================================================
# Test Outputs
#=================================================================
# Test Reports
test-output/
test-results/
reports/
allure-results/
allure-report/
extent-reports/
cucumber-reports/
surefire-reports/
junit-reports/

# Screenshots and Videos
screenshots/
screen-captures/
recordings/
videos/
*.png
*.jpg
*.gif
*.mp4
*.avi
# Except expected screenshots for visual testing
!test-data/expected-screenshots/*.png

# Logs
logs/
*.log

# Coverage
coverage/
jacoco/
.nyc_output/

#=================================================================
# Dependencies
#=================================================================
# Maven
.mvn/
dependency-reduced-pom.xml

# NPM/Node
node_modules/
package-lock.json
yarn.lock

# Python
venv/
__pycache__/
*.pyc

#=================================================================
# IDE and Editor Files
#=================================================================
# IntelliJ IDEA
.idea/
*.iml
*.ipr
*.iws

# Eclipse
.classpath
.project
.settings/
.metadata/

# VS Code
.vscode/
*.code-workspace

# Other
*.swp
*.swo
*~

#=================================================================
# Operating System Files
#=================================================================
.DS_Store
.DS_Store?
._*
Thumbs.db
desktop.ini
$RECYCLE.BIN/

#=================================================================
# Configuration and Secrets
#=================================================================
# Local configurations
config/local.properties
config/*-local.yml
*.local
.env
.env.local
.env.test

# Credentials (NEVER commit these!)
**/credentials.properties
**/secrets.yml
**/*password*
**/*secret*
**/*key.json

#=================================================================
# Temporary Files
#=================================================================
*.tmp
*.bak
*.cache
*.temp

#=================================================================
# Browser Drivers (download at runtime)
#=================================================================
drivers/
chromedriver*
geckodriver*
msedgedriver*

#=================================================================
# Additional Exclusions
#=================================================================
# Add project-specific ignores below
```

### 10.7 Best Practices

#### 10.7.1 What to Ignore

**Always Ignore:**
```gitignore
# Build outputs
target/
build/

# Dependencies
node_modules/

# IDE files
.idea/
.vscode/

# OS files
.DS_Store

# Logs
*.log

# Test outputs
test-output/
reports/

# Local configurations
*.local
.env

# Sensitive data
credentials.properties
secrets.yml
```

**Never Ignore:**
```
# Source code
src/

# Test code
test/

# Configuration templates
config/template.properties

# Documentation
README.md
docs/

# Build configuration
pom.xml
package.json

# CI/CD configuration
.github/
.gitlab-ci.yml

# Git configuration
.gitignore
.gitattributes
```

#### 10.7.2 Organize .gitignore

```gitignore
# Use comments and sections for clarity

#=================================================================
# Section 1: Build Outputs
#=================================================================
target/
build/

#=================================================================
# Section 2: Test Reports
#=================================================================
test-output/
reports/

# Makes it easier to maintain
```

#### 10.7.3 Project-Specific .gitignore

```bash
# Different .gitignore for different projects

# Web Automation Project
.gitignore (Selenium, WebDriver)

# API Automation Project
.gitignore (REST Assured, Postman)

# Mobile Automation Project
.gitignore (Appium, iOS/Android specific)
```

#### 10.7.4 Team Agreement

```markdown
Team .gitignore Standards:

1. Never commit:
   - Build artifacts (target/, build/)
   - Test reports (test-output/, reports/)
   - IDE files (.idea/, .vscode/)
   - OS files (.DS_Store, Thumbs.db)
   - Logs (*.log)
   - Local configs (*.local, .env)

2. Always commit:
   - Source code (src/)
   - Tests (test/)
   - Config templates (*.template)
   - Documentation (README.md, docs/)
   - Build files (pom.xml, package.json)

3. Review before commit:
   - Large files (> 1MB)
   - Binary files
   - Data files
```

### 10.8 Common Mistakes to Avoid

**Mistake 1: Committing Secrets**
```bash
# Bad: Committed credentials
git add config/database.properties  # Contains passwords!

# Prevention:
echo "config/database.properties" >> .gitignore
# Use config/database.properties.template instead
```

**Mistake 2: Ignoring Too Much**
```gitignore
# Bad: Too broad
*

# Good: Specific
target/
*.log
```

**Mistake 3: Not Using .gitignore Early**
```bash
# Bad: Create .gitignore after many commits
# Now many unwanted files already in history

# Good: Create .gitignore in first commit
git init
touch .gitignore
# Add patterns
git add .gitignore
git commit -m "Initial commit with gitignore"
```

**Mistake 4: Platform-Specific Ignores in Project .gitignore**
```gitignore
# Bad in project .gitignore:
.DS_Store  # macOS only
Thumbs.db  # Windows only

# Good: Put these in global ~/.gitignore_global
# Keep project .gitignore for project-specific ignores
```

### 10.9 Interview Questions - .gitignore

**Q1: What is .gitignore and why is it important?**

**Answer:**
```
.gitignore:
File that specifies intentionally untracked files
Git should ignore.

Purpose:
1. Exclude build artifacts (target/, build/)
2. Ignore IDE files (.idea/, .vscode/)
3. Keep secrets out of repo (credentials, API keys)
4. Reduce repository size
5. Avoid committing local configurations

Example:
# .gitignore for Java test automation
target/              # Maven build output
*.class             # Compiled files
test-output/        # TestNG reports
.idea/              # IntelliJ IDEA
config/local.*      # Local configurations
*.log               # Log files

Importance in Test Automation:
- Don't commit test reports (generated each run)
- Don't commit screenshots (change frequently)
- Don't commit browser drivers (OS-specific)
- Don't commit local test configurations
- Don't commit sensitive test data (credentials)

Without .gitignore:
- Large repository size
- Merge conflicts in generated files
- Security risks (committed secrets)
- Cluttered repository
```

**Q2: How do you untrack a file that was already committed?**

**Answer:**
```
Scenario: Accidentally committed file that should be ignored

Steps:

1. Add to .gitignore:
   echo "config/credentials.properties" >> .gitignore

2. Remove from Git (keep in working directory):
   git rm --cached config/credentials.properties

3. Commit the removal:
   git commit -m "Untrack credentials file"

4. Push:
   git push origin main

For directories:
git rm -r --cached test-output/
git commit -m "Untrack test-output directory"

Verify:
git status
# config/credentials.properties should not appear

Important:
--cached flag: Removes from Git but keeps in working directory
Without --cached: Deletes file from both Git and disk

If file contains secrets:
Need to remove from entire history (use git-filter-repo)
Then rotate the compromised credentials
```

**Q3: What's the difference between .gitignore and .git/info/exclude?**

**Answer:**
```
.gitignore:
- Committed to repository
- Shared with all team members
- Project-wide ignore rules
- Version controlled
- For project-specific ignores

.git/info/exclude:
- Local to your repository
- Not committed or shared
- Personal ignore rules
- Not version controlled
- For personal preferences

Example:

.gitignore (shared):
target/
*.log
test-output/

.git/info/exclude (personal):
my-local-notes.txt
temp-test-data/
personal-test-config.properties

Use .gitignore for:
- Build artifacts
- Test reports
- IDE files (if team agrees)
- Secrets and credentials

Use .git/info/exclude for:
- Personal scratch files
- Local experiments
- Notes not for sharing

Global .gitignore (~/.gitignore_global):
- OS-specific files (.DS_Store, Thumbs.db)
- Editor files (*.swp, *~)
- Personal IDE preferences
- Applied to all your repositories

Configuration:
git config --global core.excludesfile ~/.gitignore_global
```

---

*[Continuing with sections 11 and 12...]*

## 11. Git Workflow for Test Automation Projects

### 11.1 Gitflow for Test Automation

Gitflow is a branching model designed for project release management.

```
Production:   main ─────────────────────┬──────> [v1.0] ────────┬──> [v1.1]
                                        │                      │
Staging:      develop ──────┬──────────┴─┬──────────┬─────────┴─────
                            │            │          │
Features:            feature/api ────────┘   feature/ui ────────┘
                                          
Hotfix:                               hotfix/critical ──────────┘
```

#### 11.1.1 Branch Structure

**Main Branches:**

```
main (or master)
├── Production-ready code
├── Tagged with version numbers (v1.0.0, v2.0.0)
└── Only receives merges from develop or hotfix

develop
├── Integration branch
├── Latest features
├── Pre-release code
└── Feature branches merge here
```

**Supporting Branches:**

```
feature/*
├── New test suites
├── New features
├── Branch from: develop
└── Merge to: develop

release/*
├── Release preparation
├── Minor bug fixes
├── Branch from: develop
└── Merge to: main and develop

hotfix/*
├── Critical bug fixes
├── Emergency patches
├── Branch from: main
└── Merge to: main and develop
```

### 11.2 Feature Branch Workflow

Most common workflow for test automation teams.

```
main ─────────────────────────────────────────>
     \          \          \          \
      feature1   feature2   feature3   feature4
           \          \          \          \
           PR         PR         PR         PR
             \          \          \          \
              ──────────────────────────────────> main
```

#### 11.2.1 Daily Workflow Example

**Morning - Start New Feature:**

```bash
# 1. Sync with main
cd ~/automation-framework
git checkout main
git pull origin main

# 2. Create feature branch
git checkout -b feature/checkout-tests

# 3. Verify clean start
git status
git log --oneline -3

# 4. Create test structure
mkdir -p src/test/java/tests/checkout
mkdir -p src/test/java/pages/checkout

# 5. Start development
# ... create CheckoutTests.java ...
# ... create CheckoutPage.java ...
```

**During Day - Make Commits:**

```bash
# First commit - structure
git add src/test/java/pages/checkout/CheckoutPage.java
git commit -m "feat: Add CheckoutPage object model

- Created page object for checkout flow
- Added locators for all checkout elements
- Implemented helper methods for interactions"

# Second commit - basic tests
git add src/test/java/tests/checkout/CheckoutTests.java
git commit -m "test: Add basic checkout test suite

- Positive checkout flow test
- Guest checkout test
- Logged-in user checkout test"

# Third commit - data-driven
git add src/test/resources/testdata/checkout-data.csv
git commit -m "test: Add data-driven checkout tests

- Multiple test scenarios using CSV data
- Boundary value tests
- Negative test cases"

# Fourth commit - improvements
git add src/test/java/tests/checkout/CheckoutTests.java
git commit -m "refactor: Improve checkout test assertions

- Added comprehensive verifications
- Improved error messages
- Added test documentation"
```

**End of Day - Push Work:**

```bash
# Review your work
git log --oneline

# Output:
a1b2c3d (HEAD -> feature/checkout-tests) refactor: Improve checkout test assertions
e4f5g6h test: Add data-driven checkout tests
i7j8k9l test: Add basic checkout test suite
m0n1o2p feat: Add CheckoutPage object model

# Push to remote
git push -u origin feature/checkout-tests

# Output:
Total 0 (delta 0), reused 0 (delta 0), pack-reused 0
remote: 
remote: Create a pull request for 'feature/checkout-tests' on GitHub by visiting:
remote:      https://github.com/company/automation-framework/pull/new/feature/checkout-tests
remote:
To github.com:company/automation-framework.git
 * [new branch]      feature/checkout-tests -> feature/checkout-tests
Branch 'feature/checkout-tests' set up to track remote branch 'feature/checkout-tests' from 'origin'.
```

**Next Day - Continue Work:**

```bash
# Morning sync
git checkout feature/checkout-tests
git fetch origin
git merge origin/main  # Get latest changes from main

# Continue development
# ... add more tests ...

git add .
git commit -m "test: Add payment validation tests"
git push
```

**Ready for Review - Create PR:**

```bash
# Final sync before PR
git fetch origin
git rebase origin/main  # Clean up history

# Force push rebased branch
git push --force-with-lease origin feature/checkout-tests

# Create Pull Request
gh pr create --title "feat: Add comprehensive checkout test suite" \
             --body "Implements end-to-end checkout tests with multiple scenarios"

# Or create PR via GitHub web interface
```

**After PR Approved - Cleanup:**

```bash
# PR merged to main

# Switch to main
git checkout main
git pull origin main

# Delete feature branch locally
git branch -d feature/checkout-tests

# Delete remote branch (if not auto-deleted)
git push origin --delete feature/checkout-tests

# Clean up remote-tracking branches
git fetch --prune
```

### 11.3 Release Branch Workflow

For managing releases in test automation.

```bash
# Scenario: Preparing v1.2.0 release

# 1. Create release branch from develop
git checkout develop
git pull origin develop
git checkout -b release/v1.2.0

# 2. Version bump and final prep
# Update version in pom.xml, package.json, etc.
vim pom.xml  # Change version to 1.2.0

# Update CHANGELOG
vim CHANGELOG.md

# 3. Commit release prep
git add .
git commit -m "chore: Prepare release v1.2.0

- Bump version to 1.2.0
- Update CHANGELOG
- Final test configuration for release"

# 4. Run full test suite
mvn clean test -Psuite=regression

# 5. Fix any release-specific bugs
# ... bug fixes ...
git commit -m "fix: Correct environment config for release"

# 6. Merge to main (production)
git checkout main
git pull origin main
git merge --no-ff release/v1.2.0 -m "Merge release v1.2.0"

# 7. Tag the release
git tag -a v1.2.0 -m "Release version 1.2.0

Features:
- Comprehensive checkout test suite
- API authentication tests
- Mobile test framework

Bug Fixes:
- Fixed flaky login tests
- Corrected payment validation

Test Coverage: 85%"

# 8. Push main and tags
git push origin main
git push origin v1.2.0

# 9. Merge back to develop
git checkout develop
git merge --no-ff release/v1.2.0 -m "Merge release v1.2.0 back to develop"
git push origin develop

# 10. Delete release branch
git branch -d release/v1.2.0
git push origin --delete release/v1.2.0
```

### 11.4 Hotfix Workflow

For critical production bugs.

```bash
# Scenario: Critical bug in production login tests

# 1. Create hotfix from main
git checkout main
git pull origin main
git checkout -b hotfix/login-selector-fix

# 2. Fix the issue
vim src/test/java/pages/LoginPage.java
# Fix incorrect selector

# 3. Commit fix
git add src/test/java/pages/LoginPage.java
git commit -m "hotfix: Correct login button selector

Login button ID changed in production.
Updated selector from #loginBtn to #login-button.

Fixes: #456
Urgency: High - Login tests failing in production"

# 4. Run tests to verify
mvn clean test -Dtest=LoginTests

# 5. Merge to main
git checkout main
git merge --no-ff hotfix/login-selector-fix
git tag -a v1.2.1 -m "Hotfix v1.2.1 - Login selector fix"
git push origin main
git push origin v1.2.1

# 6. Merge to develop
git checkout develop
git merge --no-ff hotfix/login-selector-fix
git push origin develop

# 7. Delete hotfix branch
git branch -d hotfix/login-selector-fix
git push origin --delete hotfix/login-selector-fix
```

### 11.5 Collaborative Workflow

**Scenario: Multiple QA Engineers Working Together**

**Engineer 1 - API Tests:**
```bash
git checkout -b feature/api-tests
# Work on API tests
git add src/test/java/api/
git commit -m "test: Add user API tests"
git push -u origin feature/api-tests
# Create PR
```

**Engineer 2 - UI Tests:**
```bash
git checkout -b feature/ui-tests
# Work on UI tests
git add src/test/java/ui/
git commit -m "test: Add checkout UI tests"
git push -u origin feature/ui-tests
# Create PR
```

**Engineer 3 - Framework Improvements:**
```bash
git checkout -b feature/reporting-enhancement
# Improve reporting
git add src/main/java/framework/
git commit -m "feat: Add ExtentReports integration"
git push -u origin feature/reporting-enhancement
# Create PR
```

**Team Lead - Review and Merge:**
```bash
# Review PRs
gh pr list

# Output:
#45  feature/api-tests              Engineer1:feature/api-tests
#46  feature/ui-tests               Engineer2:feature/ui-tests
#47  feature/reporting-enhancement  Engineer3:feature/reporting-enhancement

# Review each
gh pr checkout 45
mvn clean test
gh pr review 45 --approve

# Merge
gh pr merge 45 --squash

# Repeat for others
```

### 11.6 CI/CD Integration Workflow

**GitHub Actions Example:**

```yaml
# .github/workflows/ci.yml
name: Test Automation CI

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main, develop ]

jobs:
  test:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Set up JDK 11
      uses: actions/setup-java@v3
      with:
        java-version: '11'
    
    - name: Cache Maven packages
      uses: actions/cache@v3
      with:
        path: ~/.m2
        key: ${{ runner.os }}-m2-${{ hashFiles('**/pom.xml') }}
    
    - name: Run smoke tests
      if: github.event_name == 'pull_request'
      run: mvn clean test -Psuite=smoke
    
    - name: Run regression tests
      if: github.event_name == 'push' && github.ref == 'refs/heads/main'
      run: mvn clean test -Psuite=regression
    
    - name: Upload test reports
      if: always()
      uses: actions/upload-artifact@v3
      with:
        name: test-reports
        path: target/surefire-reports/
    
    - name: Generate test report
      if: always()
      uses: dorny/test-reporter@v1
      with:
        name: Test Results
        path: target/surefire-reports/*.xml
        reporter: java-junit
```

**Workflow Integration:**

```bash
# Developer workflow with CI
git checkout -b feature/new-tests

# Make changes
git add .
git commit -m "test: Add new test cases"

# Push triggers CI
git push -u origin feature/new-tests

# CI runs:
# 1. Checkout code
# 2. Set up environment
# 3. Run smoke tests
# 4. Generate reports
# 5. Comment on PR with results

# Create PR
gh pr create

# CI automatically:
# - Runs tests
# - Reports results
# - Blocks merge if tests fail
# - Updates PR with test report link

# After approval and merge
# CI on main branch:
# - Runs full regression suite
# - Generates comprehensive reports
# - Archives artifacts
# - Notifies team
```

### 11.7 Test Automation Specific Workflows

#### 11.7.1 Regression Suite Workflow

```bash
# Maintain regression suite in dedicated branch

# Main regression branch
git checkout -b test/regression-suite

# Add stable tests only
git merge feature/stable-login-tests
git merge feature/stable-checkout-tests

# Schedule regression runs
# CI runs regression-suite branch nightly

# Update regression suite
git checkout test/regression-suite
git merge develop  # Get latest stable tests
git push origin test/regression-suite

# CI triggers nightly regression
```

#### 11.7.2 Environment-Specific Branches

```bash
# Different branches for different environments

# QA environment tests
git checkout -b env/qa
# Configure for QA
vim config/qa.properties

# Staging environment tests
git checkout -b env/staging
# Configure for staging
vim config/staging.properties

# Production smoke tests
git checkout -b env/production-smoke
# Configure for production
vim config/production.properties

# CI/CD runs appropriate branch for each environment
# QA deploy → runs env/qa
# Staging deploy → runs env/staging
# Production deploy → runs env/production-smoke
```

#### 11.7.3 Test Data Management Workflow

```bash
# Separate branch for test data updates

git checkout -b data/test-users-update

# Update test data
vim src/test/resources/testdata/users.csv
vim src/test/resources/testdata/products.json

git add src/test/resources/testdata/
git commit -m "data: Update test user credentials for Q1 2026"

# Create PR for review
gh pr create --title "Update Q1 2026 test data"

# After approval
git checkout main
git merge data/test-users-update
git push origin main
git branch -d data/test-users-update
```

### 11.8 Best Practices for Test Automation Workflows

#### 11.8.1 Commit Message Standards

```bash
# Follow conventional commits

# Format:
<type>(<scope>): <subject>

<body>

<footer>

# Types for test automation:
test:     # Adding or updating tests
feat:     # New test framework feature
fix:      # Bug fix in tests or framework
refactor: # Code restructuring
perf:     # Performance improvement
docs:     # Documentation
chore:    # Maintenance tasks
ci:       # CI/CD changes
data:     # Test data updates

# Examples:
git commit -m "test: Add checkout flow end-to-end tests"

git commit -m "feat: Add ExtentReports integration

Implemented ExtentReports for better test reporting:
- Configured ExtentReports in TestBase
- Added screenshots on failure
- Created custom report template
- Integrated with CI/CD pipeline

Test report now includes:
- Visual timeline
- Screenshots
- Detailed logs
- Environment info"

git commit -m "fix: Correct flaky login test

Login test was failing intermittently due to race condition.
Added explicit wait for welcome message element.

Fixes: #234"

git commit -m "refactor: Extract common waits to BasePage

Moved all WebDriverWait logic to BasePage:
- waitForElementVisible()
- waitForElementClickable()
- waitForTextPresent()

Reduces code duplication across page objects."
```

#### 11.8.2 Branch Naming Conventions

```bash
# Consistent naming scheme

# Features
feature/api-authentication-tests
feature/mobile-login-suite
feature/reporting-enhancement

# Bug fixes
fix/login-button-locator
fix/flaky-checkout-test
fix/test-data-encoding

# Tests
test/smoke-suite
test/regression-suite
test/api-contract-tests

# Hotfixes
hotfix/critical-login-failure
hotfix/production-data-issue

# Chores
chore/update-dependencies
chore/refactor-page-objects

# Data
data/q1-test-users
data/product-catalog-update

# Environment
env/qa-configuration
env/staging-setup

# Releases
release/v1.2.0
release/v2.0.0-beta
```

#### 11.8.3 Code Review Checklist

```markdown
Test Automation Code Review Checklist:

## Test Quality
- [ ] Tests are independent and can run in any order
- [ ] No hard-coded waits (Thread.sleep)
- [ ] Proper assertions (not just element presence)
- [ ] Test data externalized
- [ ] Tests clean up after themselves
- [ ] No flaky tests

## Code Quality
- [ ] Follows Page Object Model
- [ ] Reusable methods extracted
- [ ] Meaningful variable/method names
- [ ] No code duplication
- [ ] Proper exception handling
- [ ] Comments for complex logic

## Framework Standards
- [ ] Uses framework utilities (waits, actions)
- [ ] Follows naming conventions
- [ ] Proper use of annotations (@Test, @BeforeMethod)
- [ ] Logging implemented
- [ ] Screenshots on failure

## Configuration
- [ ] No hardcoded URLs or credentials
- [ ] Uses configuration files
- [ ] Environment-specific configs externalized

## Documentation
- [ ] Clear test method names
- [ ] Test description/comments
- [ ] README updated if needed
- [ ] Test data documented

## Performance
- [ ] Tests run in reasonable time
- [ ] No unnecessary waits
- [ ] Proper use of parallel execution

## Security
- [ ] No credentials in code
- [ ] Sensitive data in .gitignore
- [ ] No API keys committed
```

### 11.9 Troubleshooting Common Workflow Issues

**Issue 1: Forgot to create branch**
```bash
# Made changes on main by mistake
git status
# On branch main (oops!)

# Solution: Create branch and move changes
git checkout -b feature/my-changes
# Changes move to new branch

git status
# Now on feature/my-changes with your changes
```

**Issue 2: Committed to wrong branch**
```bash
# Committed to develop instead of feature branch

# Solution: Cherry-pick to correct branch
git log --oneline -1
# a1b2c3d My commit

git checkout -b feature/correct-branch
git cherry-pick a1b2c3d

# Remove from wrong branch
git checkout develop
git reset --hard HEAD~1
```

**Issue 3: Need to update feature branch with main**
```bash
# Feature branch outdated

# Option 1: Merge (preserves history)
git checkout feature/my-branch
git merge main

# Option 2: Rebase (cleaner history)
git checkout feature/my-branch
git rebase main
```

**Issue 4: Accidentally pushed to wrong remote**
```bash
# Pushed to wrong fork

# Solution: Force push to correct remote
git remote -v
# origin  yourfork
# upstream  company

git push -f upstream main  # Wrong!

# Fix:
git push origin main  # Correct
git push -f upstream :main  # Delete from wrong remote (if you have permission)
```

### 11.10 Interview Questions - Workflows

**Q1: Explain a typical Git workflow for test automation projects.**

**Answer:**
```
Feature Branch Workflow (Most Common):

1. Start:
   git checkout main
   git pull origin main
   git checkout -b feature/checkout-tests

2. Develop:
   # Write tests
   git add src/test/
   git commit -m "test: Add checkout tests"
   # Continue development
   git commit -m "refactor: Improve assertions"

3. Sync:
   git fetch origin
   git rebase origin/main

4. Push:
   git push -u origin feature/checkout-tests

5. Pull Request:
   # Create PR on GitHub
   # Code review
   # CI/CD runs tests

6. Address Feedback:
   # Make changes
   git commit -m "Address PR review comments"
   git push

7. Merge:
   # After approval
   # Squash and merge to main

8. Cleanup:
   git checkout main
   git pull origin main
   git branch -d feature/checkout-tests

Benefits:
- Isolated development
- Code review before merge
- CI/CD validation
- Clean main branch
- Easy rollback

Team Workflow:
- Multiple engineers create feature branches
- Each works independently
- PRs reviewed by team
- Merged to main after approval
- Main always stable
```

**Q2: How do you handle hotfixes in a test automation project?**

**Answer:**
```
Hotfix Workflow for Critical Issues:

Scenario: Production tests failing due to selector change

1. Create Hotfix:
   git checkout main
   git pull origin main
   git checkout -b hotfix/login-selector-fix

2. Fix Issue:
   # Update selector in LoginPage
   git add src/test/pages/LoginPage.java
   git commit -m "hotfix: Update login button selector"

3. Verify:
   mvn clean test -Dtest=LoginTests
   # Ensure tests pass

4. Merge to Main:
   git checkout main
   git merge --no-ff hotfix/login-selector-fix
   git tag -a v1.2.1 -m "Hotfix v1.2.1"
   git push origin main --tags

5. Merge to Develop:
   git checkout develop
   git merge --no-ff hotfix/login-selector-fix
   git push origin develop

6. Cleanup:
   git branch -d hotfix/login-selector-fix

Key Points:
- Branch from main (production)
- Quick fix only
- Test thoroughly
- Merge to both main and develop
- Tag new version
- Document in CHANGELOG

Why not feature branch:
- Hotfixes are urgent
- Can't wait for normal PR process
- Need to deploy immediately
- Bypass normal review for speed
```

**Q3: What are the best practices for commit messages in test automation?**

**Answer:**
```
Commit Message Best Practices:

Format:
<type>(<scope>): <subject>

<body>

<footer>

Types:
test:     # Test-related changes
feat:     # New features
fix:      # Bug fixes
refactor: # Code improvements
docs:     # Documentation
chore:    # Maintenance

Examples:

Good:
git commit -m "test: Add API authentication tests

Implemented REST Assured tests for:
- Login endpoint
- Token generation
- Token validation

Coverage increased from 60% to 75%

Closes: #234"

Bad:
git commit -m "updates"
git commit -m "fix"
git commit -m "changes"

Guidelines:
1. Use imperative mood ("Add" not "Added")
2. First line < 50 chars
3. Explain WHY not just WHAT
4. Reference issue numbers
5. Be specific and clear

Test-Specific Tips:
test: Add login tests
test: Update checkout flow tests
feat: Add Page Object Model for checkout
fix: Correct flaky API test
refactor: Extract common waits to BasePage
data: Update Q1 test user credentials

Benefits:
- Clear history
- Easy to find changes
- Better collaboration
- Automated changelog generation
- Git blame more useful
```

---

## 12. Advanced Git Commands

### 12.1 Git Rebase

Rebase reapplies commits on top of another base commit, creating a linear history.

```
Before Rebase:
main:    A---B---C---D
          \
feature:   E---F---G

After Rebase:
main:    A---B---C---D
                      \
feature:               E'---F'---G'
```

#### 12.1.1 Basic Rebase

```bash
# Update feature branch with latest main
git checkout feature/api-tests
git rebase main

# Output:
First, rewinding head to replay your work on top of it...
Applying: Add API test framework
Applying: Implement authentication tests
Applying: Add error handling tests
```

**Rebase Process:**
1. Git saves your commits (E, F, G)
2. Resets branch to main (D)
3. Replays your commits one by one (E', F', G')
4. Each commit gets new hash (history rewritten)

#### 12.1.2 Interactive Rebase

Interactive rebase allows you to modify commits before reapplying them.

```bash
# Rebase last 4 commits interactively
git rebase -i HEAD~4

# Opens editor:
pick a1b2c3d test: Add basic API tests
pick e4f5g6h test: Add authentication tests
pick i7j8k9l fix: Correct test assertions
pick m0n1o2p test: Add error handling

# Rebase a1b2c3d..m0n1o2p onto a1b2c3d (4 commands)
#
# Commands:
# p, pick <commit> = use commit
# r, reword <commit> = use commit, but edit the commit message
# e, edit <commit> = use commit, but stop for amending
# s, squash <commit> = use commit, but meld into previous commit
# f, fixup <commit> = like "squash", but discard this commit's log message
# x, exec <command> = run command (the rest of the line) using shell
# b, break = stop here (continue rebase later with 'git rebase --continue')
# d, drop <commit> = remove commit
```

**Common Operations:**

**1. Squash Commits:**
```bash
pick a1b2c3d test: Add basic API tests
squash e4f5g6h test: Add authentication tests
squash i7j8k9l fix: Correct test assertions
squash m0n1o2p test: Add error handling

# Result: All 4 commits squashed into 1
# New commit message editor opens
```

**2. Reword Commit Message:**
```bash
pick a1b2c3d test: Add basic API tests
reword e4f5g6h test: Add authentication tests
pick i7j8k9l fix: Correct test assertions
pick m0n1o2p test: Add error handling

# Editor opens for commit e4f5g6h to change message
```

**3. Reorder Commits:**
```bash
pick m0n1o2p test: Add error handling
pick a1b2c3d test: Add basic API tests
pick e4f5g6h test: Add authentication tests
pick i7j8k9l fix: Correct test assertions

# Commits will be applied in this new order
```

**4. Drop Commits:**
```bash
pick a1b2c3d test: Add basic API tests
drop e4f5g6h test: Add authentication tests
pick i7j8k9l fix: Correct test assertions
pick m0n1o2p test: Add error handling

# Commit e4f5g6h will be removed from history
```

**5. Edit Commits:**
```bash
pick a1b2c3d test: Add basic API tests
edit e4f5g6h test: Add authentication tests
pick i7j8k9l fix: Correct test assertions

# Rebase stops at e4f5g6h for you to make changes
# Make changes
git add .
git commit --amend
git rebase --continue
```

#### 12.1.3 Practical Rebase Example

**Scenario: Clean up before creating PR**

```bash
# Your feature branch has messy commits
git log --oneline

# Output:
a1b2c3d WIP: trying different approach
e4f5g6h Add tests
i7j8k9l Fix typo
m0n1o2p Add more tests
q3r4s5t Fix another typo
u5v6w7x Final tests

# Interactive rebase to clean up
git rebase -i HEAD~6

# In editor:
pick a1b2c3d WIP: trying different approach
squash e4f5g6h Add tests
squash i7j8k9l Fix typo
pick m0n1o2p Add more tests
squash q3r4s5t Fix another typo
squash u5v6w7x Final tests

# Save and close editor
# New commit message editor opens

# Write clean commit message:
test: Add comprehensive API test suite

Implemented REST Assured tests covering:
- Basic CRUD operations
- Authentication flows
- Error handling
- Input validation

Test coverage increased from 60% to 85%

Closes: #234

# Result: 6 messy commits → 2 clean commits
git log --oneline
# Output:
a1b2c3d test: Add comprehensive API test suite
m0n1o2p test: Add additional API test scenarios
```

#### 12.1.4 Rebase Conflicts

```bash
# Start rebase
git rebase main

# Output:
CONFLICT (content): Merge conflict in src/test/LoginTests.java
error: could not apply e4f5g6h... Add authentication tests
Resolve all conflicts manually, mark them as resolved with
"git add/rm <conflicted_files>", then run "git rebase --continue".
You can instead skip this commit: run "git rebase --skip".
To abort and get back to the state before "git rebase", run "git rebase --abort".
Could not apply e4f5g6h... Add authentication tests

# Check status
git status

# Output:
interactive rebase in progress; onto a1b2c3d
Last command done (1 command done):
   pick e4f5g6h Add authentication tests
Next commands to do (2 remaining commands):
   pick i7j8k9l Fix test assertions
   pick m0n1o2p Add error handling
You are currently rebasing branch 'feature/api-tests' on 'a1b2c3d'.

Unmerged paths:
  (use "git restore --staged <file>..." to unstage)
  (use "git add <file>..." to mark resolution)
        both modified:   src/test/LoginTests.java

# Resolve conflict
vim src/test/LoginTests.java
# ... fix conflicts ...

# Mark as resolved
git add src/test/LoginTests.java

# Continue rebase
git rebase --continue

# If more conflicts, repeat
# ... resolve ...
# git add .
# git rebase --continue

# If too complex, abort
git rebase --abort
```

### 12.2 Git Cherry-Pick

Cherry-pick applies specific commits from one branch to another.

```
main:    A---B---C---D
          \
feature:   E---F---G---H

# Cherry-pick commit G to main
main:    A---B---C---D---G'
          \
feature:   E---F---G---H
```

#### 12.2.1 Basic Cherry-Pick

```bash
# Pick specific commit
git checkout main
git cherry-pick e4f5g6h

# Output:
[main a1b2c3d] test: Add authentication tests
 Date: Mon Jan 28 14:30:00 2026 -0800
 2 files changed, 87 insertions(+), 12 deletions(-)
```

**Use Cases:**

**1. Apply Hotfix to Multiple Branches:**
```bash
# Fixed bug in hotfix branch
git checkout hotfix/critical-fix
git log --oneline -1
# a1b2c3d fix: Correct login selector

# Apply to develop
git checkout develop
git cherry-pick a1b2c3d

# Apply to release branch
git checkout release/v1.2
git cherry-pick a1b2c3d
```

**2. Move Commit to Different Branch:**
```bash
# Committed to wrong branch
git checkout feature/wrong-branch
git log --oneline -1
# a1b2c3d test: Add checkout tests

# Cherry-pick to correct branch
git checkout feature/correct-branch
git cherry-pick a1b2c3d

# Remove from wrong branch
git checkout feature/wrong-branch
git reset --hard HEAD~1
```

**3. Apply Multiple Commits:**
```bash
# Cherry-pick range of commits
git cherry-pick e4f5g6h^..m0n1o2p

# Cherry-pick multiple specific commits
git cherry-pick a1b2c3d e4f5g6h i7j8k9l
```

#### 12.2.2 Cherry-Pick Conflicts

```bash
# Cherry-pick with conflict
git cherry-pick a1b2c3d

# Output:
error: could not apply a1b2c3d... Add authentication tests
hint: after resolving the conflicts, mark the corrected paths
hint: with 'git add <paths>' or 'git rm <paths>'
hint: and commit the result with 'git cherry-pick --continue'.

# Resolve conflict
vim src/test/AuthTests.java
# ... fix ...

# Mark as resolved
git add src/test/AuthTests.java

# Continue cherry-pick
git cherry-pick --continue

# Or abort
git cherry-pick --abort
```

### 12.3 Git Stash

Stash temporarily shelves changes so you can work on something else.

```
Working Directory:  Modified files
         ↓
      git stash
         ↓
Stash:  Saved changes
         ↓
Working Directory:  Clean

Later:
      git stash pop
         ↓
Working Directory:  Modified files restored
```

#### 12.3.1 Basic Stash Operations

```bash
# Save current changes
git stash

# Output:
Saved working directory and index state WIP on main: a1b2c3d Latest commit

# Stash with message
git stash save "WIP: Implementing checkout tests"

# Output:
Saved working directory and index state On main: WIP: Implementing checkout tests

# List stashes
git stash list

# Output:
stash@{0}: On main: WIP: Implementing checkout tests
stash@{1}: WIP on feature/api-tests: e4f5g6h Add API tests
stash@{2}: WIP on main: a1b2c3d Latest commit

# Apply most recent stash (keeps in stash list)
git stash apply

# Apply and remove from list
git stash pop

# Apply specific stash
git stash apply stash@{1}

# Drop specific stash
git stash drop stash@{0}

# Clear all stashes
git stash clear
```

#### 12.3.2 Practical Stash Scenarios

**Scenario 1: Switch Context Quickly**
```bash
# Working on feature
git checkout feature/checkout-tests
# ... writing tests ...

# Urgent: Need to fix bug on main
git status
# Modified: src/test/CheckoutTests.java

# Stash current work
git stash save "WIP: Checkout tests - halfway through payment validation"

# Switch to main
git checkout main
git pull origin main
git checkout -b hotfix/urgent-fix

# Fix bug
# ... fix ...
git add .
git commit -m "hotfix: Critical bug fix"
git push origin hotfix/urgent-fix

# Return to feature work
git checkout feature/checkout-tests
git stash pop

# Continue where you left off
```

**Scenario 2: Clean Working Directory for Pull**
```bash
# Modified files
git status
# Modified: src/test/LoginTests.java

# Want to pull latest changes
git stash

# Pull
git pull origin main

# Restore changes
git stash pop

# If conflicts, resolve and drop stash
git add .
git stash drop
```

**Scenario 3: Try Experimental Change**
```bash
# Stash current work
git stash save "Current implementation"

# Try different approach
# ... experiment ...

# Don't like it, restore original
git reset --hard
git stash pop

# Or keep experiment
git add .
git commit -m "Better implementation"
git stash drop
```

#### 12.3.3 Advanced Stash Operations

```bash
# Stash including untracked files
git stash -u

# Stash including untracked and ignored files
git stash -a

# Create branch from stash
git stash branch feature/stashed-work stash@{0}

# Show stash contents
git stash show -p stash@{0}

# Partial stash (interactive)
git stash -p

# Output:
diff --git a/src/test/LoginTests.java b/src/test/LoginTests.java
...
Stash this hunk [y,n,q,a,d,e,?]?
```

### 12.4 Git Tags

Tags mark specific points in history, typically for releases.

#### 12.4.1 Creating Tags

**Lightweight Tags:**
```bash
# Simple pointer to commit
git tag v1.0.0

# Tag specific commit
git tag v1.0.0 a1b2c3d
```

**Annotated Tags (Recommended):**
```bash
# Annotated tag with message
git tag -a v1.0.0 -m "Release version 1.0.0"

# Detailed annotated tag
git tag -a v1.0.0 -m "Release v1.0.0 - Test Automation Framework

Features:
- Complete login test suite
- Checkout flow tests
- API authentication tests
- Mobile test framework

Test Coverage: 85%
Platforms: Web, API, Mobile

Release Date: 2026-01-28
Tested Browsers: Chrome, Firefox, Safari, Edge
"
```

#### 12.4.2 Listing and Viewing Tags

```bash
# List all tags
git tag

# Output:
v0.9.0
v0.9.1
v1.0.0
v1.0.1
v1.1.0

# List tags matching pattern
git tag -l "v1.0.*"

# Output:
v1.0.0
v1.0.1

# Show tag details
git show v1.0.0

# Output:
tag v1.0.0
Tagger: John Doe <john@example.com>
Date:   Mon Jan 28 14:30:00 2026 -0800

Release v1.0.0 - Test Automation Framework

Features:
- Complete login test suite
...

commit a1b2c3d4e5f6...
Author: John Doe <john@example.com>
Date:   Mon Jan 28 14:30:00 2026 -0800

    Final commit for v1.0.0
```

#### 12.4.3 Pushing Tags

```bash
# Push specific tag
git push origin v1.0.0

# Push all tags
git push origin --tags

# Push tags and commits together
git push origin main --tags
```

#### 12.4.4 Deleting Tags

```bash
# Delete local tag
git tag -d v1.0.0

# Delete remote tag
git push origin --delete v1.0.0

# Or
git push origin :refs/tags/v1.0.0
```

#### 12.4.5 Checking Out Tags

```bash
# Checkout specific tag (detached HEAD)
git checkout v1.0.0

# Output:
Note: switching to 'v1.0.0'.

You are in 'detached HEAD' state...

HEAD is now at a1b2c3d Final commit for v1.0.0

# Create branch from tag
git checkout -b hotfix/v1.0.1 v1.0.0
```

#### 12.4.6 Tagging for Test Automation

**Release Tagging:**
```bash
# Major release
git tag -a v2.0.0 -m "Major release v2.0.0

Breaking Changes:
- Updated to Selenium 4
- New Page Object structure

New Features:
- Shadow DOM support
- BiDi API integration
- W3C WebDriver compliance

Test Coverage: 90%"

git push origin v2.0.0

# Minor release
git tag -a v2.1.0 -m "Minor release v2.1.0

New Features:
- API test suite
- Mobile login tests

Bug Fixes:
- Fixed flaky checkout tests
- Improved waits in BasePage

Test Coverage: 92%"

# Patch release
git tag -a v2.1.1 -m "Patch release v2.1.1

Bug Fixes:
- Critical login selector fix
- Environment config correction

Test Coverage: 92%"
```

**Test Suite Tagging:**
```bash
# Tag stable test suites
git tag -a smoke-suite-v1 -m "Stable smoke test suite

Tests: 25
Execution Time: 5 minutes
Browsers: Chrome, Firefox
Pass Rate: 100%"

git tag -a regression-suite-v3 -m "Regression test suite v3

Tests: 250
Execution Time: 45 minutes
Coverage: 85%
Last Updated: 2026-01-28"
```

### 12.5 Git Reset

Reset moves branch pointer to different commit, modifying history.

```
Before Reset:
main:    A---B---C---D---E
                         ^
                        HEAD

After Reset --soft HEAD~2:
main:    A---B---C
             ^
            HEAD
(Changes from D and E staged)

After Reset --mixed HEAD~2:
main:    A---B---C
             ^
            HEAD
(Changes from D and E in working directory)

After Reset --hard HEAD~2:
main:    A---B---C
             ^
            HEAD
(Changes from D and E discarded)
```

#### 12.5.1 Reset Modes

**Soft Reset:**
```bash
# Reset to previous commit, keep changes staged
git reset --soft HEAD~1

# Use case: Change last commit
git reset --soft HEAD~1
# Edit files
git add .
git commit -m "Better commit message"
```

**Mixed Reset (Default):**
```bash
# Reset to previous commit, keep changes unstaged
git reset HEAD~1
# OR
git reset --mixed HEAD~1

# Use case: Unstage files
git reset HEAD file.txt
```

**Hard Reset:**
```bash
# Reset to previous commit, discard all changes
git reset --hard HEAD~1

# Use case: Discard all local changes
git reset --hard origin/main

# WARNING: This destroys uncommitted work!
```

#### 12.5.2 Practical Reset Examples

**Example 1: Undo Last Commit (Keep Changes)**
```bash
# Committed too early
git commit -m "test: Add tests"

# Realize you forgot some files
git reset --soft HEAD~1

# Add forgotten files
git add forgotten-file.java
git commit -m "test: Add comprehensive test suite"
```

**Example 2: Completely Discard Commit**
```bash
# Committed wrong changes
git log --oneline -2
# a1b2c3d Wrong commit
# e4f5g6h Previous commit

# Discard wrong commit
git reset --hard HEAD~1

# Verify
git log --oneline -1
# e4f5g6h Previous commit
```

**Example 3: Reset to Specific Commit**
```bash
# Reset to specific commit
git log --oneline -5
# a1b2c3d Latest
# e4f5g6h Commit 2
# i7j8k9l Commit 3
# m0n1o2p Good commit
# q3r4s5t Old commit

# Reset to "Good commit"
git reset --hard m0n1o2p

# Now HEAD is at m0n1o2p
git log --oneline -1
# m0n1o2p Good commit
```

**Example 4: Reset Pushed Commits (Dangerous!)**
```bash
# Reset commits that were pushed
git reset --hard HEAD~2
git push origin main --force

# WARNING: Rewrites public history!
# Only do this on personal branches
# Never on shared branches (main, develop)
```

### 12.6 Git Reflog

Reflog (reference log) records when branch tips are updated.

```bash
# View reflog
git reflog

# Output:
a1b2c3d (HEAD -> main) HEAD@{0}: commit: Add authentication tests
e4f5g6h HEAD@{1}: commit: Add basic tests
i7j8k9l HEAD@{2}: checkout: moving from feature/api to main
m0n1o2p HEAD@{3}: commit: Implement API helper
q3r4s5t HEAD@{4}: checkout: moving from main to feature/api
u5v6w7x HEAD@{5}: pull origin main: Fast-forward
y7z8a9b HEAD@{6}: commit: Add login tests
```

#### 12.6.1 Recovering Lost Commits

**Scenario: Accidentally reset too far**

```bash
# Your commits
git log --oneline -3
# a1b2c3d Add authentication tests
# e4f5g6h Add basic tests
# i7j8k9l Add API framework

# Accidentally reset too far
git reset --hard HEAD~3

# Commits gone!
git log --oneline -1
# m0n1o2p Old commit

# Use reflog to find lost commits
git reflog

# Output:
m0n1o2p (HEAD -> main) HEAD@{0}: reset: moving to HEAD~3
a1b2c3d HEAD@{1}: commit: Add authentication tests
e4f5g6h HEAD@{2}: commit: Add basic tests
i7j8k9l HEAD@{3}: commit: Add API framework

# Recover by resetting to lost commit
git reset --hard a1b2c3d

# Verify
git log --oneline -3
# a1b2c3d Add authentication tests
# e4f5g6h Add basic tests
# i7j8k9l Add API framework

# Commits recovered!
```

**Scenario: Recover deleted branch**

```bash
# Deleted branch accidentally
git branch -D feature/important-work

# Output:
Deleted branch feature/important-work (was a1b2c3d).

# Find branch in reflog
git reflog

# Output:
e4f5g6h HEAD@{0}: checkout: moving from feature/important-work to main
a1b2c3d HEAD@{1}: commit: Important work
...

# Recreate branch
git checkout -b feature/important-work a1b2c3d

# Branch recovered!
```

### 12.7 Git Bisect

Bisect uses binary search to find which commit introduced a bug.

```
Good:   A---B---C---D---E---F---G---H---I---J  Bad
                                               ^
                                              Bug

Bisect Process:
1. Test E (middle): Bad - bug present
2. Test C (middle of A-E): Good
3. Test D (middle of C-E): Good
4. Test between D-E: Bad commit found!
```

#### 12.7.1 Using Git Bisect

```bash
# Start bisect
git bisect start

# Mark current commit as bad
git bisect bad

# Mark known good commit
git bisect good v1.0.0

# Output:
Bisecting: 50 revisions left to test after this (roughly 6 steps)
[a1b2c3d...] Commit message

# Test current commit
mvn clean test -Dtest=LoginTests

# If test fails
git bisect bad

# If test passes
git bisect good

# Continue until Git finds the bad commit
# Output:
a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6 is the first bad commit
commit a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6
Author: Developer <dev@example.com>
Date:   Mon Jan 20 14:30:00 2026

    fix: Update login selector

 src/test/pages/LoginPage.java | 1 +
 1 file changed, 1 insertion(+)

# End bisect
git bisect reset
```

#### 12.7.2 Automated Bisect

```bash
# Automated bisect with test script
git bisect start HEAD v1.0.0
git bisect run mvn clean test -Dtest=LoginTests

# Git automatically bisects using test exit code
# 0 = good, non-zero = bad

# Output:
running mvn clean test -Dtest=LoginTests
Bisecting: 25 revisions left to test after this (roughly 5 steps)
running mvn clean test -Dtest=LoginTests
Bisecting: 12 revisions left to test after this (roughly 4 steps)
...
a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6 is the first bad commit
```

### 12.8 Interview Questions - Advanced Commands

**Q1: What is the difference between git reset and git revert?**

**Answer:**
```
git reset:
- Moves branch pointer backward
- Rewrites history
- Removes commits from history
- Dangerous on shared branches

git revert:
- Creates new commit that undoes changes
- Preserves history
- Adds to history (doesn't remove)
- Safe on shared branches

Example:

Reset:
Before:  A---B---C---D
                     ^
                    main

After:   A---B---C
             ^
            main
(D is removed from history)

Revert:
Before:  A---B---C---D
                     ^
                    main

After:   A---B---C---D---D'
                         ^
                        main
(D' undoes D, but D remains in history)

Usage:

Private branch:
git reset --hard HEAD~1  # OK

Shared branch:
git revert HEAD  # Safe

Test Automation Example:
# Bad commit broke tests on main
git checkout main

# Don't use reset (shared branch):
# git reset --hard HEAD~1  # NO!

# Use revert:
git revert HEAD
git push origin main  # Safe
```

**Q2: Explain git rebase vs git merge.**

**Answer:**
```
Merge:
- Combines branches with merge commit
- Preserves complete history
- Shows when feature was integrated
- Non-destructive

Rebase:
- Replays commits on new base
- Creates linear history
- Rewrites commit history
- Cleaner but destructive

Visual:

Merge:
main:    A---B---C-------M
          \             /
feature:   D---E---F---

Rebase:
main:    A---B---C
                  \
feature:           D'---E'---F'

When to Use:

Merge:
- Integrating feature to main
- Working on shared branch
- Want to preserve exact history
- Team uses merge-based workflow

Rebase:
- Updating feature branch with main
- Cleaning up before PR
- Want linear history
- Personal branches only

Test Automation Example:

# Updating feature with main
git checkout feature/api-tests

# Option 1: Merge (preserves history)
git merge main

# Option 2: Rebase (cleaner)
git rebase main

# For PR: Rebase to clean history
git rebase -i main  # Interactive cleanup
git push --force-with-lease

# Merging to main: Use PR merge
# (GitHub handles this)

Rules:
- Never rebase shared branches
- Rebase personal branches before PR
- Use merge for final integration
```

**Q3: How do you use git stash in daily workflow?**

**Answer:**
```
Git Stash Use Cases:

1. Context Switching:
# Working on feature
git stash save "WIP: checkout tests"
git checkout main
# Fix urgent bug
git checkout feature/checkout
git stash pop

2. Pull with Dirty Working Directory:
git stash
git pull origin main
git stash pop

3. Experiment Without Committing:
git stash
# Try different approach
# If doesn't work:
git reset --hard
git stash pop

4. Move Changes to Different Branch:
# Made changes on wrong branch
git stash
git checkout correct-branch
git stash pop

Commands:

# Save changes
git stash
git stash save "message"
git stash -u  # Include untracked

# List stashes
git stash list

# Apply stash
git stash pop     # Apply and drop
git stash apply   # Apply and keep

# Specific stash
git stash apply stash@{1}

# View stash
git stash show -p stash@{0}

# Drop stash
git stash drop stash@{0}
git stash clear  # Remove all

Best Practices:
- Use descriptive messages
- Don't keep stashes too long
- Clean up old stashes
- Prefer commits over long-term stashes

Test Automation Example:
# Writing tests, need to switch
git stash save "Half-done payment tests"
git checkout main
git pull
git checkout -b hotfix/urgent
# Fix issue
git commit -am "hotfix: Critical fix"
git push
git checkout feature/checkout
git stash pop
# Continue testing
```

---

## Conclusion

This comprehensive guide covers Git and Version Control from basics to advanced concepts specifically tailored for Test Automation Engineers. Understanding these concepts is crucial for:

- **Collaboration**: Working effectively in teams
- **Code Quality**: Maintaining high-quality test code through reviews
- **Version Management**: Tracking changes and managing releases
- **CI/CD Integration**: Enabling automated test execution
- **Best Practices**: Following industry-standard workflows

### Key Takeaways

1. **Master the Basics**: init, clone, add, commit, push, pull
2. **Branching Strategy**: Use feature branches for all work
3. **Pull Requests**: Always get code reviewed before merging
4. **Conflict Resolution**: Don't fear conflicts, learn to resolve them
5. **Gitignore**: Never commit build artifacts, test reports, or secrets
6. **Commit Messages**: Write clear, descriptive commit messages
7. **Advanced Commands**: Know when to use rebase, cherry-pick, stash
8. **Workflow**: Follow team conventions consistently

### Resources for Further Learning

```markdown
Official Documentation:
- Git Documentation: https://git-scm.com/doc
- Pro Git Book: https://git-scm.com/book/en/v2

Interactive Learning:
- Learn Git Branching: https://learngitbranching.js.org/
- Git Immersion: https://gitimmersion.com/
- GitHub Learning Lab: https://lab.github.com/

Practice:
- Create sample test automation repository
- Practice branching and merging
- Simulate team workflows
- Experiment with advanced commands in safe environment

Tools:
- GitHub Desktop: https://desktop.github.com/
- GitKraken: https://www.gitkraken.com/
- SourceTree: https://www.sourcetreeapp.com/
```

### Final Interview Tips

```markdown
Be Prepared to Discuss:
1. Your Git workflow in current/previous projects
2. How you handle merge conflicts
3. Branching strategy your team follows
4. CI/CD integration with Git
5. Code review process
6. How you manage test automation code in Git
7. Handling sensitive data (credentials, API keys)
8. Large file management (test data, drivers)
9. Git best practices for test automation
10. Recovery from Git mistakes

Common Interview Questions:
- Explain your typical Git workflow
- Difference between merge and rebase
- How do you resolve merge conflicts?
- What's in your .gitignore for test projects?
- When would you use git stash?
- Explain pull request process
- How do you handle hotfixes?
- What's the difference between git fetch and git pull?
- How do you revert a commit?
- Explain git branching strategies

Practical Tasks:
- Clone repository and run tests
- Create feature branch
- Make changes and commit
- Handle merge conflict
- Create pull request
- Review code changes
- Explain commit history
```

---

**Document Information:**
- **Version**: 2.0
- **Last Updated**: January 28, 2026
- **Total Lines**: ~6000+
- **Sections**: 12
- **Target Audience**: Senior Test Automation Engineers (10+ years experience)
- **Focus**: Practical Git usage in test automation projects

**Feedback and Contributions:**
This is a living document. Feedback and suggestions for improvements are welcome.

---

*End of Git & Version Control Theory Guide for Test Automation Engineers*
