# Phase 3 - Git & Version Control: Practical Exercises (Beginner Step-by-Step)

## Exercise 1: Initialize a Git Repository

### Objective
Set up a new Git repository and make your first commit.

### Step-by-step (Beginner)
1. Create a new folder: `mkdir my-test-project`
2. Navigate into it: `cd my-test-project`
3. Initialize Git: `git init`
4. Create a file: `echo "# Test Project" > README.md`
5. Check status: `git status` (file should be untracked)
6. Stage the file: `git add README.md`
7. Check status again: `git status` (file should be staged)
8. Commit: `git commit -m "Initial commit with README"`
9. View commit history: `git log`

**Deliverable**: Working Git repository with initial commit

---

## Exercise 2: Working Directory, Staging, and Commit

### Objective
Understand the three states of Git.

### Step-by-step (Beginner)
1. In your project, create a test case file: `TestCases.txt`
2. Add some test cases to it (3-4 lines).
3. Check status: `git status` (should show untracked)
4. Stage it: `git add TestCases.txt`
5. Check status: `git status` (should show staged)
6. Modify the file again (add one more line).
7. Check status: `git status` (should show both staged and modified)
8. Stage the new changes: `git add TestCases.txt`
9. Commit: `git commit -m "Add test cases document"`
10. Verify: `git log --oneline`

**Deliverable**: Understanding of working directory, staging area, and repository

---

## Exercise 3: Branching Basics

### Objective
Create and switch between branches.

