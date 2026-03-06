##  CI/CD Pipeline Flow (Interview-Ready Explanation)

### 1️ **Trigger the Pipeline (via Git Webhooks)**

* The CI/CD process starts when an event occurs — usually a **code push, pull request, or merge**.
* A **Git webhook** notifies Jenkins (or other CI tools) of the change in the repository (GitHub/GitLab/Bitbucket).
* This ensures the pipeline runs **automatically**, reducing manual intervention and delays.

> *Example*: Developer pushes code to `main` → webhook triggers Jenkins pipeline.



### 2️ **Checkout the Code**

* Jenkins uses a **Git plugin** to **pull the latest code** from the repository.
* This code is downloaded onto the **agent node** (executor) where the pipeline stages will run.
* Ensures that all stages (build, test, deploy) use the **most recent code version**.



### 3️ **Build the Project**

* Jenkins compiles or builds the project using relevant tools:

  * **Maven/Gradle** (for Java)
  * **npm/yarn** (for Node.js)
  * **go build** (for Golang)
* This stage ensures the application is packaged and ready for deployment.

> *Example*: Run `mvn clean package` to generate `.jar` or `.war` file.



### 4️ **Test the Code**

* Automated tests are executed to **verify code quality and functionality**.
* Types of tests:

  * **Unit Tests**
  * **Integration Tests**
  * **Smoke Tests**
* If any test fails, Jenkins halts the pipeline and alerts the team.

> *Tools*: JUnit, TestNG, PyTest, etc.



### 5️ **Code Quality Scan using SonarQube**

* SonarQube performs **static code analysis** to detect:

  * Bugs
  * Code smells
  * Security vulnerabilities
* Jenkins integrates with SonarQube using plugins or CLI scanners.

> *Command*: `sonar-scanner -Dsonar.projectKey=...`



### 6️ **Build Docker Image**

* Jenkins uses a **Dockerfile** to package the app and its dependencies into an image.
* This image is portable and consistent across environments.

> *Command*: `docker build -t myapp:latest .`



### 7️ **Docker Image Scan using Trivy**

* Trivy scans the Docker image for:

  * OS package vulnerabilities
  * CVEs in base image layers
* Jenkins can fail the pipeline if **critical vulnerabilities** are found.

> *Command*: `trivy image myapp:latest`



### 8️ **Push Docker Image to Registry**

* If the scan passes, Jenkins pushes the Docker image to:

  * **Docker Hub**
  * **Amazon ECR**
  * **GitHub Container Registry**
* Ensures the image is available for deployment in later stages.

> *Command*: `docker push myapp:latest`



## 🚀 CD: Continuous Deployment Phase

### 9️ **Deploy the Application**

There are **multiple approaches** to deploy your app depending on your infrastructure:



####  **Option 1: Jenkins-Based Deployment**

* Jenkins connects via SSH or uses plugins to deploy directly to:

  * **VMs**
  * **Kubernetes**
  * **AWS EC2/ECS/EKS**
* Tools like `kubectl`, `ansible-playbook`, or `scp` are used.

> *Example*: `kubectl apply -f deployment.yaml`



####  **Option 2: GitOps with Argo CD**

* Argo CD continuously monitors a **Git repo** that holds the **Kubernetes YAML/Helm charts**.
* Whenever Jenkins updates the image tag in Git, Argo CD **automatically syncs** and deploys it to the cluster.
* Benefits:

  * Declarative
  * Auditable
  * Self-healing

> *Example*: Jenkins updates `values.yaml` → Argo CD detects change → syncs → deploys.



####  **Option 3: Ansible Deployment**

* Ansible playbooks automate:

  * Package installation
  * App deployment
  * Configuration changes
* Jenkins executes `ansible-playbook` to deploy apps or configure infra.

> *Example*: `ansible-playbook deploy_app.yaml -i inventory`



### **Post-Deployment Actions (Optional)**

