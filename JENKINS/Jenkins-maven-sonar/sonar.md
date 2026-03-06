#### 1. What is SonarQube?
SonarQube is an open-source platform used for continuous inspection of code quality.

It detects:

* Bugs
* Code smells
* Security vulnerabilities
* Duplications
* Code coverage (if integrated with testing tools)

#### 2. What are code smells in SonarQube?
Code smells are maintainability issues in code. They don’t affect functionality but make the code harder to maintain.

#### 3. What is a quality gate?
A quality gate is a set of rules and thresholds (e.g., code coverage ≥ 80%, no critical issues).
It gives a pass/fail result after analysis.

#### 4. What are the types of issues SonarQube detects?

* Bugs – Logic errors or defects
* Vulnerabilities – Security issues
* Code smells – Maintainability issues
* Duplications – Repeated code
* Coverage – Unit test coverage


#### 5. How does SonarQube integrate into a CI/CD pipeline?

* Add SonarQube scanner to Jenkins/GitLab CI/GitHub Actions
* Run scanner during build or test stage
* Example for Maven:

```bash
mvn sonar:sonar -Dsonar.projectKey=myapp -Dsonar.host.url=http://localhost:9000 -Dsonar.login=your_token
```

#### 6. What is a SonarQube scanner?
It's a command-line tool or plugin (e.g., for Maven/Gradle) that sends code analysis data to the SonarQube server.

#### 7. What languages does SonarQube support?
Supports 25+ languages: Java, Python, JavaScript, C#, Go, Kotlin, etc.


#### 8. What are rules and rule sets?

* Rules define what SonarQube checks for (e.g., "Avoid hard-coded passwords").
* Rules are grouped into Quality Profiles.

#### 9. What is a quality profile?
A collection of rules for a specific language.
Each project can use a different profile per language.

#### 10. What metrics does SonarQube use to judge code quality?

* Bugs
* Vulnerabilities
* Code smells
* Coverage
* Duplications
* Cyclomatic complexity
* Lines of code


#### 11. How do you handle false positives in SonarQube?

* Use `//NOSONAR` comment to suppress a line
* Mark issue as “Won’t Fix” or “False Positive” via UI

#### 12. What is technical debt in SonarQube?
Time required to fix all maintainability issues (code smells).
Shown as `x days/hours of work`.

#### 13. How do you analyze code from a Git repo using SonarQube?

* Clone repo in CI/CD
* Build project
* Run SonarQube scanner
* Example in Jenkins:

```bash
withSonarQubeEnv('SonarQube') {
  sh 'mvn clean verify sonar:sonar'
}
```

#### 14. What is the difference between SonarQube and SonarCloud?

* SonarQube: Self-hosted
* SonarCloud: SaaS/cloud version, hosted by SonarSource
  (free for public repos)


#### 15. How do you manage user access in SonarQube?

* Use built-in roles (Admin, User)
* Integrate with LDAP/SAML
* Configure project permissions (Browse, Admin, Execute Analysis)

#### 16. How do you handle PR analysis in SonarQube?

* Use branch or pull request analysis feature
* Requires Developer Edition+
* Shows new issues introduced by the PR


#### 17. How to trigger SonarQube analysis manually?

```bash
sonar-scanner \
  -Dsonar.projectKey=MyApp \
  -Dsonar.sources=. \
  -Dsonar.host.url=http://localhost:9000 \
  -Dsonar.login=your_token
```
