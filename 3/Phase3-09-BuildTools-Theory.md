# Phase 3 - Build Tools: Comprehensive Theory (Maven & npm)

## Part 1: Introduction to Build Tools

### What is a Build Tool?

A build tool automates the process of:
- Compiling source code
- Managing dependencies
- Running tests
- Packaging applications
- Deploying artifacts

### Why Do Testers Need to Know Build Tools?

1. **Test Execution**: Many automated tests run through build tools
2. **Environment Setup**: Understanding dependencies helps set up test environments
3. **CI/CD Integration**: Build tools are central to continuous testing
4. **Debugging**: Knowing build process helps troubleshoot test failures
5. **Collaboration**: Better communication with developers

### Common Build Tools by Language

- **Java**: Maven, Gradle, Ant
- **JavaScript/Node.js**: npm, yarn, webpack
- **Python**: pip, setuptools
- **.NET**: MSBuild, NuGet
- **Ruby**: Bundler, Rake

---

## Part 2: Apache Maven (Java Build Tool)

### What is Maven?

Maven is a build automation and project management tool primarily for Java projects. It uses XML configuration (pom.xml) and follows convention over configuration.

### Key Maven Concepts

#### 1. POM (Project Object Model)

The `pom.xml` file is the heart of a Maven project.

**Basic pom.xml structure**:
```xml
<project>
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.company.app</groupId>
    <artifactId>my-application</artifactId>
    <version>1.0.0</version>
    <packaging>jar</packaging>

    <dependencies>
        <!-- Dependencies listed here -->
    </dependencies>
</project>
```

**Key Elements**:
- **groupId**: Organization identifier (e.g., com.company)
- **artifactId**: Project name
- **version**: Project version
- **packaging**: Output type (jar, war, pom)

#### 2. Maven Coordinates (GAV)

Every Maven artifact is uniquely identified by:
- **G**roupId
- **A**rtifactId
- **V**ersion

Example: `com.google.guava:guava:31.0-jre`

#### 3. Dependencies

External libraries your project needs.

```xml
<dependencies>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.13.2</version>
        <scope>test</scope>
    </dependency>
</dependencies>
```

**Dependency Scopes**:
- **compile**: Default, available in all classpaths
- **test**: Only for testing (e.g., JUnit)
- **provided**: Provided by runtime (e.g., Servlet API)
- **runtime**: Not needed for compilation, needed for execution

#### 4. Maven Repository

