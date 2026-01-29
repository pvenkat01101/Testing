# Phase 3 - Build Tools: Practical Exercises (Maven & npm)

## Part A: Maven Practical Exercises

### Exercise 1: Create Your First Maven Project

#### Objective
Create a basic Maven project from scratch.

#### Pre-requisites
- Java JDK installed (Java 8 or higher)
- Maven installed (verify with `mvn -version`)

#### Step-by-step (Beginner)
1. Open terminal/command prompt
2. Create project using Maven archetype:
   ```bash
   mvn archetype:generate -DgroupId=com.testing.demo -DartifactId=my-first-maven-project -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
   ```
3. Navigate into project: `cd my-first-maven-project`
4. View generated structure: `ls -R` (Mac/Linux) or `tree` (Windows)
5. Open `pom.xml` in a text editor
6. Review the basic structure
7. Build the project: `mvn compile`
8. Check `target/classes` folder for compiled output

**Deliverable**: Working Maven project with standard structure

---

### Exercise 2: Understanding pom.xml

#### Objective
Read and understand a pom.xml file.

#### Step-by-step (Beginner)
1. Open `pom.xml` from Exercise 1
2. Identify these elements:
   - modelVersion
   - groupId
   - artifactId
   - version
   - packaging
   - dependencies section
3. Note the JUnit dependency (if present)
4. Check the dependency scope (should be "test")
5. Add a comment above each major section explaining its purpose
6. Save the file

**Deliverable**: Documented pom.xml with comments

---

### Exercise 3: Add Dependencies to pom.xml

#### Objective
Add external libraries to your project.

#### Step-by-step (Beginner)
1. Open `pom.xml`
2. Locate the `<dependencies>` section
3. Add Selenium WebDriver dependency:
   ```xml
   <dependency>
       <groupId>org.seleniumhq.selenium</groupId>
       <artifactId>selenium-java</artifactId>
       <version>4.8.0</version>
   </dependency>
   ```
4. Add TestNG dependency:
   ```xml
   <dependency>
       <groupId>org.testng</groupId>
       <artifactId>testng</artifactId>
       <version>7.7.1</version>
       <scope>test</scope>
   </dependency>
   ```
5. Save pom.xml
6. Run: `mvn clean install`
7. Wait for Maven to download dependencies
8. Check `~/.m2/repository` to see downloaded JARs

**Deliverable**: pom.xml with Selenium and TestNG dependencies

---

### Exercise 4: Maven Build Lifecycle

#### Objective
Execute different Maven lifecycle phases.

#### Step-by-step (Beginner)
1. In your Maven project, run each command and observe output:
   ```bash
   mvn validate      # Validate project structure
   mvn compile       # Compile source code
   mvn test          # Run tests
   mvn package       # Create JAR file
   mvn verify        # Run integration tests
   mvn install       # Install to local repository
   mvn clean         # Delete target folder
   ```
2. After `mvn package`, check `target/` folder for JAR file
3. After `mvn clean`, verify `target/` is deleted
4. Run combined command: `mvn clean install`
5. Observe that all phases run in sequence
6. Document the output of each command in a text file

**Deliverable**: Understanding of lifecycle phases

---

### Exercise 5: Run Tests with Maven

#### Objective
Execute unit tests using Maven.

#### Step-by-step (Beginner)
1. Navigate to `src/test/java/com/testing/demo/`
2. Create a simple test class `CalculatorTest.java`:
   ```java
   package com.testing.demo;

   import org.testng.Assert;
   import org.testng.annotations.Test;

   public class CalculatorTest {

       @Test
       public void testAddition() {
           int result = 2 + 3;
           Assert.assertEquals(result, 5);
       }

       @Test
       public void testSubtraction() {
           int result = 5 - 2;
           Assert.assertEquals(result, 3);
       }
   }
   ```
3. Run tests: `mvn test`
4. Check output in terminal (should show 2 tests passed)
5. Open `target/surefire-reports/` folder
6. View HTML and XML reports
7. Document test results

