# Phase 3 - Supporting Skills: Interview Questions and Answers

## Section 1: Git & Version Control (Questions 1-15)

### Q1: What is Git and why is it important for testers?

**Answer**:
Git is a distributed version control system that tracks code changes over time.

**Why important for testers**:
- Version test scripts and test data
- Collaborate with team on test automation
- Track defects with commit history
- Integrate with CI/CD for automated testing
- Roll back test code if something breaks

**Practical Example**:
A tester commits test cases to Git, pushes to GitHub, and Jenkins automatically runs those tests on every code change.

**In-depth Explanation**:
Git enables collaboration, prevents test code loss, and provides audit trails for test changes.

---

### Q2: Explain the Git workflow: Working Directory, Staging Area, Repository.

**Answer**:
Git has three states:
1. **Working Directory**: Where you modify files
2. **Staging Area (Index)**: Where you prepare files for commit
3. **Repository**: Where committed files are permanently stored

**Practical Example**:
1. Modify `LoginTest.java` (Working Directory)
2. `git add LoginTest.java` (moves to Staging)
3. `git commit -m "Update login tests"` (saves to Repository)

**In-depth Explanation**:
Staging allows selective commits - you can modify 5 files but commit only 2. This gives precise control over what changes are saved together.

---

### Q3: What is the difference between git pull and git fetch?

**Answer**:
- **git fetch**: Downloads changes from remote but does NOT merge them
- **git pull**: Downloads changes AND merges them into current branch (fetch + merge)

**Practical Example**:
```bash
git fetch origin main        # Download changes, review them
git log origin/main          # See what changed
git merge origin/main        # Merge when ready

# OR

git pull origin main         # Download and merge immediately
```

**In-depth Explanation**:
`fetch` is safer for reviewing changes before integrating. `pull` is faster but can cause unexpected merges.

---

### Q4: How do you resolve a merge conflict in Git?

**Answer**:
1. Attempt merge: `git merge branch-name`
2. Git shows conflict message
3. Open conflicted files (marked with `<<<<<<<`, `=======`, `>>>>>>>`)
4. Edit to resolve conflict (keep one version or combine both)
5. Remove conflict markers
6. Stage resolved file: `git add file.txt`
7. Complete merge: `git commit -m "Resolve conflict"`

**Practical Example**:
Two testers modify the same test case file. Git marks conflicting lines. You review both changes, keep the better one, remove markers, stage, and commit.

**In-depth Explanation**:
Conflicts happen when same lines are modified in different branches. Manual review ensures correct resolution.

---

### Q5: What is branching in Git and why is it used?

**Answer**:
Branching creates an independent line of development. You can work on features without affecting main code.

**Practical Example**:
```bash
git checkout -b feature/api-tests    # Create and switch to new branch
# Work on API tests
git commit -am "Add API test suite"
git checkout main                     # Switch back to main
git merge feature/api-tests          # Merge when ready
```

**In-depth Explanation**:
Branches isolate work, enable parallel development, and make code review easier through pull requests.

---

### Q6: What is the difference between merge and rebase?

**Answer**:
- **Merge**: Combines branches and creates a merge commit (preserves history)
- **Rebase**: Moves commits from one branch onto another (linear history)

**Practical Example**:
```bash
# Merge (creates merge commit)
git checkout main
git merge feature-branch

# Rebase (no merge commit, linear)
git checkout feature-branch
git rebase main
```