* Notify via:

  * **Slack / Microsoft Teams**
  * **Email alerts**
  * **Webhooks**
* Can also trigger:

  * Smoke test suite
  * Monitoring dashboard refresh
  * API health checks



###  **Monitor Pipeline Outcome**

* Jenkins tracks success/failure of each stage.
* If a stage fails:

  * Pipeline halts (based on configuration)
  * Logs are stored for debugging
  * Team is notified immediately
* Helps maintain **pipeline visibility and accountability**



##  Tools Involved at Each Stage

| Stage      | Tools                             |
| - |  |
| Trigger    | Git Webhooks                      |
| CI/CD      | Jenkins, GitHub Actions           |
| Build      | Maven, Gradle, npm                |
| Test       | JUnit, TestNG, PyTest             |
| Code Scan  | SonarQube                         |
| Docker     | Docker CLI, Docker Hub            |
| Image Scan | Trivy                             |
| Deployment | Argo CD, Ansible, kubectl, Helm   |
| Monitoring | Slack, Email, Grafana, Prometheus |




1. **What is Jenkins?**

   Jenkins is an open-source automation server used to automate parts of the software development process like building, testing, and deploying.

2. **What are the key features of Jenkins?**

* Open-source and free
* Distributed build system
* Integration with many tools
* Supports pipelines
* Extensible via plugins

3. **How does Jenkins work?**

   Jenkins polls the version control system (e.g., Git) for changes, and when changes are detected, it triggers a build and executes defined steps like build, test, and deploy.

4. **What is a Jenkins pipeline?**

   A pipeline is a suite of plugins that supports integrating and implementing continuous delivery pipelines using code.

5. **What is the difference between freestyle project and pipeline?**

   Freestyle: UI-based and limited customization.
   Pipeline: Scripted and supports complex workflows as code.

6. **What are the types of pipelines in Jenkins?**

* Declarative Pipeline
* Scripted Pipeline

7. **What is Jenkinsfile?**

   It’s a text file that contains the definition of a Jenkins pipeline and is checked into source control.

8. **Where is Jenkins installed?**

   Typically as a WAR file on Java, or using system packages like `.rpm`, `.deb`, or Docker.

9. **How do you configure a job in Jenkins?**

   By creating a new job, choosing project type (freestyle/pipeline), and setting steps like source control, build steps, post-build actions.

10. **What is the use of Jenkins executor?**

    Executor is a computational slot for running builds on an agent.

11. **How do you trigger a Jenkins job automatically?**

    Using SCM polling, webhook triggers, or scheduling (CRON syntax).

12. **What is a build trigger?**

    A mechanism to start a job (e.g., after another job, SCM change, webhook).

13. **What is an agent in Jenkins?**

    An agent is a node (machine) where Jenkins runs jobs.

14. **Difference between master and agent?**

    Master: controls Jenkins setup and job scheduling.
    Agent: executes jobs as per master’s direction.

15. **How to secure Jenkins?**

* Enable security realm
* Use Matrix-based security
* Integrate LDAP or SSO
* Use Role-Based Access Control plugin

16. **How do you integrate Jenkins with GitHub?**

    Using GitHub plugin and webhooks to trigger jobs on commit/pull request.

17. **What plugins have you used in Jenkins?**

    Git, Docker, Slack, Pipeline, Blue Ocean, Email Extension, Role Strategy, Kubernetes.

18. **How do you install a plugin in Jenkins?**

    Go to: Manage Jenkins → Manage Plugins → Available → Search → Install.

19. **How do you monitor Jenkins?**

    Using monitoring plugins (e.g., Prometheus), logs, and alerts.

20. **What is a parameterized build?**

    Allows passing parameters (string, choice, boolean) to customize job behavior.

21. **Explain declarative pipeline syntax.**

    Structured format using `pipeline {}` block. Easier to read and validate.

22. **Explain scripted pipeline syntax.**

    Groovy-based flexible syntax using `node {}` blocks.