**Three types**:
1. **Local Repository**: `~/.m2/repository` on your machine
2. **Central Repository**: Maven Central (https://repo.maven.apache.org)
3. **Remote Repository**: Company or custom repositories

**How it works**:
1. Maven checks local repository first
2. If not found, downloads from central/remote
3. Stores in local repository for future use

#### 5. Maven Build Lifecycle

Maven has three built-in lifecycles:

##### A. Default Lifecycle (Main Build)
Key phases in order:
1. **validate**: Validate project structure
2. **compile**: Compile source code
3. **test**: Run unit tests
4. **package**: Package into JAR/WAR
5. **verify**: Run integration tests
6. **install**: Install to local repository
7. **deploy**: Deploy to remote repository

##### B. Clean Lifecycle
- **clean**: Delete target directory (compiled files)

##### C. Site Lifecycle
- **site**: Generate project documentation

**Important**: When you run a phase, all preceding phases run automatically.

Example:
```bash
mvn test    # Runs: validate → compile → test
mvn package # Runs: validate → compile → test → package
```

#### 6. Maven Plugins

Plugins provide specific functionality.

**Common Plugins**:
- **maven-compiler-plugin**: Compiles Java code
- **maven-surefire-plugin**: Runs unit tests
- **maven-failsafe-plugin**: Runs integration tests
- **maven-jar-plugin**: Creates JAR files

Example configuration:
```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-surefire-plugin</artifactId>
            <version>2.22.2</version>
            <configuration>
                <includes>
                    <include>**/*Test.java</include>
                </includes>
            </configuration>
        </plugin>
    </plugins>
</build>
```

#### 7. Maven Properties

Variables to avoid repetition.

```xml
<properties>
    <java.version>11</java.version>
    <junit.version>5.8.2</junit.version>
</properties>

<dependencies>
    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter</artifactId>
        <version>${junit.version}</version>
    </dependency>
</dependencies>
```

### Maven Project Structure (Convention)

```
my-project/
├── pom.xml
├── src/
│   ├── main/
│   │   ├── java/           # Application source code
│   │   └── resources/      # Configuration files
│   └── test/
│       ├── java/           # Test source code
│       └── resources/      # Test configuration
└── target/                 # Compiled output (generated)
    ├── classes/
    ├── test-classes/
    └── my-project-1.0.jar
```

### Common Maven Commands

```bash
mvn clean                  # Delete target directory
mvn compile                # Compile source code
mvn test                   # Run unit tests
mvn package                # Create JAR/WAR
mvn install                # Install to local repo
mvn clean install          # Clean + compile + test + package + install
mvn clean test             # Clean and run tests
mvn dependency:tree        # Show dependency hierarchy
mvn dependency:resolve     # Download all dependencies
mvn help:effective-pom     # Show complete POM with inherited values
```

### Maven for Testers - Practical Use Cases

#### Running Tests
```bash
mvn test                              # Run all tests
mvn test -Dtest=LoginTest             # Run specific test class
mvn test -Dtest=LoginTest#testValidLogin  # Run specific test method
mvn test -DskipTests=true             # Skip tests (compilation happens)
mvn package -Dmaven.test.skip=true    # Skip tests completely
```

#### Viewing Test Reports
After running tests, reports are in:
- `target/surefire-reports/` (unit tests)
- `target/failsafe-reports/` (integration tests)

#### Setting Test Properties
```bash
mvn test -Dbrowser=chrome -Denv=staging
```

In test code, access with:
```java
String browser = System.getProperty("browser", "firefox");
```

### Dependency Management

#### Transitive Dependencies

Maven automatically downloads dependencies of dependencies.

Example:
- Your project depends on Spring Framework
- Spring depends on Commons Logging
- Maven downloads both automatically

#### Dependency Conflicts

If two dependencies need different versions of the same library:
- Maven uses "nearest definition" rule
- You can explicitly exclude or force a version

```xml
<dependency>
    <groupId>com.example</groupId>
    <artifactId>library-a</artifactId>
    <version>1.0</version>
    <exclusions>
        <exclusion>
            <groupId>commons-logging</groupId>
            <artifactId>commons-logging</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```

### Maven Profiles

Profiles allow environment-specific configurations.

```xml
<profiles>
    <profile>
        <id>dev</id>
        <properties>
            <env>development</env>
        </properties>
    </profile>
    <profile>
        <id>prod</id>
        <properties>
            <env>production</env>
        </properties>
    </profile>
</profiles>
```

Activate profile:
```bash
mvn test -Pdev
```

---

## Part 3: npm (Node Package Manager)

### What is npm?

npm is the default package manager for Node.js. It manages JavaScript dependencies and provides tools for running scripts and publishing packages.

### Key npm Concepts

#### 1. package.json

The `package.json` file is the heart of an npm project.

**Basic structure**:
```json
{
  "name": "my-test-project",
  "version": "1.0.0",
  "description": "Test automation project",
  "main": "index.js",
  "scripts": {
    "test": "mocha tests/**/*.js",
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.18.0"
  },
  "devDependencies": {
    "mocha": "^10.0.0",
    "chai": "^4.3.0"
  }
}
```

**Key Fields**:
- **name**: Project name (lowercase, no spaces)
- **version**: Project version (semantic versioning)
- **scripts**: Custom commands
- **dependencies**: Production dependencies
- **devDependencies**: Development/testing dependencies

#### 2. Semantic Versioning (SemVer)

Format: `MAJOR.MINOR.PATCH` (e.g., 1.4.2)

- **MAJOR**: Breaking changes
- **MINOR**: New features (backward compatible)
- **PATCH**: Bug fixes

**Version Ranges**:
- `"1.4.2"`: Exact version
- `"^1.4.2"`: Compatible with 1.x.x (>= 1.4.2, < 2.0.0)
- `"~1.4.2"`: Approximately equivalent (>= 1.4.2, < 1.5.0)
- `"*"` or `"latest"`: Any version (not recommended)

#### 3. node_modules Directory

Where npm installs all dependencies.

```
my-project/
├── package.json
├── package-lock.json
├── node_modules/          # All installed packages (do NOT commit to Git)
│   ├── express/
│   ├── mocha/
│   └── ... (hundreds of folders)
└── src/
```

**Important**: Always add `node_modules/` to `.gitignore`

#### 4. package-lock.json

Locks exact versions of all dependencies (including transitive).

**Purpose**:
- Ensures consistent installs across teams
- Faster installs (knows exact versions)
- Security (prevents unexpected updates)

**Always commit** `package-lock.json` to Git.

#### 5. npm Registry

Default registry: https://registry.npmjs.org

Contains over 2 million packages.

#### 6. Global vs Local Installation

**Local Installation** (default):
```bash
npm install mocha
```
- Installed in `node_modules/`
- Available only in this project

**Global Installation**:
```bash
npm install -g mocha
```
- Installed system-wide
- Use for CLI tools you want everywhere

### Common npm Commands

#### Installation Commands
```bash
npm install                    # Install all dependencies from package.json
npm install express            # Install and add to dependencies
npm install --save express     # Same as above (--save is default)
npm install --save-dev mocha   # Install and add to devDependencies
npm install -g typescript      # Install globally
npm install express@4.17.1     # Install specific version
```

#### Uninstallation
```bash
npm uninstall express          # Remove package
npm uninstall -g typescript    # Remove global package
```

#### Updating
```bash
npm update                     # Update all packages (respecting semver)
npm update express             # Update specific package
npm outdated                   # List outdated packages
```

#### Information Commands
```bash
npm list                       # List installed packages (tree)
npm list --depth=0             # List top-level packages only
npm list -g                    # List global packages
npm view express               # View package info
npm view express versions      # View all available versions
npm search selenium            # Search npm registry
```

#### Other Useful Commands
```bash
npm init                       # Create new package.json (interactive)
npm init -y                    # Create package.json with defaults
npm run test                   # Run script from package.json
npm start                      # Run "start" script
npm test                       # Run "test" script
npm cache clean --force        # Clear npm cache
```

### npm Scripts

Custom commands defined in `package.json`.

```json
{
  "scripts": {
    "test": "mocha tests/**/*.js",
    "test:unit": "mocha tests/unit/**/*.js",
    "test:integration": "mocha tests/integration/**/*.js",
    "test:watch": "mocha tests/**/*.js --watch",
    "lint": "eslint src/**/*.js",
    "start": "node server.js",
    "build": "webpack --mode production"
  }
}
```

**Running scripts**:
```bash
npm run test
npm run test:unit
npm test          # Shortcut for "npm run test"
npm start         # Shortcut for "npm run start"
```

**Chaining scripts**:
```json
{
  "scripts": {
    "prebuild": "npm run lint",
    "build": "webpack",
    "postbuild": "npm test"
  }
}
```

Running `npm run build` automatically runs: prebuild → build → postbuild

### npm for Testers - Practical Use Cases

#### 1. Installing Test Frameworks
```bash
npm install --save-dev mocha chai
npm install --save-dev jest
npm install --save-dev playwright
```

#### 2. Running Tests
```json
{
  "scripts": {
    "test": "mocha",
    "test:chrome": "mocha --browser chrome",
    "test:firefox": "mocha --browser firefox",
    "test:ci": "mocha --reporter json > test-results.json"
  }
}
```

```bash
npm test
npm run test:chrome
```

#### 3. Managing Test Dependencies
```json
{
  "devDependencies": {
    "selenium-webdriver": "^4.1.0",
    "mocha": "^10.0.0",
    "chai": "^4.3.6",
    "@playwright/test": "^1.30.0"
  }
}
```

#### 4. Setting Up Test Environment
```bash
npm install            # Install all test dependencies
npm test               # Run test suite
```

### npx (npm Package Runner)

Executes packages without installing globally.

```bash
npx create-react-app my-app    # Run create-react-app without global install
npx mocha test.js              # Run local mocha
```

**Benefits**:
- No need for global installs
- Always uses latest version (unless specified)
- Keeps system clean

### npm vs yarn

**yarn** is an alternative to npm.

**Similarities**:
- Both manage Node.js packages
- Both use package.json
- Similar commands

**Key Differences**:
| Feature | npm | yarn |
|---------|-----|------|
| Lock file | package-lock.json | yarn.lock |
| Install all | npm install | yarn or yarn install |
| Add package | npm install pkg | yarn add pkg |
| Speed | Slower | Faster (parallel) |
| Offline mode | Limited | Built-in |

Most concepts in this guide apply to both npm and yarn.

---

## Part 4: Build Tools in Testing Context

### How Testers Use Build Tools

#### 1. Running Automated Tests

**Maven**:
```bash
mvn test                                    # Run all tests
mvn test -Dtest=SmokeTests                 # Run specific suite
mvn test -Dbrowser=chrome -Denv=staging    # Pass parameters
```

**npm**:
```bash
npm test                           # Run all tests
npm run test:smoke                 # Run smoke tests
npm test -- --grep "login"         # Run tests matching pattern
```

#### 2. Setting Up Test Environment

**Maven** - Install all dependencies:
```bash
mvn clean install -DskipTests
```

**npm** - Install all dependencies:
```bash
npm install
```

#### 3. Generating Test Reports

**Maven with Surefire Plugin**:
- HTML reports in `target/surefire-reports/`
- Can integrate with reporting plugins for better visuals

**npm with Mocha/Jest**:
```json
{
  "scripts": {
    "test:report": "mocha --reporter mochawesome"
  }
}
```

#### 4. Continuous Integration (CI/CD)

Build tools are essential in CI pipelines:

**Jenkins with Maven**:
```groovy
stage('Test') {
    steps {
        sh 'mvn clean test'
    }
}
```

**Jenkins with npm**:
```groovy
stage('Test') {
    steps {
        sh 'npm install'
        sh 'npm test'
    }
}
```

#### 5. Dependency Management for Test Frameworks

**Maven - Selenium example**:
```xml
<dependency>
    <groupId>org.seleniumhq.selenium</groupId>
    <artifactId>selenium-java</artifactId>
    <version>4.8.0</version>
</dependency>
```

**npm - Playwright example**:
```bash
npm install --save-dev @playwright/test
```

### Common Build Tool Issues and Solutions

#### Maven Issues

**Issue 1: Dependency not found**
```
Could not resolve dependencies for project...
```
**Solution**:
```bash
mvn dependency:purge-local-repository
mvn clean install
```

**Issue 2: Tests failing due to compilation errors**
**Solution**: Check Java version matches project requirements
```bash
mvn -version
```

**Issue 3: Slow builds**
**Solution**: Skip tests when not needed
```bash
mvn clean install -DskipTests
```

#### npm Issues

**Issue 1: Module not found**
```
Error: Cannot find module 'express'
```
**Solution**:
```bash
rm -rf node_modules package-lock.json
npm install
```

**Issue 2: Permission denied (on Mac/Linux)**
**Solution**: Don't use sudo, fix npm permissions
```bash
npm config set prefix ~/.npm-global
```

**Issue 3: Outdated packages**
**Solution**:
```bash
npm outdated
npm update
```

---

## Part 5: Best Practices

### Maven Best Practices

1. **Keep pom.xml clean**: Remove unused dependencies
2. **Use properties**: Define versions once, reference everywhere
3. **Lock versions**: Don't use LATEST or RELEASE
4. **Use dependency management**: For multi-module projects
5. **Run clean regularly**: `mvn clean` before important builds
6. **Check for vulnerabilities**: Use OWASP dependency-check plugin

### npm Best Practices

1. **Commit package-lock.json**: Ensures consistent installs
2. **Use exact versions for critical dependencies**: Remove ^ or ~ if stability is critical
3. **Audit security**: Run `npm audit` regularly
4. **Keep dependencies updated**: Run `npm outdated` periodically
5. **Use .npmrc for configuration**: Team-wide npm settings
6. **Separate dev and production deps**: Use `--save-dev` correctly
7. **Don't commit node_modules**: Always in .gitignore

### General Best Practices for Testers

1. **Understand the build process**: Don't just run commands blindly
2. **Read build logs**: Errors often appear in build output
3. **Keep local environment clean**: Clear caches when issues arise
4. **Document setup steps**: Help team members set up quickly
5. **Use build tool for test execution**: Consistent with CI/CD
6. **Version control build files**: pom.xml and package.json must be in Git

---

## Part 6: Comparison Summary

| Aspect | Maven | npm |
|--------|-------|-----|
| **Language** | Java | JavaScript/Node.js |
| **Config File** | pom.xml (XML) | package.json (JSON) |
| **Lock File** | - (Maven uses exact versions) | package-lock.json |
| **Local Repo** | ~/.m2/repository | node_modules/ |
| **Central Repo** | Maven Central | npm Registry |
| **Run Tests** | mvn test | npm test |
| **Install Deps** | mvn install | npm install |
| **Scope Types** | compile, test, provided | dependencies, devDependencies |
| **Global Install** | N/A | npm install -g |
| **Scripts** | Maven plugins | package.json scripts |

---

## Part 7: Interview-Ready Summary

### Must-Know Maven Points

1. pom.xml structure (groupId, artifactId, version)
2. Build lifecycle phases (compile, test, package, install)
3. Dependency scopes (compile, test, provided)
4. Local repository location (~/.m2/repository)
5. Common commands (mvn clean, mvn test, mvn install)
6. How to run specific tests
7. Surefire plugin for unit tests
8. Maven coordinates (GAV)

### Must-Know npm Points

1. package.json structure
2. dependencies vs devDependencies
3. Semantic versioning (^, ~, exact)
4. node_modules and package-lock.json
5. Common commands (npm install, npm test, npm update)
6. npm scripts for test execution
7. Local vs global installation
8. npx for running packages

### Key Differences to Remember

- Maven is convention-based (standard directory structure)
- npm is more flexible (you define structure)
- Maven has built-in lifecycle, npm uses scripts
- Maven for Java, npm for JavaScript
- Both manage dependencies automatically
- Both integrate with CI/CD tools

---

## Conclusion

Understanding build tools is essential for modern testers:
- **Maven** for Java projects: Manage dependencies, compile code, run tests
- **npm** for JavaScript projects: Manage packages, run scripts, execute tests
- Both are integral to CI/CD pipelines
- Knowing these tools improves collaboration with developers and enables better test automation

Practice with hands-on exercises to solidify these concepts!