**Deliverable**: Test execution and reports

---

### Exercise 6: Run Specific Tests

#### Objective
Execute individual test classes or methods.

#### Step-by-step (Beginner)
1. Create another test class `LoginTest.java`:
   ```java
   package com.testing.demo;

   import org.testng.Assert;
   import org.testng.annotations.Test;

   public class LoginTest {

       @Test
       public void testValidLogin() {
           Assert.assertTrue(true);
       }

       @Test
       public void testInvalidLogin() {
           Assert.assertTrue(true);
       }
   }
   ```
2. Run all tests: `mvn test` (should run 4 tests total)
3. Run only CalculatorTest: `mvn test -Dtest=CalculatorTest`
4. Run only specific method: `mvn test -Dtest=CalculatorTest#testAddition`
5. Run multiple classes: `mvn test -Dtest=CalculatorTest,LoginTest`
6. Document the command outputs

**Deliverable**: Understanding of selective test execution

---

### Exercise 7: Skip Tests

#### Objective
Learn how to skip test execution when needed.

#### Step-by-step (Beginner)
1. Run build with tests: `mvn clean install` (note duration)
2. Skip tests during compilation: `mvn clean install -DskipTests`
   - Tests are compiled but not run
3. Skip tests completely: `mvn clean install -Dmaven.test.skip=true`
   - Tests are not compiled or run
4. Compare execution times
5. Document when you might use each approach

**Deliverable**: Understanding of test skipping options

---

### Exercise 8: Maven Properties

#### Objective
Use properties to manage versions centrally.

#### Step-by-step (Beginner)
1. Open `pom.xml`
2. Add `<properties>` section after `<packaging>`:
   ```xml
   <properties>
       <maven.compiler.source>11</maven.compiler.source>
       <maven.compiler.target>11</maven.compiler.target>
       <selenium.version>4.8.0</selenium.version>
       <testng.version>7.7.1</testng.version>
   </properties>
   ```
3. Update dependencies to use properties:
   ```xml
   <dependency>
       <groupId>org.seleniumhq.selenium</groupId>
       <artifactId>selenium-java</artifactId>
       <version>${selenium.version}</version>
   </dependency>
   <dependency>
       <groupId>org.testng</groupId>
       <artifactId>testng</artifactId>
       <version>${testng.version}</version>
       <scope>test</scope>
   </dependency>
   ```
4. Save and run: `mvn clean compile`
5. Verify build succeeds
6. Change Selenium version in properties to 4.9.0
7. Run: `mvn clean install -U` (force update)

**Deliverable**: pom.xml with properties

---

### Exercise 9: Dependency Tree

#### Objective
View and understand transitive dependencies.

#### Step-by-step (Beginner)
1. Run: `mvn dependency:tree`
2. Observe the hierarchy of dependencies
3. Note that Selenium brings many transitive dependencies
4. Save output to file: `mvn dependency:tree > dependencies.txt`
5. Open the file and identify:
   - Direct dependencies (your pom.xml)
   - Transitive dependencies (pulled by direct ones)
6. Count how many total JARs are downloaded for just one Selenium dependency

**Deliverable**: Understanding of dependency tree

---

### Exercise 10: Configure Surefire Plugin

#### Objective
Customize test execution with Surefire plugin.