23. **What is the use of `stage` and `steps`?**

    `stage`: logical section in the pipeline
    `steps`: actual commands/tasks within a stage

24. **How do you handle errors in a pipeline?**

    Using `try-catch`, `catchError`, or `when` blocks.

25. **What is post section in pipeline?**

    Executes tasks regardless of build result:

```groovy
post {
  always { ... }
  success { ... }
  failure { ... }
}
```

26. **How to send email notifications in Jenkins?**

    Using Email Extension plugin in post-build or pipeline post block.

27. **What is input step in Jenkins pipeline?**

    Pauses the pipeline for manual intervention/approval.

28. **What is `when` condition in declarative pipeline?**

    Used to control whether a stage should run.

29. **What are shared libraries in Jenkins?**

    Reusable Groovy scripts stored in SCM to be used across pipelines.

30. **How to define environment variables in pipeline?**

```groovy
environment {
  VAR_NAME = "value"
}
```

31. **What is the difference between `node` and `agent`?**

    `node` is used in scripted pipelines.
    `agent` is used in declarative pipelines to assign where the job runs.

32. **How do you deploy code using Jenkins?**

    By defining deployment stages using scripts or plugins (e.g., SSH, Ansible, Helm, etc.)

33. **How do you rollback in Jenkins pipeline?**

    Use versioning, tagging, and rollback scripts or previous stable deployments.

34. **What is Blue Ocean in Jenkins?**

    Modern UI for Jenkins pipelines with better visualization.

35. **How to run parallel stages in pipeline?**

```groovy
parallel {
  stageA { steps { ... } }
  stageB { steps { ... } }
}
```

36. **What is `stash` and `unstash` in Jenkins?**

    Used to share files between pipeline stages.

37. **How do you pass credentials in Jenkins securely?**

    Using Jenkins credentials store and referencing them via `withCredentials`.

38. **How do you archive artifacts in Jenkins?**

    Use `archiveArtifacts` step in pipeline or post-build action in freestyle.

39. **How do you clean workspace before/after build?**

    Use `cleanWs()` plugin or configure build environment.

40. **How do you integrate Docker with Jenkins?**

    Install Docker plugin, run Docker commands in build steps, or use Docker agents.

41. **What to do if Jenkins build fails suddenly?**

* Check console logs
* Check SCM and build script changes
* Restart Jenkins if necessary
* Look at plugin versions or system changes

42. **How do you handle flaky tests in Jenkins?**

    Isolate tests, retry strategy, improve test stability, or move to separate stage.

43. **How to run a pipeline on multiple environments?**

    Use `matrix` or environment-specific stages with conditions.

44. **How do you use Jenkins with Kubernetes?**

    Use Kubernetes plugin to dynamically provision pods as Jenkins agents.

45. **How do you maintain Jenkins jobs in large environments?**

    Use Job DSL or pipelines with shared libraries to define jobs as code.

46. **How to handle credentials in pipeline?**


```groovy
withCredentials([string(credentialsId: 'id', variable: 'VAR')]) {
  sh 'echo $VAR'
}
```

47. **How do you integrate Jenkins with Slack or MS Teams?**

    Using notifier plugins and webhook URLs.

48. **What is the role of SCM in Jenkins pipeline?**

    SCM provides source code, pipeline definitions (Jenkinsfile), and triggers for builds.

49. **How do you avoid hardcoding values in pipeline?**

    Use parameters, environment variables, and external config files.

50. **How do you back up Jenkins configuration?**

    Regularly back up `$JENKINS_HOME`, or use backup plugins.

#### How do you configure AWS security credentials in Jenkins?

Use:

Jenkins Credentials Manager (add AWS keys)

Environment variables

AWS CLI with IAM role on EC2 or IRSA in EKS

#### Can you explain the build stages in Jenkins for your project?

Common stages:

Checkout

Build

Test

Docker Build

Push to ECR

Deploy to Kubernetes (kubectl)