### Step-by-step (Beginner)
1. View current branch: `git branch`
2. Create a new branch: `git branch feature/login-tests`
3. List all branches: `git branch`
4. Switch to new branch: `git checkout feature/login-tests`
5. Verify current branch: `git branch` (asterisk shows current)
6. Create a file: `LoginTests.txt` with 3 test cases
7. Stage and commit: `git add LoginTests.txt` then `git commit -m "Add login test cases"`
8. Switch back to main: `git checkout main`
9. Check if LoginTests.txt exists (it shouldn't in main branch)
10. Switch back to feature branch and verify file is there

**Deliverable**: Practice switching between branches

---

## Exercise 4: Merging Branches

### Objective
Merge a feature branch into main branch.

### Step-by-step (Beginner)
1. Ensure you are on main branch: `git checkout main`
2. View current files: `ls` or `dir`
3. Merge feature branch: `git merge feature/login-tests`
4. Check files again: `ls` or `dir` (LoginTests.txt should now appear)
5. View commit history: `git log --oneline --graph --all`
6. Delete merged branch: `git branch -d feature/login-tests`
7. Verify branch deleted: `git branch`

**Deliverable**: Successfully merged feature into main

---

## Exercise 5: Resolving Merge Conflicts

### Objective
Create and resolve a merge conflict.

### Step-by-step (Beginner)
1. On main branch, modify README.md: Add line "Version: 1.0"
2. Commit: `git commit -am "Update version in README"`
3. Create new branch: `git checkout -b feature/update-readme`
4. In feature branch, modify same line in README.md: "Version: 2.0"
5. Commit: `git commit -am "Update version to 2.0"`
6. Switch to main: `git checkout main`
7. Try to merge: `git merge feature/update-readme` (conflict will occur)
8. Open README.md and see conflict markers: `<<<<<<<`, `=======`, `>>>>>>>`
9. Resolve by choosing one version or combining both
10. Remove conflict markers
11. Stage resolved file: `git add README.md`
12. Complete merge: `git commit -m "Resolve version conflict"`
13. View history: `git log --oneline --graph --all`

**Deliverable**: Resolved merge conflict

---

## Exercise 6: Viewing Commit History

### Objective
Use various git log options.

### Step-by-step (Beginner)
1. View full log: `git log`
2. View compact log: `git log --oneline`
3. View with graph: `git log --oneline --graph --all`
4. View last 3 commits: `git log -3`
5. View commits by author: `git log --author="YourName"`
6. View commits with files changed: `git log --stat`
7. View specific file history: `git log -- README.md`
8. Show difference in last commit: `git show HEAD`

**Deliverable**: Understanding of git log variations

---

## Exercise 7: Undoing Changes (Working Directory)

### Objective
Discard unstaged changes.

### Step-by-step (Beginner)
1. Modify TestCases.txt (add random text)
2. Check status: `git status`
3. View changes: `git diff`
4. Discard changes: `git checkout -- TestCases.txt` (or `git restore TestCases.txt`)
5. Verify changes reverted: `cat TestCases.txt`

**Deliverable**: Understanding how to discard working directory changes

---

## Exercise 8: Undoing Staged Changes

### Objective
Unstage files without losing changes.

### Step-by-step (Beginner)
1. Modify TestCases.txt again
2. Stage it: `git add TestCases.txt`
3. Check status: `git status` (should show staged)
4. Unstage: `git reset HEAD TestCases.txt` (or `git restore --staged TestCases.txt`)
5. Check status: `git status` (should show modified, not staged)
6. Changes still exist in file, just not staged

**Deliverable**: Understanding how to unstage files

---

## Exercise 9: Amending Last Commit

### Objective
Fix the last commit message or add forgotten files.

### Step-by-step (Beginner)
1. Create file: `BugReport.txt`
2. Stage and commit: `git add BugReport.txt` then `git commit -m "Add bug reort"` (typo intentional)
3. View log: `git log --oneline` (see typo in message)
4. Amend commit: `git commit --amend -m "Add bug report"`
5. View log again: `git log --oneline` (message should be corrected)
6. Note: Only amend commits that haven't been pushed

**Deliverable**: Corrected commit message

---

## Exercise 10: Stashing Changes

### Objective
Temporarily save work without committing.

### Step-by-step (Beginner)
1. Modify TestCases.txt (add one line)
2. Check status: `git status`
3. Stash changes: `git stash`
4. Check status: `git status` (working directory clean)
5. View stash list: `git stash list`
6. Switch to another branch: `git checkout -b hotfix/urgent-bug`
7. Do some work and commit it
8. Switch back: `git checkout main`
9. Reapply stashed changes: `git stash pop`
10. Check status: `git status` (changes are back)

**Deliverable**: Understanding of stashing workflow

---

## Exercise 11: Tagging Releases

### Objective
Create tags for release versions.

### Step-by-step (Beginner)
1. View existing tags: `git tag`
2. Create lightweight tag: `git tag v1.0`
3. View tags: `git tag`
4. Create annotated tag: `git tag -a v1.1 -m "Release version 1.1"`
5. View tag details: `git show v1.1`
6. List all tags: `git tag -l`
7. Tag a specific commit: `git tag v1.0.1 <commit-hash>`

**Deliverable**: Tagged repository with version numbers

---

## Exercise 12: Working with Remote Repository (GitHub)

### Objective
Connect local repo to GitHub and push code.

### Pre-requisite
Create a new empty repository on GitHub (don't initialize with README)

### Step-by-step (Beginner)
1. Copy the remote URL from GitHub
2. Add remote: `git remote add origin <your-github-url>`
3. Verify remote: `git remote -v`
4. Push to remote: `git push -u origin main`
5. Refresh GitHub page and see your code
6. View remote branches: `git branch -r`

**Deliverable**: Code pushed to GitHub

---

## Exercise 13: Cloning a Repository

### Objective
Clone an existing remote repository.

### Step-by-step (Beginner)
1. Find any public GitHub repository URL (or use your own from Exercise 12)
2. Navigate to a different directory: `cd ..`
3. Clone repository: `git clone <repository-url>`
4. Navigate into cloned folder: `cd <repository-name>`
5. View remote: `git remote -v`
6. View branches: `git branch -a`
7. View commit history: `git log --oneline`

**Deliverable**: Successfully cloned repository

---

## Exercise 14: Pull and Push Workflow

### Objective
Fetch changes from remote and push local changes.

### Step-by-step (Beginner)
1. Make a change directly on GitHub (edit README.md via GitHub UI)
2. In your local repo, pull changes: `git pull origin main`
3. View updated file locally
4. Make a local change: modify TestCases.txt
5. Stage and commit: `git commit -am "Update test cases"`
6. Push to remote: `git push origin main`
7. Verify changes on GitHub

**Deliverable**: Understanding of pull-push cycle

---

## Exercise 15: Feature Branch Workflow

### Objective
Complete a realistic feature branch workflow.

### Step-by-step (Beginner)
1. Pull latest: `git pull origin main`
2. Create feature branch: `git checkout -b feature/add-api-tests`
3. Create file: `APITests.txt` with 5 API test scenarios
4. Commit: `git add APITests.txt` then `git commit -m "Add API test scenarios"`
5. Push feature branch: `git push -u origin feature/add-api-tests`
6. Go to GitHub and create a Pull Request
7. Review your own PR (in real work, someone else reviews)
8. Merge PR on GitHub
9. Locally, switch to main: `git checkout main`
10. Pull merged changes: `git pull origin main`
11. Delete local feature branch: `git branch -d feature/add-api-tests`
12. Delete remote branch: `git push origin --delete feature/add-api-tests`

**Deliverable**: Complete feature branch workflow with PR

---

## Exercise 16: Forking and Contributing

### Objective
Simulate contributing to an open-source project.

### Step-by-step (Beginner)
1. Find a public repository on GitHub or use a friend's repository
2. Click "Fork" button on GitHub (creates your own copy)
3. Clone your fork: `git clone <your-fork-url>`
4. Add upstream remote: `git remote add upstream <original-repo-url>`
5. Verify remotes: `git remote -v` (should see origin and upstream)
6. Create branch: `git checkout -b fix/typo-in-readme`
7. Make a small change (fix a typo in README)
8. Commit: `git commit -am "Fix typo in README"`
9. Push to your fork: `git push origin fix/typo-in-readme`
10. Create Pull Request from your fork to original repository

**Deliverable**: Understanding of fork-and-PR workflow

---

## Exercise 17: Gitignore File

### Objective
Exclude files from version control.

### Step-by-step (Beginner)
1. Create a `.gitignore` file in repository root
2. Add patterns:
   ```
   *.log
   temp/
   .env
   node_modules/
   ```
3. Create a test file: `debug.log`
4. Check status: `git status` (debug.log should not appear)
5. Create folder: `mkdir temp` and add file in it
6. Check status: `git status` (temp folder ignored)
7. Commit .gitignore: `git add .gitignore` then `git commit -m "Add gitignore"`

**Deliverable**: Working .gitignore file

---

## Exercise 18: Viewing Differences

### Objective
Compare changes between commits, branches, and staged files.

### Step-by-step (Beginner)
1. Modify TestCases.txt (don't stage)
2. View unstaged changes: `git diff`
3. Stage the file: `git add TestCases.txt`
4. View staged changes: `git diff --staged`
5. Commit the changes
6. View difference between current and last commit: `git diff HEAD~1`
7. View difference between two branches: `git diff main feature/login-tests`
8. View difference for specific file: `git diff HEAD~2 HEAD -- TestCases.txt`

**Deliverable**: Understanding of git diff variations

---

## Exercise 19: Cherry-Pick Commit

### Objective
Apply a specific commit from one branch to another.

### Step-by-step (Beginner)
1. Create branch: `git checkout -b experiment`
2. Make 3 commits with different files
3. Note the commit hash of the second commit: `git log --oneline`
4. Switch to main: `git checkout main`
5. Cherry-pick that commit: `git cherry-pick <commit-hash>`
6. View log: `git log --oneline` (that specific commit now in main)
7. Note: Only the chosen commit is applied, not all commits from experiment branch

**Deliverable**: Understanding of selective commit application

---

## Exercise 20: Rebase Basics

### Objective
Rebase a feature branch onto main.

### Step-by-step (Beginner)
1. On main, create file: `main1.txt` and commit
2. Create branch: `git checkout -b feature/rebase-test`
3. On feature branch, create file: `feature1.txt` and commit
4. Switch to main: `git checkout main`
5. Create file: `main2.txt` and commit
6. Switch back to feature: `git checkout feature/rebase-test`
7. Rebase onto main: `git rebase main`
8. View history: `git log --oneline --graph --all` (linear history)
9. Compare with merge (doesn't create merge commit)

**Deliverable**: Understanding difference between merge and rebase

---

## Exercise 21: Reset Commits (Soft, Mixed, Hard)

### Objective
Understand different reset modes.

### Step-by-step (Beginner)
1. Create 3 files and commit each: `file1.txt`, `file2.txt`, `file3.txt`
2. View log: `git log --oneline`
3. Note the commit hash before last commit
4. Soft reset: `git reset --soft HEAD~1`
   - Last commit undone, changes still staged
5. Check status: `git status`
6. Commit again: `git commit -m "Re-add file3"`
7. Mixed reset: `git reset HEAD~1`
   - Last commit undone, changes unstaged
8. Check status: `git status`
9. Stage and commit again
10. Hard reset (CAUTION): `git reset --hard HEAD~1`
    - Last commit undone, changes discarded permanently
11. Verify: `ls` (file3.txt gone)

**Deliverable**: Understanding of reset modes (use with caution!)

---

## Exercise 22: Collaborative Conflict Resolution

### Objective
Simulate team conflict and resolution.

### Step-by-step (Beginner)
1. Two team members work on same file (simulate with two branches)
2. Main branch: modify line 1 of README.md
3. Commit: `git commit -am "Update line 1 by person A"`
4. Create branch: `git checkout -b teammate-work`
5. Modify same line differently
6. Commit: `git commit -am "Update line 1 by person B"`
7. Switch to main: `git checkout main`
8. Try merge: `git merge teammate-work` (conflict!)
9. Open README.md, review both changes
10. Decide which to keep or combine both
11. Remove conflict markers
12. Stage: `git add README.md`
13. Complete merge: `git commit -m "Resolve conflict between A and B"`

**Deliverable**: Realistic conflict resolution practice

---

## Exercise 23: Git Blame for Code Review

### Objective
Track who made specific changes.

### Step-by-step (Beginner)
1. View who last modified each line: `git blame TestCases.txt`
2. View with shorter commit hashes: `git blame -s TestCases.txt`
3. View line range: `git blame -L 5,10 TestCases.txt`
4. See full commit details: `git log -p <commit-hash>`

**Deliverable**: Understanding how to trace changes

---

## Exercise 24: Git Bisect for Bug Hunting

### Objective
Find the commit that introduced a bug.

### Step-by-step (Beginner)
1. Create 10 simple commits (each adding a number to a file)
2. Introduce a "bug" in commit 6 (add text "BUG" instead of number)
3. Start bisect: `git bisect start`
4. Mark current as bad: `git bisect bad`
5. Mark first commit as good: `git bisect good <first-commit-hash>`
6. Git checks out middle commit
7. Check if bug exists
8. Mark as good or bad: `git bisect good` or `git bisect bad`
9. Repeat until Git identifies the exact commit
10. View result: Git shows the faulty commit
11. End bisect: `git bisect reset`

**Deliverable**: Understanding binary search for bug detection

---

## Exercise 25: Managing Multiple Remotes

### Objective
Work with multiple remote repositories.

### Step-by-step (Beginner)
1. Clone a repository: `git clone <url>`
2. View remotes: `git remote -v`
3. Add second remote: `git remote add backup <another-url>`
4. View remotes: `git remote -v` (should show origin and backup)
5. Push to specific remote: `git push origin main`
6. Push to backup: `git push backup main`
7. Rename remote: `git remote rename backup secondary`
8. Remove remote: `git remote remove secondary`

**Deliverable**: Understanding multiple remote management

---

## Exercise 26: Git Hooks Basics

### Objective
Create a simple pre-commit hook.

### Step-by-step (Beginner)
1. Navigate to: `cd .git/hooks`
2. List samples: `ls`
3. Create pre-commit hook: `nano pre-commit` (or use any editor)
4. Add simple script:
   ```bash
   #!/bin/sh
   echo "Running pre-commit checks..."
   echo "Commit allowed!"
   ```
5. Make executable: `chmod +x pre-commit`
6. Go back to repo root: `cd ../..`
7. Make a change and try to commit
8. You should see your pre-commit message

**Deliverable**: Basic understanding of Git hooks

---

## Exercise 27: Git Reflog (Recovering Lost Commits)

### Objective
Recover commits after reset.

### Step-by-step (Beginner)
1. Create file and commit: `echo "test" > recover.txt` then commit
2. Note the commit hash: `git log --oneline`
3. Hard reset: `git reset --hard HEAD~1`
4. File is gone: `ls` (no recover.txt)
5. View reflog: `git reflog`
6. Find the lost commit hash in reflog
7. Recover it: `git checkout <lost-commit-hash>`
8. Or reset to it: `git reset --hard <lost-commit-hash>`
9. File is back!

**Deliverable**: Understanding that Git rarely loses data permanently

---

## Exercise 28: Squashing Commits

### Objective
Combine multiple commits into one.

### Step-by-step (Beginner)
1. Create branch: `git checkout -b feature/multiple-commits`
2. Make 4 small commits (add 4 files separately)
3. View log: `git log --oneline`
4. Interactive rebase: `git rebase -i HEAD~4`
5. In editor, change "pick" to "squash" for last 3 commits
6. Save and close
7. Edit combined commit message
8. View log: `git log --oneline` (now single commit)

**Deliverable**: Cleaner commit history

---

## Exercise 29: Git Worktree

### Objective
Work on multiple branches simultaneously.

### Step-by-step (Beginner)
1. View current worktrees: `git worktree list`
2. Add new worktree: `git worktree add ../project-feature2 -b feature/new-feature`
3. Navigate to new location: `cd ../project-feature2`
4. Work on this branch while main branch is in original location
5. Both folders work independently
6. Remove worktree: `git worktree remove ../project-feature2`

**Deliverable**: Understanding parallel branch work

---

## Exercise 30: Complete Testing Workflow Simulation

### Objective
Simulate real-world testing team Git workflow.

### Scenario
You're part of a testing team working on an e-commerce project.

### Step-by-step (Beginner)
1. Clone main repository (your project repo)
2. Pull latest: `git pull origin main`
3. Create feature branch: `git checkout -b test/checkout-validation`
4. Create test case file: `CheckoutTests.md` with 10 test cases
5. Commit: `git commit -am "Add checkout validation test cases"`
6. Create defect report: `Defects.md` with 3 bugs found
7. Commit: `git commit -am "Document checkout defects"`
8. Push branch: `git push -u origin test/checkout-validation`
9. Create Pull Request on GitHub
10. Request review from lead
11. Lead suggests changes in review comments
12. Make changes locally: update CheckoutTests.md
13. Commit: `git commit -am "Address review comments"`
14. Push: `git push origin test/checkout-validation`
15. PR gets approved and merged
16. Switch to main: `git checkout main`
17. Pull merged changes: `git pull origin main`
18. Delete branch: `git branch -d test/checkout-validation`
19. Tag the release: `git tag -a v1.2.0 -m "Checkout tests complete"`
20. Push tag: `git push origin v1.2.0`

**Deliverable**: Complete professional Git workflow for testing team