#### Step-by-step (Beginner)
1. Open `pom.xml`
2. Add `<build>` section with Surefire configuration:
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
                   <testFailureIgnore>false</testFailureIgnore>
               </configuration>
           </plugin>
       </plugins>
   </build>
   ```
3. Save and run: `mvn test`
4. Modify to run only tests ending with "Test": already configured
5. Try parallel execution by adding:
   ```xml
   <parallel>methods</parallel>
   <threadCount>2</threadCount>
   ```
6. Run tests and observe execution

**Deliverable**: Configured Surefire plugin

---

### Exercise 11: Maven Profiles

#### Objective
Create profiles for different environments.

#### Step-by-step (Beginner)
1. Open `pom.xml`
2. Add `<profiles>` section:
   ```xml
   <profiles>
       <profile>
           <id>dev</id>
           <properties>
               <environment>development</environment>
           </properties>
       </profile>
       <profile>
           <id>staging</id>
           <properties>
               <environment>staging</environment>
           </properties>
       </profile>
       <profile>
           <id>prod</id>
           <properties>
               <environment>production</environment>
           </properties>
       </profile>
   </profiles>
   ```
3. In test code, access property:
   ```java
   String env = System.getProperty("environment", "dev");
   System.out.println("Running on: " + env);
   ```
4. Run with profile: `mvn test -Pstaging`
5. Check output to see "Running on: staging"
6. Try different profiles

**Deliverable**: Profile-based configuration

---

### Exercise 12: Local Repository Management

#### Objective
Understand and manage Maven local repository.

#### Step-by-step (Beginner)
1. Find your local repository:
   - Mac/Linux: `~/.m2/repository`
   - Windows: `C:\Users\YourName\.m2\repository`
2. Navigate to it and explore folders
3. Find Selenium JAR: `org/seleniumhq/selenium/selenium-java/`
4. View the size of the repository: `du -sh ~/.m2/repository`
5. Clear a specific dependency: `mvn dependency:purge-local-repository -DmanualInclude="org.seleniumhq.selenium:selenium-java"`
6. Re-run: `mvn clean install` (it will re-download)
7. Document the location and size

**Deliverable**: Understanding of local repository

---

### Exercise 13: Resolve Dependency Conflicts

#### Objective
Identify and resolve dependency conflicts.

#### Step-by-step (Beginner)
1. Add two dependencies that conflict:
   ```xml
   <dependency>
       <groupId>org.apache.logging.log4j</groupId>
       <artifactId>log4j-core</artifactId>
       <version>2.19.0</version>
   </dependency>
   <dependency>
       <groupId>org.apache.logging.log4j</groupId>
       <artifactId>log4j-api</artifactId>
       <version>2.17.0</version>
   </dependency>
   ```
2. Run: `mvn dependency:tree` and look for conflicts
3. Maven chooses nearest version, but you can force:
   ```xml
   <dependencyManagement>
       <dependencies>
           <dependency>
               <groupId>org.apache.logging.log4j</groupId>
               <artifactId>log4j-api</artifactId>
               <version>2.19.0</version>
           </dependency>
       </dependencies>
   </dependencyManagement>
   ```
4. Run: `mvn clean install`
5. Verify versions are consistent

**Deliverable**: Understanding of conflict resolution

---

### Exercise 14: Maven with Selenium Test

#### Objective
Write and execute a real Selenium test with Maven.

#### Step-by-step (Beginner)
1. Ensure Selenium and TestNG dependencies are in pom.xml
2. Create test class `GoogleSearchTest.java`:
   ```java
   package com.testing.demo;

   import org.openqa.selenium.WebDriver;
   import org.openqa.selenium.chrome.ChromeDriver;
   import org.testng.Assert;
   import org.testng.annotations.AfterMethod;
   import org.testng.annotations.BeforeMethod;
   import org.testng.annotations.Test;

   public class GoogleSearchTest {
       WebDriver driver;

       @BeforeMethod
       public void setup() {
           driver = new ChromeDriver();
       }

       @Test
       public void testGoogleTitle() {
           driver.get("https://www.google.com");
           String title = driver.getTitle();
           Assert.assertTrue(title.contains("Google"));
       }

       @AfterMethod
       public void teardown() {
           if(driver != null) {
               driver.quit();
           }
       }
   }
   ```
3. Ensure ChromeDriver is in PATH or use WebDriverManager
4. Run: `mvn test -Dtest=GoogleSearchTest`
5. Check test reports in `target/surefire-reports/`

**Deliverable**: Selenium test executed via Maven

---

### Exercise 15: Maven Clean Room Build

#### Objective
Ensure repeatable builds by starting fresh.

#### Step-by-step (Beginner)
1. Run: `mvn clean` (delete target folder)
2. Delete local Maven repository specific to your project:
   ```bash
   rm -rf ~/.m2/repository/com/testing/demo
   ```
3. Run full build: `mvn clean install`
4. Maven downloads all dependencies fresh
5. All tests run from scratch
6. Verify build succeeds
7. This simulates a CI/CD environment (fresh every time)

**Deliverable**: Understanding of clean builds

---

## Part B: npm Practical Exercises

### Exercise 16: Initialize an npm Project

#### Objective
Create a new npm project from scratch.

#### Pre-requisites
- Node.js installed (verify with `node -v`)
- npm installed (verify with `npm -v`)

#### Step-by-step (Beginner)
1. Create new folder: `mkdir my-test-project`
2. Navigate: `cd my-test-project`
3. Initialize npm: `npm init -y` (creates package.json with defaults)
4. Open `package.json` in text editor
5. Modify fields:
   - name: "my-test-project"
   - version: "1.0.0"
   - description: "Test automation project"
6. Save the file

**Deliverable**: Initialized npm project with package.json

---

### Exercise 17: Install Dependencies

#### Objective
Add external packages to your project.

#### Step-by-step (Beginner)
1. Install Mocha (test framework): `npm install --save-dev mocha`
2. Install Chai (assertion library): `npm install --save-dev chai`
3. Open `package.json` and see added devDependencies
4. Check `node_modules/` folder (hundreds of folders created)
5. Check `package-lock.json` (auto-generated, locks versions)
6. Install a production dependency: `npm install axios`
7. See axios in dependencies (not devDependencies)

**Deliverable**: package.json with dependencies

---

### Exercise 18: Understanding package.json

#### Objective
Read and modify package.json.

#### Step-by-step (Beginner)
1. Open `package.json`
2. Identify sections:
   - name, version, description
   - scripts
   - dependencies
   - devDependencies
3. Add a script:
   ```json
   "scripts": {
       "test": "mocha",
       "test:unit": "mocha tests/unit/**/*.js"
   }
   ```
4. Save file
5. Run: `npm test` (it will look for tests folder)
6. Document each section's purpose

**Deliverable**: Annotated package.json

---

### Exercise 19: Create and Run Tests with Mocha

#### Objective
Write tests and execute with npm.

#### Step-by-step (Beginner)
1. Create folder: `mkdir tests`
2. Create file: `tests/calculator.test.js`
3. Add test code:
   ```javascript
   const assert = require('chai').assert;

   describe('Calculator Tests', function() {
       it('should add two numbers', function() {
           const result = 2 + 3;
           assert.equal(result, 5);
       });

       it('should subtract two numbers', function() {
           const result = 5 - 2;
           assert.equal(result, 3);
       });
   });
   ```
4. Run tests: `npm test`
5. Should see 2 passing tests
6. Create another test file: `tests/string.test.js`
   ```javascript
   const assert = require('chai').assert;

   describe('String Tests', function() {
       it('should concatenate strings', function() {
           const result = 'Hello' + ' ' + 'World';
           assert.equal(result, 'Hello World');
       });
   });
   ```
7. Run: `npm test` (all tests run)

**Deliverable**: Working tests with Mocha and Chai

---

### Exercise 20: npm Scripts

#### Objective
Create custom npm scripts for different test scenarios.

#### Step-by-step (Beginner)
1. Open `package.json`
2. Update scripts section:
   ```json
   "scripts": {
       "test": "mocha tests/**/*.test.js",
       "test:calculator": "mocha tests/calculator.test.js",
       "test:string": "mocha tests/string.test.js",
       "test:watch": "mocha tests/**/*.test.js --watch",
       "lint": "echo 'Linting code...'"
   }
   ```
3. Run different scripts:
   ```bash
   npm test                    # All tests
   npm run test:calculator     # Only calculator tests
   npm run test:string         # Only string tests
   npm run lint                # Custom script
   ```
4. Document what each script does

**Deliverable**: Multiple npm scripts configured

---

### Exercise 21: Semantic Versioning

#### Objective
Understand version ranges in package.json.

#### Step-by-step (Beginner)
1. Open `package.json`
2. Note the version format for dependencies (e.g., `"^8.0.0"`)
3. Understand symbols:
   - `^8.0.0` = >=8.0.0, <9.0.0 (compatible with 8.x.x)
   - `~8.0.0` = >=8.0.0, <8.1.0 (approximately equivalent)
   - `8.0.0` = exactly 8.0.0
4. Modify mocha version to exact: `"mocha": "10.2.0"`
5. Modify chai to range: `"chai": "^4.3.0"`
6. Run: `npm install`
7. Check `package-lock.json` for exact installed versions

**Deliverable**: Understanding of version ranges

---

### Exercise 22: Update Dependencies

#### Objective
Check for and update outdated packages.

#### Step-by-step (Beginner)
1. Check outdated packages: `npm outdated`
2. Output shows current vs wanted vs latest
3. Update specific package: `npm update mocha`
4. Update all packages: `npm update`
5. For major version updates: `npm install mocha@latest`
6. Run tests to ensure nothing breaks: `npm test`
7. Document version changes

**Deliverable**: Updated dependencies

---

### Exercise 23: Global vs Local Installation

#### Objective
Understand difference between global and local packages.

#### Step-by-step (Beginner)
1. Install Mocha locally (already done): `npm install --save-dev mocha`
2. Try running: `mocha` (command not found)
3. Run with npx: `npx mocha` (works!)
4. Install globally: `npm install -g mocha`
5. Now run: `mocha` (works directly)
6. List global packages: `npm list -g --depth=0`
7. Uninstall global: `npm uninstall -g mocha`
8. Note: Prefer local installation for projects

**Deliverable**: Understanding of installation scopes

---

### Exercise 24: Using npx

#### Objective
Run packages without installing globally.

#### Step-by-step (Beginner)
1. Without installing globally, run: `npx mocha tests/calculator.test.js`
2. Works! npx uses local node_modules
3. Run a package you don't have: `npx cowsay "Hello Testing"`
4. npx downloads temporarily and runs it
5. Try: `npx create-react-app my-app` (creates React app without global install)
6. Cancel after it starts (Ctrl+C)
7. Document npx benefits

**Deliverable**: Understanding of npx

---

### Exercise 25: package-lock.json

#### Objective
Understand the role of package-lock.json.

#### Step-by-step (Beginner)
1. Open `package-lock.json`
2. Note it contains exact versions of all packages (including transitive)
3. Delete it: `rm package-lock.json`
4. Delete node_modules: `rm -rf node_modules`
5. Reinstall: `npm install`
6. package-lock.json is recreated
7. Compare with previous version (versions might differ slightly)
8. Commit package-lock.json to Git (important for team consistency)

**Deliverable**: Understanding of lock file

---

### Exercise 26: Uninstall Dependencies

#### Objective
Remove packages properly.

#### Step-by-step (Beginner)
1. List installed packages: `npm list --depth=0`
2. Uninstall axios: `npm uninstall axios`
3. Check package.json (axios removed from dependencies)
4. Check node_modules (axios folder gone)
5. Reinstall: `npm install axios`
6. Uninstall with shortcut: `npm un axios`
7. Verify removal in package.json

**Deliverable**: Understanding of package removal

---

### Exercise 27: npm Audit (Security)

#### Objective
Check for security vulnerabilities.

#### Step-by-step (Beginner)
1. Run security audit: `npm audit`
2. Review output (shows vulnerabilities if any)
3. See recommended fixes
4. Fix automatically (if possible): `npm audit fix`
5. Force fix (may break things): `npm audit fix --force`
6. Run audit again: `npm audit`
7. Document any remaining vulnerabilities

**Deliverable**: Security audit report

---

### Exercise 28: .gitignore for Node Projects

#### Objective
Properly exclude node_modules from Git.

#### Step-by-step (Beginner)
1. Initialize Git: `git init`
2. Create `.gitignore` file:
   ```
   node_modules/
   package-lock.json
   .env
   *.log
   dist/
   ```
3. Check status: `git status`
4. node_modules should not appear
5. Add and commit:
   ```bash
   git add .
   git commit -m "Initial commit"
   ```
6. Verify node_modules not in repo

**Deliverable**: Proper .gitignore for Node project

---

### Exercise 29: Install from package.json

#### Objective
Set up project on a new machine.

#### Scenario
Simulate cloning a project without node_modules.

#### Step-by-step (Beginner)
1. Delete node_modules: `rm -rf node_modules`
2. Delete package-lock.json: `rm package-lock.json`
3. Now you only have package.json (simulating fresh clone)
4. Install all dependencies: `npm install`
5. node_modules is recreated
6. package-lock.json is recreated
7. Run tests: `npm test` (everything works!)
8. This is what happens in CI/CD and when teammates clone your repo

**Deliverable**: Understanding of fresh installs

---

### Exercise 30: Playwright Test with npm

#### Objective
Set up and run Playwright tests using npm.

#### Step-by-step (Beginner)
1. Create new project folder: `mkdir playwright-test && cd playwright-test`
2. Initialize: `npm init -y`
3. Install Playwright: `npm install --save-dev @playwright/test`
4. Install browsers: `npx playwright install`
5. Create test file: `tests/example.spec.js`
   ```javascript
   const { test, expect } = require('@playwright/test');

   test('Google search page', async ({ page }) => {
       await page.goto('https://www.google.com');
       const title = await page.title();
       expect(title).toContain('Google');
   });
   ```
6. Update package.json scripts:
   ```json
   "scripts": {
       "test": "playwright test"
   }
   ```
7. Run tests: `npm test`
8. View report: `npx playwright show-report`

**Deliverable**: Working Playwright test with npm

---

## Part C: Comparison Exercises

### Exercise 31: Maven vs npm - Side by Side

#### Objective
Compare equivalent operations in Maven and npm.

#### Step-by-step (Beginner)
1. Create a comparison table:

| Task | Maven Command | npm Command |
|------|---------------|-------------|
| Initialize project | `mvn archetype:generate` | `npm init` |
| Install dependencies | `mvn install` | `npm install` |
| Run tests | `mvn test` | `npm test` |
| Clean build | `mvn clean` | `rm -rf node_modules` |
| Add dependency | Edit pom.xml | `npm install pkg` |
| View dependencies | `mvn dependency:tree` | `npm list` |
| Update dependencies | `mvn -U` | `npm update` |

2. Document similarities and differences

**Deliverable**: Comparison table

---

### Exercise 32: CI/CD Simulation

#### Objective
Simulate a CI/CD pipeline using build tools.

#### Maven Version:
```bash
# Fresh start
mvn clean
# Download dependencies
mvn install -DskipTests
# Run tests
mvn test
# Package application
mvn package
```

#### npm Version:
```bash
# Fresh start
rm -rf node_modules
# Download dependencies
npm install
# Run tests
npm test
# Build application (if applicable)
npm run build
```

#### Step-by-step (Beginner)
1. Create a shell script for Maven: `maven-ci.sh`
2. Create a shell script for npm: `npm-ci.sh`
3. Make executable: `chmod +x *.sh`
4. Run each script
5. Document output and duration

**Deliverable**: CI/CD simulation scripts

---

## Conclusion

These practical exercises cover:
- **Maven**: Project creation, dependency management, test execution, plugins, profiles
- **npm**: Project initialization, package management, npm scripts, testing frameworks
- **Both**: Understanding build lifecycles, dependency resolution, CI/CD integration

Practice these exercises multiple times to build muscle memory. Build tools are fundamental skills for modern testers working with automation frameworks!