#### What are Jenkins shared libraries and how do they work?

Shared Groovy code (like vars/mySteps.groovy) stored in a Git repo and imported across pipelines using @Library.

#### What are the different types of Jenkins jobs?

Freestyle Job

Pipeline Job (Scripted / Declarative)

Multibranch Pipeline

Folder / Matrix Job

#### Do you work with freestyle projects or scripted pipelines in Jenkins?

Scripted pipelines offer more control; declarative pipelines are preferred for structured, readable, and version-controlled CI/CD.

#### Key difference when configuring a new CI/CD pipeline?

- Jenkins: Customizable but manual setup (Jenkinsfile, agents, plugins).
- GitLab: Easy, tightly integrated with Git, uses .gitlab-ci.yml.

#### What is the difference between Declarative and Scripted Pipeline in Jenkins?
Declarative Pipeline: Uses a more structured and predefined syntax. Easier to write and read, especially for beginners.

Scripted Pipeline: Uses Groovy-based syntax. Offers more flexibility and is suitable for complex logic.

#### How can you upgrade Jenkins?
1, Backup Jenkins (Recommended) 
Backup your Jenkins home directory (usually /var/lib/jenkins):(sudo cp -r /var/lib/jenkins /var/lib/jenkins_backup)

2, Upgrade Jenkins (Using Package Manager)

sudo yum check-update
sudo yum upgrade jenkins
sudo systemctl restart jenkins

#### What is Jenkins Master-Slave Architecture?
Master: The main Jenkins server that manages the overall environment — it schedules builds, handles the UI, and delegates tasks to agents.
Slave (Agent): Remote machines that connect to the master and run the actual build/test jobs. They can be Linux, Windows, or container-based systems.

#### How it works:
Master receives a trigger (e.g., code push).

Assigns the job to an available slave.

Slave executes the job and reports back the result.

Connection methods: SSH, JNLP, or WebSocket.

#### Why use it?
Load distribution

Run jobs in parallel

Use different environments for different jobs (e.g., Java on one, Python on another)


#### What is SonarQube?
SonarQube is a code quality and security analysis tool.

It inspects code for bugs, vulnerabilities, and code smells.

Integrates with Jenkins, GitHub, and other CI/CD tools.

#### How to integrate GitHub with Jenkins?
Install Git and GitHub plugin in Jenkins.

Create a GitHub Personal Access Token.

In Jenkins, go to Manage Jenkins > Configure System > GitHub and add credentials.

Set up webhooks in GitHub to trigger builds on code push.

#### What are Shared Libraries in Jenkins?
Shared libraries allow you to reuse common code across multiple Jenkins pipelines.

They are stored in a separate Git repo or directory structure and loaded using:

@Library('library-name') _( in shell script)

#### Explain Jenkins architecture.
 Jenkins follows a master-agent architecture.

The master schedules builds, manages agents, and handles web UI.

Agents execute build jobs on different platforms or environments.

#### What is the difference between Jenkins agents and labels?
 Agents are machines (nodes) that run jobs.

Labels are tags you assign to agents to group them for job scheduling (e.g., linux, docker).

#### If we get an error while pipeline execution, how do you solve it?
Answer:
•	Check pipeline logs to identify the exact stage and error message.

•	Reproduce the issue locally (if possible).

•	Fix config or script issues (e.g., syntax, credentials).

•	Rerun the pipeline and monitor.

#### How can you automatically trigger jobs in a CI/CD pipeline?

Via webhooks, polling SCM, Git push triggers, or GitHub Actions → Jenkins integration.

#### How do you connect Jenkins to the cloud (AWS)?
Q: How do you connect Jenkins to cloud environments like AWS?

 I configure Jenkins with AWS CLI/SDK or IAM credentials (via credentials plugin). I install plugins like AWS EC2, use IAM roles (on EC2 agents), and store secrets in AWS Secrets Manager or Jenkins credentials.

