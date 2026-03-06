###  **Basic Maven Questions**

**1. What is Maven?**
Maven is a **build automation tool** used primarily for Java projects. It manages **dependencies**, builds, testing, and **project lifecycle**.

**2. What are the main features of Maven?**

* Dependency management
* Standard project structure
* Build lifecycle and plugins
* Repository management
* Easy integration with CI/CD tools

**3. What is the `pom.xml` file?**
`pom.xml` (Project Object Model) is the **core configuration file** in Maven. It defines:

* Project info
* Dependencies
* Plugins
* Build profiles
* Repositories

**4. What is the Maven repository?**
A storage location for Maven artifacts:

* **Local** – `.m2` directory on your machine
* **Central** – Maven Central repository (default remote repo)
* **Remote** – Custom repositories (e.g., Nexus, Artifactory)


###  **Dependency Management**

**5. What are the types of Maven dependencies?**

* `compile` (default) – Needed at compile and runtime
* `provided` – Required for compile but provided at runtime (e.g., servlet API)
* `runtime` – Only required during runtime
* `test` – Only for testing
* `system` – Provided explicitly via system path

**6. What is a transitive dependency?**
If Project A depends on B, and B depends on C, then A also gets C.
Maven **resolves these automatically**.

**7. How do you exclude a transitive dependency?**

```xml
<dependency>
  <groupId>org.example</groupId>
  <artifactId>example-lib</artifactId>
  <exclusions>
    <exclusion>
      <groupId>org.unwanted</groupId>
      <artifactId>unwanted-lib</artifactId>
    </exclusion>
  </exclusions>
</dependency>
```


###  **Build & Lifecycle**

**8. What is Maven's build lifecycle?**
Maven has **three built-in lifecycles**:

* `default` – Build and deploy code
* `clean` – Clean up artifacts
* `site` – Generate project documentation

**9. Common phases in the default lifecycle?**

* `validate`
* `compile`
* `test`
* `package`
* `verify`
* `install`
* `deploy`

**10. What does `mvn clean install` do?**

* `clean`: Deletes `target/` directory
* `install`: Compiles, tests, and installs the built artifact into the local repository


###  **Plugins & Goals**

**11. What are Maven plugins?**
Plugins provide **goal-based tasks** like compiling code, running tests, packaging JARs, etc.

Example:

* `maven-compiler-plugin`: for compiling
* `maven-surefire-plugin`: for testing
* `maven-jar-plugin`: to build a JAR

**12. How do you specify the Java version in Maven?**

```xml
<properties>
  <maven.compiler.source>1.8</maven.compiler.source>
  <maven.compiler.target>1.8</maven.compiler.target>
</properties>
```


###  **Profiles and Customization**

**13. What is a Maven profile?**
Profiles allow you to define **custom build configurations** for different environments (dev, test, prod).

```xml
<profiles>
  <profile>
    <id>dev</id>
    <properties>
      <env.name>dev</env.name>
    </properties>
  </profile>
</profiles>
```

Run with:

```bash
mvn clean install -Pdev
```

**14. How do you skip tests in Maven?**

```bash
mvn install -DskipTests   # Skips test execution, compiles test classes  
mvn install -Dmaven.test.skip=true   # Skips test compile and execution
```


###  **Advanced / Real-World Use**

**15. How do you integrate Maven with Jenkins?**

* Use Maven in Jenkins pipeline or freestyle job
* Define goals: `clean install`
* Configure `pom.xml` in SCM
* Use `Maven` tool from Jenkins global config

**16. What is the difference between `install` and `deploy` in Maven?**

* `install`: Installs artifact to the **local** repo
* `deploy`: Pushes artifact to a **remote** repo (Nexus, Artifactory)

**17. What is the `target/` folder?**
The output folder where Maven stores compiled code, test reports, and built artifacts (JAR, WAR).