**In-depth Explanation**:
Merge is safer and preserves full history. Rebase creates cleaner history but rewrites commits (don't rebase public branches).

---

### Q7: How do you undo changes in Git?

**Answer**:
Depends on the state:

1. **Unstaged changes**: `git checkout -- file.txt` or `git restore file.txt`
2. **Staged changes**: `git reset HEAD file.txt` then restore if needed
3. **Committed changes**: `git revert commit-hash` (safe) or `git reset` (dangerous)

**Practical Example**:
```bash
# Modified file, not staged
git restore TestCases.txt

# Staged file
git restore --staged TestCases.txt

# Undo last commit (keep changes)
git reset --soft HEAD~1

# Undo last commit (discard changes)
git reset --hard HEAD~1  # CAUTION: loses work
```

**In-depth Explanation**:
Use `restore`/`reset` for local changes, `revert` for published commits (creates new commit that undoes changes).

---

### Q8: What is git stash and when would you use it?

**Answer**:
`git stash` temporarily saves work without committing.

**Use cases**:
- Need to switch branches but have uncommitted work
- Want to pull latest code but have local changes
- Interrupted work, need to work on urgent bug

**Practical Example**:
```bash
# Working on test cases, urgent bug reported
git stash                    # Save current work
git checkout hotfix-branch   # Switch to fix bug
# Fix bug, commit
git checkout original-branch
git stash pop                # Restore saved work
```

**In-depth Explanation**:
Stash is like a clipboard for uncommitted changes. Multiple stashes can exist (use `git stash list`).

---

### Q9: Explain git cherry-pick with a use case.

**Answer**:
`git cherry-pick` applies a specific commit from one branch to another (without merging entire branch).

**Practical Example**:
```bash
# Bug fix in feature branch, need same fix in main
git checkout main
git cherry-pick abc123       # Apply only commit abc123
```

**Use case for testers**:
A critical test fix is made in a feature branch. Instead of merging entire feature, cherry-pick just the fix to main.

**In-depth Explanation**:
Cherry-pick is selective - you choose specific commits. Useful when you need one change but not the whole branch.

---

### Q10: What is a pull request (PR) and how does it relate to testing?

**Answer**:
A pull request is a request to merge code from one branch to another, typically with code review.

**Testing relation**:
- Test code changes before merge
- Review test cases in PR
- CI/CD runs automated tests on PR
- Ensures quality before merging

**Practical Example**:
1. Create feature branch with new tests
2. Push to GitHub: `git push origin feature/new-tests`
3. Open PR on GitHub
4. CI runs tests automatically
5. Team reviews test code
6. Merge when approved and tests pass

**In-depth Explanation**:
PRs enable collaboration, code review, and automated testing before code reaches main branch.

---

### Q11: How do you view commit history in Git?

**Answer**:
Multiple ways:

```bash
git log                        # Full log
git log --oneline              # Compact view
git log --graph --all          # Visual branch graph
git log -3                     # Last 3 commits
git log --author="Name"        # Commits by author
git log -- file.txt            # Commits affecting file
git log -p                     # Show diff in each commit
git show commit-hash           # Details of specific commit
```

**Practical Example**:
To find when a test was added: `git log --oneline -- LoginTest.java`

**In-depth Explanation**:
Log helps track changes, debug issues, and understand code evolution.

---

### Q12: What is .gitignore and what should testers include in it?

**Answer**:
`.gitignore` specifies files Git should ignore (not track).

**Testers should ignore**:
```
# Build outputs
target/
build/
dist/
bin/

# Dependencies
node_modules/
.m2/

# IDE files
.idea/
.vscode/
*.iml

# Test reports
test-results/
screenshots/
*.log

# Environment files
.env
config.local.js

# OS files
.DS_Store
Thumbs.db
```

**In-depth Explanation**:
Ignoring these files keeps repo clean, reduces conflicts, and protects sensitive data.

---

### Q13: What are Git tags and when would testers use them?

**Answer**:
Tags mark specific points in history (usually releases).

**Types**:
- Lightweight: `git tag v1.0`
- Annotated: `git tag -a v1.0 -m "Release 1.0"`

**Tester use cases**:
- Tag release versions for testing
- Mark when all tests passed
- Reference specific build versions
- Track which code was tested

**Practical Example**:
```bash
git tag -a v2.1.0 -m "All regression tests passed"
git push origin v2.1.0
```

**In-depth Explanation**:
Tags create permanent markers, unlike branches which move. Useful for traceability.

---

### Q14: How does Git integrate with CI/CD for testing?

**Answer**:
CI/CD tools monitor Git repositories and trigger actions on code changes.

**Workflow**:
1. Developer/tester pushes code to Git
2. Webhook notifies CI/CD tool (Jenkins, GitLab CI, GitHub Actions)
3. CI/CD pulls latest code
4. Runs build and tests automatically
5. Reports results back (pass/fail)
6. Blocks merge if tests fail

**Practical Example**:
```yaml
# GitHub Actions example
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - run: npm install
      - run: npm test
```

**In-depth Explanation**:
Git + CI/CD enables continuous testing, faster feedback, and prevents bad code from reaching production.

---

### Q15: What is git blame and how can testers use it?

**Answer**:
`git blame` shows who last modified each line of a file.

**Practical Example**:
```bash
git blame TestCases.txt
# Output shows: commit-hash, author, date, line-number, content
```

**Tester use cases**:
- Find who wrote a specific test
- Trace test failures to recent changes
- Understand test case history
- Contact person who knows the code

**In-depth Explanation**:
Blame is not about fault-finding but about tracing code origin for context and collaboration.

---

## Section 2: Maven (Questions 16-22)

### Q16: What is Maven and why do testers need to know it?

**Answer**:
Maven is a build automation and dependency management tool for Java projects.

**Why testers need it**:
- Run Java-based automated tests (Selenium, TestNG, JUnit)
- Manage test dependencies (libraries, frameworks)
- Execute tests from command line or CI/CD
- Understand project structure and build process

**Practical Example**:
```bash
mvn test              # Run all tests
mvn test -Dtest=LoginTest   # Run specific test
```

**In-depth Explanation**:
Maven standardizes Java project structure and automates compilation, testing, and packaging.

---

### Q17: What is pom.xml?

**Answer**:
`pom.xml` (Project Object Model) is Maven's configuration file. It defines:
- Project information (groupId, artifactId, version)
- Dependencies (libraries needed)
- Plugins (tools for build/test)
- Build configuration

**Practical Example**:
```xml
<project>
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.company.test</groupId>
    <artifactId>automation-tests</artifactId>
    <version>1.0.0</version>

    <dependencies>
        <dependency>
            <groupId>org.seleniumhq.selenium</groupId>
            <artifactId>selenium-java</artifactId>
            <version>4.8.0</version>
        </dependency>
    </dependencies>
</project>
```

**In-depth Explanation**:
pom.xml is the heart of Maven - it tells Maven what the project is, what it needs, and how to build it.

---

### Q18: Explain Maven build lifecycle phases.

**Answer**:
Maven has a default lifecycle with key phases:

1. **validate**: Check project structure
2. **compile**: Compile source code
3. **test**: Run unit tests
4. **package**: Create JAR/WAR
5. **verify**: Run integration tests
6. **install**: Install to local repository
7. **deploy**: Deploy to remote repository

**Practical Example**:
```bash
mvn test       # Runs: validate → compile → test
mvn package    # Runs: validate → compile → test → package
mvn install    # Runs all up to install
```

**In-depth Explanation**:
Running a phase automatically runs all preceding phases. This ensures proper build order.

---

### Q19: What are dependency scopes in Maven?

**Answer**:
Scopes define when dependencies are available:

- **compile** (default): Available everywhere (compile, test, runtime)
- **test**: Only for testing (e.g., JUnit, TestNG)
- **provided**: Provided by runtime (e.g., Servlet API)
- **runtime**: Not for compilation, only execution

**Practical Example**:
```xml
<dependency>
    <groupId>org.testng</groupId>
    <artifactId>testng</artifactId>
    <version>7.7.1</version>
    <scope>test</scope>  <!-- Only in test classpath -->
</dependency>
```

**In-depth Explanation**:
Scopes optimize build by including dependencies only where needed, reducing bloat.

---

### Q20: How do you run specific tests in Maven?

**Answer**:
Use `-Dtest` parameter:

```bash
mvn test                                  # All tests
mvn test -Dtest=LoginTest                 # Single class
mvn test -Dtest=LoginTest#testValidLogin  # Single method
mvn test -Dtest=Login*                    # Pattern matching
mvn test -Dtest=LoginTest,CheckoutTest    # Multiple classes
```

**Practical Example**:
Running only smoke tests:
```bash
mvn test -Dtest=*SmokeTest
```

**In-depth Explanation**:
Selective test execution saves time during development and debugging.

---

### Q21: What is Maven Surefire plugin?

**Answer**:
Surefire is the plugin that runs unit tests during `mvn test`.

**Features**:
- Runs JUnit and TestNG tests
- Generates test reports (XML, HTML)
- Supports parallel execution
- Test filtering and grouping

**Practical Example**:
```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-surefire-plugin</artifactId>
    <version>2.22.2</version>
    <configuration>
        <parallel>methods</parallel>
        <threadCount>4</threadCount>
        <includes>
            <include>**/*Test.java</include>
        </includes>
    </configuration>
</plugin>
```

**In-depth Explanation**:
Surefire integrates testing frameworks with Maven, enabling automated test execution and reporting.

---

### Q22: Where does Maven store dependencies?

**Answer**:
Local repository: `~/.m2/repository` (Mac/Linux) or `C:\Users\YourName\.m2\repository` (Windows)

**How it works**:
1. Maven checks local repository first
2. If not found, downloads from Maven Central
3. Stores in local repository
4. Reuses for all projects (saves space and time)

**Practical Example**:
After running `mvn install`, Selenium JAR is at:
`~/.m2/repository/org/seleniumhq/selenium/selenium-java/4.8.0/`

**In-depth Explanation**:
Local repository acts as cache, reducing downloads and enabling offline builds.

---

## Section 3: npm (Questions 23-28)

### Q23: What is npm and why do testers need to know it?

**Answer**:
npm (Node Package Manager) manages JavaScript dependencies and tools.

**Why testers need it**:
- Install test frameworks (Mocha, Jest, Playwright, Cypress)
- Manage test dependencies
- Run test scripts
- JavaScript is common for web testing automation

**Practical Example**:
```bash
npm install --save-dev playwright
npm test
```

**In-depth Explanation**:
npm is essential for JavaScript-based test automation, especially for modern web testing tools.

---

### Q24: What is package.json and what should it include for testing?

**Answer**:
`package.json` defines npm project metadata and dependencies.

**For testing projects**:
```json
{
  "name": "test-automation",
  "version": "1.0.0",
  "scripts": {
    "test": "mocha tests/**/*.js",
    "test:smoke": "mocha tests/smoke/**/*.js",
    "test:regression": "mocha tests/regression/**/*.js"
  },
  "devDependencies": {
    "mocha": "^10.0.0",
    "chai": "^4.3.6",
    "playwright": "^1.30.0"
  }
}
```

**In-depth Explanation**:
package.json is npm's equivalent to Maven's pom.xml - it defines what the project is and needs.

---

### Q25: What is the difference between dependencies and devDependencies?

**Answer**:
- **dependencies**: Required for production/runtime
- **devDependencies**: Only needed for development/testing

**Practical Example**:
```json
{
  "dependencies": {
    "express": "^4.18.0"      // Needed in production
  },
  "devDependencies": {
    "mocha": "^10.0.0",       // Only for testing
    "chai": "^4.3.6"          // Only for testing
  }
}
```

**Installation**:
```bash
npm install --save express        # Adds to dependencies
npm install --save-dev mocha      # Adds to devDependencies
npm install --production          # Skips devDependencies (CI prod build)
```

**In-depth Explanation**:
Separation keeps production builds lean and clarifies dependency purposes.

---

### Q26: What is package-lock.json and why is it important?

**Answer**:
`package-lock.json` locks exact versions of all dependencies (including transitive ones).

**Why important**:
- Ensures consistent installs across team
- Prevents "works on my machine" issues
- Faster installs (knows exact versions)
- Security (prevents unexpected updates)

**Practical Example**:
Developer installs Playwright v1.30.0. package-lock.json records this. When tester runs `npm install`, they get exact same version, not v1.31.0.

**In-depth Explanation**:
Always commit package-lock.json to Git. It ensures reproducible builds.

---

### Q27: Explain semantic versioning (^, ~, exact) in npm.

**Answer**:
Format: `MAJOR.MINOR.PATCH` (e.g., 2.4.1)

**Version ranges**:
- `"2.4.1"`: Exact version
- `"^2.4.1"`: Compatible (>=2.4.1, <3.0.0) - default
- `"~2.4.1"`: Approximately (>=2.4.1, <2.5.0)
- `"*"`: Any version (avoid!)

**Practical Example**:
```json
{
  "dependencies": {
    "playwright": "^1.30.0"  // Accepts 1.30.x and 1.31.x, not 2.x
  }
}
```

**In-depth Explanation**:
`^` allows minor/patch updates (usually safe). Exact versions provide maximum stability.

---

### Q28: How do you run tests using npm scripts?

**Answer**:
Define scripts in package.json, then run with `npm run <script-name>`.

**Example**:
```json
{
  "scripts": {
    "test": "mocha tests/**/*.js",
    "test:unit": "mocha tests/unit/**/*.js",
    "test:e2e": "playwright test",
    "test:chrome": "mocha --browser chrome",
    "lint": "eslint src/**/*.js"
  }
}
```

**Running**:
```bash
npm test              # Runs "test" script (shortcut)
npm run test:unit     # Runs "test:unit" script
npm run test:chrome   # Runs with Chrome
```

**In-depth Explanation**:
npm scripts standardize commands across team and simplify CI/CD integration.

---

## Section 4: General Supporting Skills (Questions 29-30)

### Q29: How do build tools (Maven/npm) integrate with CI/CD?

**Answer**:
CI/CD tools execute build tool commands automatically on code changes.

**Jenkins with Maven**:
```groovy
pipeline {
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
    }
}
```

**Jenkins with npm**:
```groovy
pipeline {
    stages {
        stage('Install') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh 'npm test'
            }
        }
    }
}
```

**In-depth Explanation**:
Build tools provide standardized commands that CI/CD tools can reliably execute, enabling automated testing.

---

### Q30: How would you troubleshoot a failing build in Maven or npm?

**Answer**:

**Maven troubleshooting**:
1. Check Java version: `mvn -version`
2. Clear cache: `mvn clean` and `rm -rf ~/.m2/repository/[problematic-package]`
3. Update dependencies: `mvn -U clean install`
4. Check for conflicts: `mvn dependency:tree`
5. Review error logs carefully
6. Verify pom.xml syntax (XML validation)

**npm troubleshooting**:
1. Check Node version: `node -v` and `npm -v`
2. Clear cache: `npm cache clean --force`
3. Delete and reinstall: `rm -rf node_modules package-lock.json && npm install`
4. Check for outdated packages: `npm outdated`
5. Audit security: `npm audit`
6. Review error logs (usually clear error messages)

**General approach**:
- Read error messages carefully
- Google exact error message
- Check if dependencies are compatible
- Verify environment setup (Java/Node versions)
- Test in clean environment

**Practical Example**:
Maven compilation error: "cannot find symbol"
→ Check if dependency is in pom.xml
→ Run `mvn clean install` to refresh
→ Verify import statements match dependency

npm error: "Cannot find module"
→ Run `rm -rf node_modules && npm install`
→ Check if dependency is in package.json
→ Verify spelling in require() statement

**In-depth Explanation**:
Most build issues are dependency-related (wrong version, missing package, conflict). Clearing cache and reinstalling often resolves issues. Always read error messages fully - they usually point to the problem.

---

## Summary: Key Takeaways

### Git Essentials
- Three states: Working Directory, Staging, Repository
- Branching enables parallel work
- Pull requests enable code review and testing
- Merge vs rebase for integrating changes
- Stash for temporary work saving
- Tags for marking releases

### Maven Essentials
- pom.xml defines project and dependencies
- Build lifecycle: compile → test → package → install
- Dependency scopes: compile, test, provided, runtime
- Surefire plugin runs tests
- Local repository: ~/.m2/repository
- Commands: mvn test, mvn install, mvn clean

### npm Essentials
- package.json defines project and dependencies
- dependencies vs devDependencies
- package-lock.json locks versions
- Semantic versioning: ^, ~, exact
- npm scripts for custom commands
- node_modules for installed packages
- Commands: npm install, npm test, npm update

### Testing Integration
- Build tools execute automated tests
- CI/CD tools use build tools for continuous testing
- Version control tracks test code changes
- All three work together in modern testing workflows

---

## Interview Tips

1. **Be practical**: Back answers with real examples from projects
2. **Show understanding**: Don't just memorize commands, explain why and when
3. **Connect to testing**: Always relate to testing context
4. **Troubleshooting mindset**: Show you can debug build issues
5. **Team collaboration**: Emphasize how these tools enable teamwork

**Common follow-up questions**:
- "Have you used this in a real project?"
- "How would you handle [specific scenario]?"
- "What would you do if [problem occurs]?"

Be ready with specific examples from your experience!
