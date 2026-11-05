 <div align="center">

# 🚀 GitLab CI/CD – Complete Pipeline Keywords & Syntax Reference

📚 **Documentation Type:** A beginner-friendly reference for GitLab CI/CD, pipeline stages, jobs, YAML keywords, nested configurations, variables, rules, and automation.

![GitLab CI/CD](https://img.shields.io/badge/GitLab-CI%2FCD-FC6D26?logo=gitlab&logoColor=white)
![YAML](https://img.shields.io/badge/Config-YAML-CB171E?logo=yaml&logoColor=white)
![Level](https://img.shields.io/badge/Level-Beginner%20to%20Advanced-success)
![Java](https://img.shields.io/badge/Java-Application-orange?logo=openjdk)
![Maven](https://img.shields.io/badge/Maven-Build-blue?logo=apachemaven)
![Docker](https://img.shields.io/badge/Docker-Containerization-blue?logo=docker)
![AWS](https://img.shields.io/badge/AWS-Deployment-orange?logo=amazonaws)
</div>

---

> 🛠️ **Technology:** GitLab CI/CD + YAML  
> 🎯 **Focus:** Pipeline keywords, stages, jobs, runners, variables, rules, artifacts, caching, reusable configurations, security and deployment  
> ☕ **Examples:** Java + Maven + Spring Boot + Docker + AWS EC2  
> 👨‍💻 **Maintained by:** byteAlok | Cloud and DevOps Learner  
> 🧪 **Examples:** Dummy repository names and placeholder values only

---

# 📑 Table of Contents

1. [📖 Introduction & GitLab CI/CD File Structure](#1--introduction--gitlab-cicd-file-structure)
2. [🧱 YAML Basics & Pipeline Anatomy](#2--yaml-basics--pipeline-anatomy)
3. [🚀 Pipeline Triggers & Workflow Rules](#3--pipeline-triggers--workflow-rules)
4. [⚙️ Global Keywords & Configuration](#4--global-keywords--configuration)
5. [🧩 Stages & Stage Execution](#5--stages--stage-execution)
6. [🪜 Jobs & Job-Level Keywords](#6--jobs--job-level-keywords)
7. [🔌 Scripts, Before Script & After Script](#7--scripts-before-script--after-script)
8. [🔐 Variables, Secrets & Protected Variables](#8--variables-secrets--protected-variables)
9. [🧠 Predefined Variables, Expressions & Rules](#9--predefined-variables-expressions--rules)
10. [🔀 Job Dependencies, Needs & Outputs](#10--job-dependencies-needs--outputs)
11. [🧮 Parallel, Matrix & Retry Strategies](#11--parallel-matrix--retry-strategies)
12. [📦 Cache, Artifacts, Environments & Services](#12--cache-artifacts-environments--services)
13. [♻️ Includes, YAML Anchors & Reusable Components](#13--includes-yaml-anchors--reusable-components)
14. [🛡️ Security, Best Practices & Common Mistakes](#14--security-best-practices--common-mistakes)
15. [🏗️ Complete Java + Docker + AWS EC2 CI/CD Pipeline](#15--complete-java--docker--aws-ec2-cicd-pipeline)

---

# 1. 📖 Introduction & GitLab CI/CD File Structure

This section introduces GitLab CI/CD, explains its main components, and shows where pipeline configuration is written inside a GitLab repository.

## 1.1 What is GitLab CI/CD? 🚀

GitLab CI/CD is a DevOps automation platform that helps developers automatically build, test, package, and deploy applications whenever code changes.

**CI – Continuous Integration**

- Developers frequently push their code into a shared Git repository.
- GitLab automatically executes configured build and testing jobs.
- Code integration problems are detected before deployment.

**CD – Continuous Delivery / Continuous Deployment**

- Continuous Delivery prepares tested application versions for release.
- Continuous Deployment automatically releases successful changes into production.
- Both approaches reduce manual deployment work and improve delivery reliability.

## 1.2 GitLab CI/CD Architecture

```text
             👨‍💻 Developer
                  |
                  | git push
                  v
          📦 GitLab Repository
                  |
                  v
           📄 .gitlab-ci.yml
                  |
                  v
            🚀 Pipeline
                  |
          +-------+-------+
          |               |
          v               v
       🧩 Job 1         🧩 Job 2
          |               |
          v               v
       🏗️ Build          🔍 Test
          |               |
          +-------+-------+
                  |
                  v
              📦 Package
                  |
                  v
              🐳 Docker
                  |
                  v
             ☁️ AWS EC2
                  |
                  v
          🌐 Running Application
```

## 1.3 Important GitLab CI/CD Components

| Component | Definition |
|---|---|
| 📄 Pipeline | A pipeline is an automated sequence of jobs that GitLab executes after a configured event occurs. |
| 🧩 Job | A job represents an individual unit of work executed by a GitLab Runner. |
| 🪜 Stage | A stage groups related jobs and determines their overall execution order within a pipeline. |
| 🏃 Runner | A runner is an execution agent responsible for running jobs assigned by GitLab. |
| ⚙️ Executor | An executor defines the environment in which a runner executes configured jobs. |
| 📦 Artifact | An artifact is a generated file or directory saved for later jobs or downloads. |
| 💾 Cache | A cache stores reusable files between jobs or pipeline executions to improve performance. |
| 🔐 Variable | A variable stores configuration values that jobs can access during pipeline execution. |
| 🐳 Image | An image provides the container environment in which a job executes. |
| 🔗 Include | An include imports external or local CI/CD configuration into the main pipeline. |

## 1.4 Where is the GitLab CI/CD Configuration Written?

GitLab uses a YAML file named `.gitlab-ci.yml` in the root directory of the repository by default.

```text
Java-SpringBoot-Application/
│
├── .gitlab-ci.yml              # Main GitLab CI/CD configuration
│
├── src/
│   ├── main/
│   │   └── java/
│   │       └── com/example/
│   │           └── Application.java
│   │
│   └── test/
│       └── java/
│
├── pom.xml                     # Maven configuration
├── Dockerfile                  # Docker image instructions
└── README.md
```

**Important:** GitLab also supports specifying a different CI/CD configuration file through project settings.

## 1.5 Basic GitLab Pipeline Example

```yaml
# .gitlab-ci.yml

# Define the execution order of pipeline stages.
stages:
  - build
  - test

# Job 1: Build the Java application.
build-job:
  stage: build
  image: maven:3.9-eclipse-temurin-17
  script:
    - mvn clean package -DskipTests

# Job 2: Execute Java application tests.
test-job:
  stage: test
  image: maven:3.9-eclipse-temurin-17
  script:
    - mvn test
```

**Execution:**

1. Developer pushes Java application code.
2. GitLab reads `.gitlab-ci.yml`.
3. GitLab creates a pipeline.
4. The build job compiles and packages the application.
5. The test job executes after the build stage succeeds.

## 1.6 GitLab CI/CD Official Resources

- [GitLab CI/CD Documentation](https://docs.gitlab.com/ci/)
- [CI/CD YAML Syntax Reference](https://docs.gitlab.com/ci/yaml/)
- [GitLab Runner Documentation](https://docs.gitlab.com/runner/)
- [GitLab CI/CD Examples](https://docs.gitlab.com/ci/examples/)

---

# 2. 🧱 YAML Basics & Pipeline Anatomy

This section explains YAML syntax and how GitLab interprets indentation, mappings, lists, strings, comments, and nested configuration.

## 2.1 What is YAML?

YAML is a human-readable data serialization format used to define structured configuration through indentation, key-value pairs, and collections.

GitLab CI/CD uses YAML to describe pipelines, stages, jobs, variables, and execution instructions.

## 2.2 Key-Value Pairs

A key-value pair associates a configuration key with its corresponding value using a colon separator.

```yaml
# Key       # Value
name: Java-CI-Pipeline

# Another key-value pair.
image: eclipse-temurin:17
```

## 2.3 Lists / Arrays

A YAML list represents multiple values under one parent key using hyphens and consistent indentation.

```yaml
# Define multiple pipeline stages.
stages:
  - build
  - test
  - package
  - deploy
```

## 2.4 Nested Mappings

Nested mappings represent hierarchical configuration where child properties belong to their parent configuration key.

```yaml
# Parent keyword: variables
variables:

  # Child variable.
  JAVA_VERSION: "17"

  # Another child variable.
  MAVEN_OPTS: "-Dmaven.repo.local=.m2/repository"

  # Nested configuration is created through indentation.
  DEPLOY:
    ENVIRONMENT: "production"
    REGION: "ap-south-1"
```

## 2.5 YAML Comments

Comments document configuration and are ignored by the YAML parser during pipeline execution.

```yaml
# This is a YAML comment.

build-job:
  stage: build

  script:
    # Compile Java source code.
    - mvn compile

    # Execute unit tests.
    - mvn test
```

## 2.6 Multiline Strings: `|` and `>`

| Symbol | Meaning | Behavior |
|---|---|---|
| `|` | Literal block scalar | Preserves line breaks inside the multiline string. |
| `>` | Folded block scalar | Folds most line breaks into spaces when parsing the string. |

**Using `|`:**

```yaml
build-job:
  script:
    - |
      echo "Starting Java build"
      mvn clean compile
      echo "Build completed"
```

**Using `>`:**

```yaml
build-job:
  script:
    - >
      echo "Starting Java build"
      && mvn clean compile
      && echo "Build completed"
```

## 2.7 Strings, Numbers and Boolean Values

```yaml
variables:
  # String value.
  APP_NAME: "spring-boot-api"

  # Numeric-looking values are commonly stored as strings.
  JAVA_VERSION: "17"

  # Boolean YAML value.
  ENABLE_DEPLOYMENT: true

  # Another Boolean value.
  DEBUG_MODE: false
```

## 2.8 YAML Indentation Rules

YAML indentation determines the relationship between parent and child configuration elements.

```yaml
# Correct indentation.
build-job:
  stage: build

  script:
    - echo "Building Java application"
    - mvn clean package
```

Incorrect indentation can cause YAML parsing errors or unexpected configuration structures.

**Remember:**

- Use consistent spaces for indentation.
- Avoid mixing tabs and spaces.
- Keep child keywords indented beneath their parent.
- List items must use the same indentation level.

## 2.9 Complete Pipeline Anatomy

```yaml
# 1. Global pipeline configuration.
stages:
  - build
  - test

# 2. Global variables.
variables:
  JAVA_VERSION: "17"

# 3. Job definition.
build-java:
  # 4. Job-level configuration.
  stage: build

  # 5. Execution environment.
  image: eclipse-temurin:17

  # 6. Commands executed by the runner.
  script:
    - java -version
    - mvn clean package
```

---

# 3. 🚀 Pipeline Triggers & Workflow Rules

This section explains how GitLab decides when to create pipelines and how to control pipeline execution using events and workflow rules.

## 3.1 What is a Pipeline Trigger?

A pipeline trigger is an event or manual action that causes GitLab to create and execute a configured pipeline.

## 3.2 Common Pipeline Sources

| Source | Definition |
|---|---|
| `push` | A push pipeline runs when commits are pushed to a branch or tag. |
| `merge_request_event` | A merge request pipeline validates changes submitted through a merge request. |
| `schedule` | A scheduled pipeline runs automatically according to a configured schedule. |
| `web` | A web pipeline starts when a user manually runs it through GitLab interface. |
| `api` | An API pipeline starts when an authorized request creates a pipeline. |
| `trigger` | A trigger pipeline starts through a configured pipeline trigger token. |
| `pipeline` | A pipeline source represents a downstream multi-project pipeline trigger. |
| `parent_pipeline` | A parent-pipeline source identifies a child pipeline triggered by another pipeline. |
| `external` | An external source identifies a pipeline created through an external integration. |
| `chat` | A chat pipeline starts through a configured GitLab chat integration. |
| `external_pull_request_event` | An external pull request event can trigger pipelines in supported repository integrations. |

## 3.3 `workflow`

`workflow` controls whether GitLab creates a pipeline for a particular event or repository condition.

```yaml
# Global keyword: workflow.
workflow:

  # Define pipeline creation conditions.
  rules:

    # Create a pipeline for merge requests.
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'

    # Create a pipeline for pushes to the main branch.
    - if: '$CI_COMMIT_BRANCH == "main"'

    # Reject other pipeline creation requests.
    - when: never
```

## 3.4 `workflow:rules`

| Keyword | Definition |
|---|---|
| `rules` | Rules define ordered conditions that control whether a pipeline or job is included. |
| `if` | The if keyword evaluates a condition using supported variables and expression operators. |
| `when` | The when keyword determines the execution behavior after a matching rule. |
| `variables` | Rule-level variables define values that become available when a condition matches. |
| `changes` | Changes checks whether specified repository files changed for the current pipeline. |
| `exists` | Exists checks whether specified files or paths exist in the repository. |

## 3.5 Pipeline Only for Main Branch

```yaml
workflow:
  rules:

    # Run pipeline only when the branch is main.
    - if: '$CI_COMMIT_BRANCH == "main"'

    # Do not create a pipeline for other branches.
    - when: never
```

## 3.6 Pipeline for Push and Merge Request

```yaml
workflow:
  rules:

    # Run for normal branch pushes.
    - if: '$CI_PIPELINE_SOURCE == "push"'

    # Run for merge request events.
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'

    # Ignore all other events.
    - when: never
```

## 3.7 Scheduled Pipelines

Scheduled pipelines execute automatically at configured times using cron expressions.

```yaml
# .gitlab-ci.yml

nightly-test:
  stage: test

  script:
    - mvn clean test

  rules:
    # Run only for scheduled pipelines.
    - if: '$CI_PIPELINE_SOURCE == "schedule"'
```

Example cron expression:

```text
0 2 * * *
```

This schedule means the pipeline is intended to run daily at 02:00 according to the schedule's configured timezone.

## 3.8 Manual Pipeline Execution

A manually started pipeline can be created from GitLab's Run Pipeline interface.

```yaml
workflow:
  rules:

    # Allow manually started web pipelines.
    - if: '$CI_PIPELINE_SOURCE == "web"'

    # Reject other pipeline sources.
    - when: never
```

## 3.9 Job-Level Rules vs Workflow Rules

| Feature | `workflow:rules` | Job-level `rules` |
|---|---|---|
| Purpose | Controls pipeline creation. | Controls job creation and configuration. |
| Location | Top-level workflow configuration. | Inside an individual job. |
| Evaluation | Evaluated when GitLab creates the pipeline. | Evaluated when GitLab creates the pipeline. |
| Scope | Entire pipeline. | Individual job. |

**Important:** `workflow:rules` decides whether a pipeline exists; job `rules` decides whether an individual job is included.

---

# 4. ⚙️ Global Keywords & Configuration

This section explains global keywords that configure pipeline-wide behavior and provide default settings for multiple jobs.

## 4.1 Global Keywords Overview

| Keyword | Definition |
|---|---|
| `stages` | Defines the ordered collection of stages used to organize pipeline jobs. |
| `workflow` | Controls whether GitLab creates a pipeline for particular events or conditions. |
| `variables` | Defines default configuration values that are available to pipeline jobs. |
| `default` | Provides default job configuration that individual jobs can inherit or override. |
| `include` | Imports additional CI/CD configuration from local or external YAML files. |
| `spec` | Defines configuration specifications and input contracts for supported configuration files. |

## 4.2 `name`

`name` defines a readable name for a pipeline configuration or supported specification.

```yaml
# Pipeline name.
workflow:
  name: "Java Application CI/CD"
  rules:
    - if: '$CI_COMMIT_BRANCH'
```

## 4.3 `stages`

`stages` defines the order in which groups of pipeline jobs are executed.

```yaml
stages:
  - build
  - test
  - package
  - deploy
```

## 4.4 `variables`

`variables` defines reusable configuration values accessible to jobs unless overridden at a narrower scope.

```yaml
variables:
  # Java application configuration.
  APP_NAME: "spring-boot-api"

  # Maven configuration.
  MAVEN_OPTS: "-Dmaven.repo.local=.m2/repository"

  # Docker image configuration.
  DOCKER_IMAGE: "spring-boot-api"

  # Deployment configuration.
  DEPLOY_ENV: "production"
```

## 4.5 `default`

`default` provides reusable job settings that apply when an individual job does not override them.

```yaml
default:

  # Default Docker image for jobs.
  image: maven:3.9-eclipse-temurin-17

  # Default command executed before each job script.
  before_script:
    - java -version
    - mvn -version

  # Default retry behavior.
  retry:
    max: 2
    when:
      - runner_system_failure
      - stuck_or_timeout_failure
```

Common supported default keywords include:

- `image`
- `services`
- `before_script`
- `after_script`
- `cache`
- `artifacts`
- `retry`
- `interruptible`
- `tags`
- `timeout`

## 4.6 `inherit`

`inherit` controls which global variables and default configuration a job receives.

```yaml
default:
  image: eclipse-temurin:17

  before_script:
    - echo "Global setup"

variables:
  JAVA_VERSION: "17"
  APP_NAME: "spring-api"

# Job-level inheritance configuration.
special-job:
  stage: build

  inherit:
    # Do not inherit global variables.
    variables: false

    # Do not inherit the default before_script.
    default:
      - image

  script:
    - echo "Special job configuration"
```

## 4.7 `include`

`include` imports reusable YAML configuration from supported locations and merges it into the pipeline configuration.

```yaml
# Import multiple configuration files.
include:

  # Import a local configuration file.
  - local: '/ci/build.yml'

  # Import another local configuration file.
  - local: '/ci/test.yml'

  # Import a project-level configuration.
  - project: 'devops/shared-ci'
    ref: main
    file: '/templates/java.yml'
```

## 4.8 `include` Types

| Keyword | Definition |
|---|---|
| `local` | Imports a YAML configuration file located inside the same Git repository. |
| `project` | Imports configuration from another GitLab project accessible to the current user. |
| `remote` | Imports a configuration file from a publicly accessible remote HTTP or HTTPS URL. |
| `template` | Imports a predefined GitLab CI/CD template maintained by GitLab. |
| `component` | Imports a versioned CI/CD component from a configured GitLab component project. |
| `rules` | Controls conditional inclusion of configuration files using supported pipeline conditions. |
| `inputs` | Supplies supported input values to reusable configurations or CI/CD components. |
| `integrity` | Verifies the SHA-256 integrity of supported remote configuration files. |

## 4.9 `include:rules`

```yaml
include:

  - local: '/ci/docker.yml'

    # Include Docker configuration only on main.
    rules:
      - if: '$CI_COMMIT_BRANCH == "main"'
```

## 4.10 `include:inputs`

```yaml
include:
  - local: '/ci/java-template.yml'

    # Values passed to the included configuration.
    inputs:
      java-version: "17"
      application-name: "spring-api"
```

The included file must define compatible inputs through its supported `spec:inputs` configuration.

## 4.11 `spec:inputs`

```yaml
# Reusable configuration file.

spec:
  inputs:

    # Required Java version input.
    java-version:
      type: string
      default: "17"
      description: "Java version used for the build"

    # Application name input.
    application-name:
      type: string
      default: "spring-api"
---
build-java:
  image: eclipse-temurin:$[[ inputs.java-version ]]

  script:
    - echo "Building $[[ inputs.application-name ]]"
```

## 4.12 `concurrency` Equivalent: `resource_group`

GitLab uses `resource_group` to control concurrent execution of jobs accessing the same deployment resource.

```yaml
deploy-production:
  stage: deploy

  # Prevent simultaneous production deployments.
  resource_group: production-server

  script:
    - echo "Deploying Java application"
```

## 4.13 `interruptible`

`interruptible` identifies jobs that GitLab can safely cancel when newer pipelines make their work redundant.

```yaml
build-java:
  stage: build

  # Allow GitLab to cancel this job when appropriate.
  interruptible: true

  script:
    - mvn clean package
```

## 4.14 `workflow:auto_cancel`

```yaml
workflow:
  auto_cancel:

    # Cancel redundant pipelines when newer commits arrive.
    on_new_commit: interruptible

    # Configure behavior when a job fails.
    on_job_failure: all

  rules:
    - if: '$CI_COMMIT_BRANCH'
```

---

# 5. 🧩 Stages & Stage Execution

This section explains stages, their execution order, and how jobs execute sequentially or concurrently inside a pipeline.

## 5.1 What is a Stage?

A stage is a logical pipeline group that organizes related jobs and controls their execution order.

Example Java application stages:

```text
       🚀 GitLab Pipeline
                |
                v
          🏗️ BUILD STAGE
          /            \
         v              v
    Compile Java    Resolve Maven
         \              /
          +------------+
                |
                v
           🔍 TEST STAGE
                |
                v
          📦 PACKAGE STAGE
                |
                v
          🐳 DOCKER STAGE
                |
                v
          ☁️ DEPLOY STAGE
```

## 5.2 Defining Stages

```yaml
# Global stage definition.
stages:

  # First stage.
  - build

  # Second stage.
  - test

  # Third stage.
  - package

  # Fourth stage.
  - deploy
```

## 5.3 Default GitLab Stages

If no stages are defined, GitLab provides the following default stage sequence.

| Stage | Purpose |
|---|---|
| `.pre` | Runs jobs that must execute before normal configured stages. |
| `build` | Groups jobs responsible for preparing application build outputs. |
| `test` | Groups jobs responsible for executing automated application tests. |
| `deploy` | Groups jobs responsible for deploying successfully prepared application versions. |
| `.post` | Runs jobs that execute after normal configured pipeline stages. |

## 5.4 Jobs in the Same Stage

Jobs in the same stage can run in parallel when suitable runners are available.

```yaml
stages:
  - build
  - test

# Build job 1.
compile-java:
  stage: build
  script:
    - mvn compile

# Build job 2.
check-java-version:
  stage: build
  script:
    - java -version

# Test job waits for build stage.
unit-test:
  stage: test
  script:
    - mvn test
```

## 5.5 Custom Stage Names

```yaml
stages:
  - source-validation
  - compile
  - unit-testing
  - integration-testing
  - docker-build
  - production-deployment
```

Custom stage names make large pipelines easier to understand and maintain.

## 5.6 Special Stages: `.pre` and `.post`

```yaml
stages:
  - .pre
  - build
  - test
  - deploy
  - .post

# Executes before normal stages.
initial-check:
  stage: .pre
  script:
    - echo "Checking repository configuration"

# Executes after normal stages.
cleanup-job:
  stage: .post
  script:
    - echo "Pipeline cleanup"
```

## 5.7 Stage vs Job

| Feature | Stage | Job |
|---|---|---|
| Meaning | Groups related execution tasks. | Performs an individual execution task. |
| Configuration | Uses the global `stages` keyword. | Defined as a top-level YAML job entry. |
| Execution | Controls overall pipeline order. | Executes its own configured commands. |
| Parallelism | Jobs within one stage can run concurrently. | Individual jobs require available compatible runners. |
| Example | `build`, `test`, `deploy`. | `compile-java`, `unit-test`, `deploy-ec2`. |

---

# 6. 🪜 Jobs & Job-Level Keywords

This section explains job definitions and the configuration keywords used to control their execution environment, dependencies, permissions, and behavior.

## 6.1 What is a Job?

A job is an independently configured execution unit that performs commands on a GitLab Runner.

Every normal job must contain either `script` or `trigger` configuration.

## 6.2 Basic Job Structure

```yaml
# Job name.
build-java:

  # Assign this job to the build stage.
  stage: build

  # Select the execution environment.
  image: eclipse-temurin:17

  # Commands executed by the runner.
  script:
    - java -version
    - mvn clean compile
```

## 6.3 Job Keywords Reference

| Keyword | Definition |
|---|---|
| `stage` | Assigns a job to a stage defined in the global pipeline configuration. |
| `script` | Defines shell commands that the runner executes to perform the job. |
| `image` | Selects the Docker image used as the job execution environment. |
| `services` | Starts additional service containers required by the job during execution. |
| `tags` | Selects compatible runners using their configured runner tags. |
| `before_script` | Defines commands executed before the main script within a job. |
| `after_script` | Defines commands executed after the main script has completed. |
| `variables` | Defines job-specific environment variables that override applicable global variables. |
| `timeout` | Overrides the configured project timeout for an individual job execution. |
| `retry` | Configures automatic retries when supported job failures occur. |
| `allow_failure` | Allows a job to fail without necessarily failing the entire pipeline. |
| `when` | Determines when GitLab schedules a job based on its execution policy. |
| `rules` | Evaluates conditions to decide whether a job is included in a pipeline. |
| `needs` | Defines job dependencies that allow execution before normal stage ordering completes. |
| `dependencies` | Restricts which earlier jobs' artifacts are downloaded by the current job. |
| `artifacts` | Defines generated files that GitLab stores after a successful job execution. |
| `cache` | Stores reusable files to accelerate subsequent compatible job executions. |
| `environment` | Associates a job with a deployment environment such as staging or production. |
| `resource_group` | Prevents conflicting jobs from concurrently accessing the same protected resource. |
| `extends` | Reuses configuration from another job or hidden configuration template. |
| `inherit` | Controls whether a job receives global defaults and variables. |
| `interruptible` | Allows suitable jobs to be canceled when newer pipeline work supersedes them. |
| `parallel` | Creates multiple instances of a job for concurrent execution. |
| `coverage` | Defines a regular expression used to extract code coverage from job logs. |
| `release` | Creates a GitLab release using information generated by the pipeline. |
| `trigger` | Starts a downstream pipeline in the same or another project. |
| `manual_confirmation` | Displays a confirmation message before a user starts a manual job. |
| `start_in` | Specifies a delay before a job becomes eligible for execution. |
| `id_tokens` | Requests identity tokens that jobs can use for supported external authentication. |
| `secrets` | Requests supported secret values from configured external secret providers. |
| `identity` | Configures supported identity federation for authenticating jobs with external services. |
| `pages` | Configures a job to publish supported static website content through GitLab Pages. |

## 6.4 `image`

`image` selects the container image that provides the software environment required by a job.

```yaml
build-java:
  stage: build

  # Use Java 17 and Maven environment.
  image: maven:3.9-eclipse-temurin-17

  script:
    - java -version
    - mvn -version
    - mvn clean package
```

Alternative images:

```yaml
# Java runtime image.
image: eclipse-temurin:17

# Maven with Java 17.
image: maven:3.9-eclipse-temurin-17

# Docker CLI image.
image: docker:27-cli
```

## 6.5 `image:name`, `entrypoint`, `pull_policy`

```yaml
build-java:
  image:

    # Container image name.
    name: maven:3.9-eclipse-temurin-17

    # Override the image entrypoint.
    entrypoint: [""]

    # Image download behavior.
    pull_policy: if-not-present

  script:
    - mvn clean package
```

## 6.6 `tags`

`tags` selects runners whose registered tags satisfy the job's runner selection requirements.

```yaml
deploy-job:
  stage: deploy

  # Select a runner registered with these tags.
  tags:
    - aws
    - production
    - shell

  script:
    - echo "Deploying application"
```

## 6.7 `timeout`

`timeout` configures the maximum allowed execution duration for a specific job.

```yaml
build-java:
  stage: build

  # Job timeout.
  timeout: 20 minutes

  script:
    - mvn clean package
```

## 6.8 `retry`

`retry` configures the number and supported failure conditions for automatic job retries.

```yaml
build-java:
  stage: build

  retry:

    # Maximum retry attempts.
    max: 2

    # Retry only selected failure types.
    when:
      - runner_system_failure
      - stuck_or_timeout_failure
      - api_failure

  script:
    - mvn clean package
```

Common retry conditions include:

- `always`
- `unknown_failure`
- `script_failure`
- `api_failure`
- `stuck_or_timeout_failure`
- `runner_system_failure`
- `runner_unsupported`
- `stale_schedule`
- `job_execution_timeout`
- `archived_failure`
- `unmet_prerequisites`
- `scheduler_failure`
- `data_integrity_failure`

## 6.9 `allow_failure`

`allow_failure` allows a job failure without automatically treating it as a blocking pipeline failure.

```yaml
code-quality:
  stage: test

  # This job is optional for pipeline success.
  allow_failure: true

  script:
    - mvn checkstyle:check
```

## 6.10 `when`

`when` controls when a job should be scheduled relative to pipeline status and manual execution.

```yaml
deploy-production:
  stage: deploy

  # Require a user to start deployment manually.
  when: manual

  script:
    - echo "Production deployment started"
```

Common values:

| Value | Definition |
|---|---|
| `on_success` | Runs the job when earlier blocking jobs have completed successfully. |
| `on_failure` | Runs the job when an earlier blocking job causes pipeline failure. |
| `always` | Runs the job regardless of previous job success or failure conditions. |
| `manual` | Requires a user to explicitly start the job through GitLab interface. |
| `delayed` | Schedules the job after a configured delay using `start_in`. |
| `never` | Prevents execution when used in a matching rule configuration. |

## 6.11 `environment`

`environment` associates a job with a named deployment environment.

```yaml
deploy-staging:
  stage: deploy

  environment:

    # Environment name.
    name: staging

    # Optional environment URL.
    url: https://staging.example.com

  script:
    - echo "Deploying to staging"
```

## 6.12 `resource_group`

```yaml
deploy-production:
  stage: deploy

  # Only one deployment job can use this resource group at a time.
  resource_group: production

  script:
    - echo "Deploying to production"
```

## 6.13 `release`

`release` creates a GitLab release associated with a repository tag and pipeline-generated information.

```yaml
create-release:
  stage: deploy

  image: registry.gitlab.com/gitlab-org/release-cli:latest

  release:
    # Release name.
    name: "Java API Release $CI_COMMIT_TAG"

    # Git tag associated with this release.
    tag_name: "$CI_COMMIT_TAG"

    # Release description.
    description: "Automated Java application release"

  script:
    - echo "Creating GitLab release"

  rules:
    - if: '$CI_COMMIT_TAG'
```

## 6.14 `manual_confirmation`

```yaml
deploy-production:
  stage: deploy

  # Ask for confirmation before starting this manual job.
  manual_confirmation: "Are you sure you want to deploy to production?"

  when: manual

  script:
    - echo "Production deployment"
```

---

# 7. 🔌 Scripts, Before Script & After Script

This section explains how GitLab Runner executes shell commands and how setup, main execution, and cleanup commands are organized.

## 7.1 `script`

`script` defines the main shell commands that a runner executes to complete a configured job.

```yaml
build-java:
  stage: build

  script:
    # Check Java installation.
    - java -version

    # Check Maven installation.
    - mvn -version

    # Compile Java application.
    - mvn clean compile

    # Package the application.
    - mvn package -DskipTests
```

## 7.2 `before_script`

`before_script` defines setup commands executed before the main script commands of a job.

```yaml
build-java:
  stage: build

  before_script:
    # Display Java version.
    - java -version

    # Display Maven version.
    - mvn -version

    # Print the current directory.
    - pwd

  script:
    - mvn clean package
```

## 7.3 `after_script`

`after_script` defines commands executed after the main job script for cleanup or reporting tasks.

```yaml
build-java:
  stage: build

  script:
    - mvn clean package

  after_script:
    # Display job completion information.
    - echo "Java build job completed"

    # Show current directory.
    - pwd
```

**Important:** `after_script` runs in a separate shell context. Shell variables and directory changes from the main script should not be assumed to persist into it.

## 7.4 Global vs Job-Level Scripts

```yaml
# Global setup commands.
before_script:
  - echo "Global setup"

# Global cleanup commands.
after_script:
  - echo "Global cleanup"

build-java:
  stage: build

  # Job-level before_script overrides the global version.
  before_script:
    - echo "Java-specific setup"

  script:
    - mvn clean package

  # Job-level after_script overrides the global version.
  after_script:
    - echo "Java build cleanup"
```

## 7.5 `shell`

`shell` is used in supported custom action configurations and runner configuration to define the command interpreter.

For normal GitLab job scripts, the shell is configured by the runner executor.

Example shell commands:

```yaml
linux-job:
  stage: build

  script:
    - echo "Running Linux shell"
    - pwd
    - ls -la
```

For a Windows PowerShell runner:

```yaml
windows-job:
  stage: build

  tags:
    - windows

  script:
    - Write-Host "Running PowerShell"
    - Get-Location
    - Get-ChildItem
```

## 7.6 `parallel`

`parallel` creates multiple instances of a job, which can be useful for independent testing tasks.

```yaml
test-java:
  stage: test

  # Create three parallel job instances.
  parallel: 3

  script:
    - echo "Parallel test execution"
    - mvn test
```

## 7.7 `script` vs `before_script` vs `after_script`

| Keyword | Purpose | Execution |
|---|---|---|
| `before_script` | Prepares the environment before main commands execute. | Before `script`. |
| `script` | Executes the primary task configured for the job. | Main execution. |
| `after_script` | Performs cleanup or reporting after the main script. | After `script`. |

## 7.8 Complete Example

```yaml
build-java:
  stage: build

  image: maven:3.9-eclipse-temurin-17

  before_script:
    - echo "Preparing Java environment"
    - java -version
    - mvn -version

  script:
    - echo "Starting build"
    - mvn clean compile
    - mvn package -DskipTests

  after_script:
    - echo "Build process finished"
```

---

# 8. 🔐 Variables, Secrets & Protected Variables

This section explains how GitLab stores configuration values, exposes environment variables to jobs, and protects credentials used by deployment pipelines.

## 8.1 What is a CI/CD Variable?

A CI/CD variable stores a configuration value that GitLab makes available to supported pipeline jobs and execution environments.

## 8.2 Global Variables

Global variables are defined at the root level and are available to pipeline jobs unless overridden.

```yaml
# Global variables.
variables:

  # Application configuration.
  APP_NAME: "spring-boot-api"

  # Java configuration.
  JAVA_VERSION: "17"

  # Maven configuration.
  MAVEN_OPTS: "-Dmaven.repo.local=.m2/repository"

  # Docker configuration.
  DOCKER_IMAGE: "spring-boot-api"

build-java:
  stage: build

  script:
    - echo "Building $APP_NAME"
    - java -version
    - mvn clean package
```

## 8.3 Job-Level Variables

Job-level variables override global variables with the same name for that job.

```yaml
variables:
  DEPLOY_ENV: "staging"

deploy-production:
  stage: deploy

  # Override global variable for this job.
  variables:
    DEPLOY_ENV: "production"

  script:
    - echo "Deploying to $DEPLOY_ENV"
```

## 8.4 Variable Precedence

GitLab supports multiple variable sources with defined precedence rules.

| Source | Example |
|---|---|
| Pipeline execution variables | Variables passed when a pipeline is created. |
| Project variables | Variables configured in project CI/CD settings. |
| Group variables | Variables configured at a GitLab group level. |
| Instance variables | Variables configured by GitLab administrators. |
| Job-level YAML variables | Variables defined inside an individual job. |
| Global YAML variables | Variables defined at the root of `.gitlab-ci.yml`. |
| Predefined variables | Variables automatically provided by GitLab. |

**Note:** Exact precedence depends on the variable source and GitLab version/configuration. Consult the official variable precedence documentation before designing security-sensitive overrides.

## 8.5 GitLab UI Variables

Sensitive credentials should generally be stored in GitLab project or group CI/CD settings instead of committed YAML files.

Navigation:

```text
GitLab Project
    |
    v
Settings
    |
    v
CI/CD
    |
    v
Variables
    |
    v
Add Variable
```

Example variables for AWS deployment:

| Variable | Example Value | Purpose |
|---|---|---|
| `AWS_ACCESS_KEY_ID` | Stored securely in GitLab | AWS access key identifier when static credentials are used. |
| `AWS_SECRET_ACCESS_KEY` | Stored securely in GitLab | AWS secret key used for authenticated AWS API requests. |
| `AWS_DEFAULT_REGION` | `ap-south-1` | AWS region used by supported AWS commands. |
| `EC2_HOST` | `203.0.113.10` | Public IP or hostname of the deployment server. |
| `EC2_USER` | `ubuntu` | Linux username used for SSH deployment. |
| `EC2_SSH_PRIVATE_KEY` | Private key content | SSH private key used for server authentication. |
| `DOCKER_REGISTRY_USER` | Registry username | Username used for private Docker registry authentication. |
| `DOCKER_REGISTRY_PASSWORD` | Stored securely | Password or token used for private registry authentication. |

These are placeholders; never put actual production credentials in committed configuration.

## 8.6 Protected Variables

Protected variables are restricted to pipelines running on configured protected branches or tags.

```yaml
deploy-production:
  stage: deploy

  # Production deployment should run only on protected refs.
  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'

  script:
    - echo "Deploying production application"
```

Enable the Protected setting for the corresponding credential in GitLab project settings.

## 8.7 Masked Variables

Masked variables prevent supported secret values from appearing directly in job logs.

**Important:** Masking is not a complete security boundary. Avoid printing secrets or passing them into untrusted commands.

## 8.8 File-Type Variables

File-type variables expose a temporary file path containing the configured variable value.

Example:

```yaml
deploy-job:
  stage: deploy

  script:
    # Example: AWS CLI reads credentials from a file variable.
    - aws configure import --csv file://$AWS_CREDENTIALS_FILE
```

The variable must be configured with the appropriate file type and expected content format.

## 8.9 Variable Expansion

```yaml
variables:
  APP_NAME: "spring-api"
  IMAGE_TAG: "$CI_COMMIT_SHORT_SHA"

build-image:
  stage: package

  script:
    - echo "Application: $APP_NAME"
    - echo "Image tag: $IMAGE_TAG"
```

## 8.10 `expand_vars`

In supported GitLab configuration contexts, `expand_vars` can expand references inside variable values.

```yaml
variables:
  APP_NAME: "spring-api"
  APP_VERSION: "1.0"

build-java:
  stage: build

  script:
    - echo "$APP_NAME"
    - echo "$APP_VERSION"
```

Do not assume every variable source or configuration field supports recursive expansion.

## 8.11 `id_tokens`

`id_tokens` requests a signed identity token for authentication with supported external services.

```yaml
deploy-aws:
  stage: deploy

  # Request an OIDC identity token.
  id_tokens:
    GITLAB_OIDC_TOKEN:
      aud: https://sts.amazonaws.com

  script:
    - echo "OIDC token is available to the job"
```

Use the token with a correctly configured AWS IAM OIDC identity provider and role trust policy. Do not print the token.

## 8.12 `secrets`

`secrets` requests supported external secret values through configured GitLab integrations.

```yaml
deploy-job:
  stage: deploy

  secrets:
    # Example external secret configuration.
    DATABASE_PASSWORD:
      vault: production/database/password@ops

  script:
    - echo "Deploying application"
```

The exact secret-provider configuration depends on the integration, GitLab version, and project setup.

## 8.13 `identity`

`identity` configures supported identity federation for authenticating a job with an external service.

```yaml
cloud-job:
  stage: deploy

  # Example supported workload identity configuration.
  identity: google_cloud

  script:
    - echo "Running authenticated cloud deployment"
```

## 8.14 `environment` and Protected Environments

Protected environments restrict who can deploy to sensitive environments.

```yaml
deploy-production:
  stage: deploy

  environment:
    name: production
    url: https://example.com

  when: manual

  script:
    - echo "Deploying production"
```

Configure deployment permissions under the project's protected environment settings.

---

# 9. 🧠 Predefined Variables, Expressions & Rules

This section explains GitLab predefined variables, shell environment variables, rule expressions, and conditions used to control pipeline execution.

## 9.1 What are Predefined Variables?

Predefined variables are automatically generated by GitLab and provide information about the current pipeline, commit, project, branch, and runner.

## 9.2 Common Predefined Variables

| Variable | Definition |
|---|---|
| `$CI` | Indicates whether the current job is executing inside a GitLab CI/CD environment. |
| `$CI_PROJECT_NAME` | Contains the repository project's name for the currently executing pipeline. |
| `$CI_PROJECT_PATH` | Contains the full namespace and project path of the current repository. |
| `$CI_PROJECT_URL` | Provides the web URL of the current GitLab project. |
| `$CI_PROJECT_DIR` | Identifies the directory where the runner checks out the project repository. |
| `$CI_PIPELINE_ID` | Identifies the unique numeric ID assigned to the current pipeline. |
| `$CI_PIPELINE_IID` | Identifies the project-scoped internal ID assigned to the current pipeline. |
| `$CI_PIPELINE_SOURCE` | Identifies the event or mechanism that created the current pipeline. |
| `$CI_JOB_ID` | Identifies the unique numeric ID assigned to the current job. |
| `$CI_JOB_NAME` | Contains the configured name of the currently executing job. |
| `$CI_COMMIT_SHA` | Contains the complete commit SHA associated with the current pipeline. |
| `$CI_COMMIT_SHORT_SHA` | Contains an abbreviated commit SHA useful for image tags and build labels. |
| `$CI_COMMIT_REF_NAME` | Contains the branch or tag name associated with the current pipeline. |
| `$CI_COMMIT_REF_SLUG` | Contains a normalized branch or tag identifier suitable for URLs and names. |
| `$CI_COMMIT_BRANCH` | Contains the branch name for applicable branch pipelines. |
| `$CI_COMMIT_TAG` | Contains the tag name when the pipeline runs for a Git tag. |
| `$CI_DEFAULT_BRANCH` | Contains the repository's configured default branch name. |
| `$CI_REGISTRY` | Contains the GitLab Container Registry hostname when available. |
| `$CI_REGISTRY_IMAGE` | Contains the project's Container Registry image path when available. |
| `$CI_REGISTRY_USER` | Contains the registry username provided for supported job authentication. |
| `$CI_REGISTRY_PASSWORD` | Contains a temporary registry password available to supported jobs. |
| `$CI_RUNNER_ID` | Identifies the runner executing the current job. |
| `$CI_RUNNER_DESCRIPTION` | Contains the description configured for the current runner. |
| `$CI_RUNNER_TAGS` | Contains the tags associated with the runner executing the current job. |
| `$CI_JOB_STATUS` | Indicates the current job status through a predefined environment variable. |
| `$CI_MERGE_REQUEST_IID` | Contains the internal merge request ID for applicable merge request pipelines. |
| `$CI_MERGE_REQUEST_SOURCE_BRANCH_NAME` | Contains the source branch name of the current merge request. |
| `$CI_MERGE_REQUEST_TARGET_BRANCH_NAME` | Contains the target branch name of the current merge request. |

## 9.3 Using Predefined Variables

```yaml
show-pipeline-info:
  stage: build

  script:
    - echo "Project: $CI_PROJECT_NAME"
    - echo "Project Path: $CI_PROJECT_PATH"
    - echo "Pipeline ID: $CI_PIPELINE_ID"
    - echo "Job ID: $CI_JOB_ID"
    - echo "Commit: $CI_COMMIT_SHA"
    - echo "Branch: $CI_COMMIT_REF_NAME"
    - echo "Runner: $CI_RUNNER_DESCRIPTION"
```

## 9.4 Shell Environment Variables

GitLab exposes variables to the shell used by the runner.

Linux shell:

```yaml
show-linux-variable:
  script:
    - echo "$CI_PROJECT_NAME"
    - echo "$CI_COMMIT_SHA"
```

PowerShell:

```yaml
show-windows-variable:
  script:
    - Write-Host $env:CI_PROJECT_NAME
    - Write-Host $env:CI_COMMIT_SHA
```

## 9.5 Variables vs Predefined Variables

| Type | Example | Source |
|---|---|---|
| Custom global variable | `$APP_NAME` | Root-level `variables`. |
| Job variable | `$DEPLOY_ENV` | Job-level `variables`. |
| Project variable | `$AWS_REGION` | GitLab project settings. |
| Group variable | `$REGISTRY_TOKEN` | GitLab group settings. |
| Predefined variable | `$CI_COMMIT_SHA` | GitLab automatically provides it. |

## 9.6 `rules:if`

`rules:if` evaluates an expression to decide whether a job should be included and how it should execute.

```yaml
deploy-production:
  stage: deploy

  rules:

    # Run only on main branch.
    - if: '$CI_COMMIT_BRANCH == "main"'

  script:
    - echo "Production deployment"
```

## 9.7 Comparison Operators

| Operator | Meaning | Example |
|---|---|---|
| `==` | Checks whether two supported values are equal. | `$CI_COMMIT_BRANCH == "main"` |
| `!=` | Checks whether two supported values are different. | `$CI_COMMIT_BRANCH != "main"` |
| `=~` | Checks whether a value matches a supported regular expression. | `$CI_COMMIT_BRANCH =~ /^feature/` |
| `!~` | Checks whether a value does not match a supported regular expression. | `$CI_COMMIT_BRANCH !~ /^release/` |
| `&&` | Requires both expressions to evaluate as true. | `$CI_COMMIT_BRANCH == "main" && $CI_PIPELINE_SOURCE == "push"` |
| `||` | Requires at least one expression to evaluate as true. | `$CI_COMMIT_BRANCH == "main" || $CI_COMMIT_TAG` |
| `()` | Groups expressions to control logical evaluation order. | `($CI_COMMIT_BRANCH == "main")` |

## 9.8 Combining Conditions

```yaml
deploy-production:
  stage: deploy

  rules:

    # Main branch and push pipeline.
    - if: '$CI_COMMIT_BRANCH == "main" && $CI_PIPELINE_SOURCE == "push"'

  script:
    - echo "Deploying production"
```

## 9.9 `rules:changes`

`rules:changes` checks repository changes and includes a job only when configured file paths are modified.

```yaml
build-java:
  stage: build

  rules:
    - changes:

        # Run if Java source changes.
        - "src/**/*.java"

        # Run if Maven configuration changes.
        - "pom.xml"

        # Run if Docker configuration changes.
        - "Dockerfile"

  script:
    - mvn clean package
```

**Important:** For schedules, tags, and other pipelines without a normal push comparison, `rules:changes` can behave differently. Use `compare_to` where an explicit comparison reference is required.

## 9.10 `rules:exists`

`rules:exists` checks whether specified files or paths exist in the repository before including a job.

```yaml
build-java:
  stage: build

  rules:
    - exists:

        # Include the job only if Maven configuration exists.
        - pom.xml

  script:
    - mvn clean package
```

## 9.11 `rules:when`

```yaml
deploy-production:
  stage: deploy

  rules:
    # Create a manual job for main.
    - if: '$CI_COMMIT_BRANCH == "main"'
      when: manual

    # Reject other branches.
    - when: never

  script:
    - echo "Deploying production"
```

## 9.12 `rules:allow_failure`

```yaml
security-check:
  stage: test

  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
      allow_failure: true

  script:
    - mvn verify
```

## 9.13 `rules:variables`

```yaml
deploy-java:
  stage: deploy

  rules:
    # Use staging configuration on develop.
    - if: '$CI_COMMIT_BRANCH == "develop"'
      variables:
        DEPLOY_ENV: "staging"

    # Use production configuration on main.
    - if: '$CI_COMMIT_BRANCH == "main"'
      variables:
        DEPLOY_ENV: "production"

  script:
    - echo "Deploying to $DEPLOY_ENV"
```

## 9.14 `rules:needs`

```yaml
build-staging:
  stage: build
  script:
    - echo "Building staging"

build-production:
  stage: build
  script:
    - echo "Building production"

test-java:
  stage: test

  rules:
    # Use staging build for develop.
    - if: '$CI_COMMIT_BRANCH == "develop"'
      needs:
        - build-staging

    # Use production build for main.
    - if: '$CI_COMMIT_BRANCH == "main"'
      needs:
        - build-production

  script:
    - echo "Running tests"
```

## 9.15 Deprecated `only` and `except`

`only` and `except` are older job-selection keywords that have largely been superseded by the more flexible `rules` syntax.

```yaml
# Older syntax; prefer rules for new configurations.
build-java:
  stage: build

  only:
    - main
    - develop

  script:
    - mvn clean package
```

Modern equivalent:

```yaml
build-java:
  stage: build

  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'
    - if: '$CI_COMMIT_BRANCH == "develop"'

  script:
    - mvn clean package
```

---

# 10. 🔀 Job Dependencies, Needs & Outputs

This section explains how jobs share execution dependencies, pass generated files, and control pipeline execution using `needs`, `dependencies`, and artifacts.

## 10.1 What is a Job Dependency?

A job dependency describes which earlier job must complete or provide output before another job can execute.

## 10.2 Normal Stage Dependency

By default, later stages wait for earlier stages to finish successfully.

```yaml
stages:
  - build
  - test
  - deploy

build-java:
  stage: build
  script:
    - mvn clean package

test-java:
  stage: test
  script:
    - mvn test

deploy-java:
  stage: deploy
  script:
    - echo "Deploying application"
```

## 10.3 `needs`

`needs` defines explicit job dependencies and allows jobs to start without waiting for every job in earlier stages.

```yaml
stages:
  - build
  - test
  - deploy

build-java:
  stage: build
  script:
    - mvn clean package

test-java:
  stage: test

  # Wait only for build-java.
  needs:
    - build-java

  script:
    - mvn test
```

## 10.4 `needs:job`

```yaml
test-java:
  stage: test

  needs:
    # Dependency job.
    - job: build-java

      # Download artifacts from the dependency.
      artifacts: true

  script:
    - mvn test
```

## 10.5 `needs:artifacts`

```yaml
test-java:
  stage: test

  needs:
    - job: build-java

      # Download the build artifact.
      artifacts: true

  script:
    - ls -la target/
```

Disable artifact downloading:

```yaml
test-java:
  stage: test

  needs:
    - job: build-java

      # Do not download artifacts from this dependency.
      artifacts: false

  script:
    - mvn test
```

## 10.6 `needs:optional`

`optional: true` allows a job to depend on another job that might not exist in a particular pipeline.

```yaml
deploy-java:
  stage: deploy

  needs:
    - job: security-check

      # Do not fail pipeline creation if security-check is absent.
      optional: true

  script:
    - echo "Deploying Java application"
```

## 10.7 `needs: []`

An empty needs list allows a job to start immediately when the pipeline is created.

```yaml
quick-validation:
  stage: test

  # No job dependencies.
  needs: []

  script:
    - echo "Checking repository structure"
```

## 10.8 `dependencies`

`dependencies` restricts which earlier-stage job artifacts are downloaded into the current job.

```yaml
stages:
  - build
  - test
  - deploy

build-java:
  stage: build

  script:
    - mvn clean package -DskipTests

  artifacts:
    paths:
      - target/*.jar

test-java:
  stage: test

  # Download artifacts only from build-java.
  dependencies:
    - build-java

  script:
    - ls -la target/
```

**Important:** Avoid combining `needs` and `dependencies` in the same job. Use `needs:artifacts` when explicit dependencies and artifact selection are both required.

## 10.9 `artifacts:reports`

`artifacts:reports` stores supported structured reports for GitLab's reporting interfaces.

```yaml
unit-test:
  stage: test

  script:
    - mvn test

  artifacts:
    reports:

      # JUnit test report.
      junit:
        - target/surefire-reports/TEST-*.xml
```

## 10.10 Job Outputs Through Dotenv Reports

GitLab supports passing generated values to later jobs through dotenv report artifacts.

```yaml
build-java:
  stage: build

  script:
    # Create a dotenv file.
    - echo "APP_VERSION=1.0.0" > build.env
    - echo "IMAGE_TAG=$CI_COMMIT_SHORT_SHA" >> build.env

  artifacts:
    reports:

      # Publish variables for later jobs.
      dotenv: build.env

deploy-java:
  stage: deploy

  needs:
    - job: build-java
      artifacts: true

  script:
    # Variables from build.env become available.
    - echo "Version: $APP_VERSION"
    - echo "Image: $IMAGE_TAG"
```

## 10.11 Passing Files Between Jobs

```yaml
stages:
  - build
  - package

build-java:
  stage: build

  script:
    - mkdir -p output
    - echo "Java application build completed" > output/build-info.txt

  artifacts:
    paths:
      - output/

package-java:
  stage: package

  needs:
    - job: build-java
      artifacts: true

  script:
    - cat output/build-info.txt
```

## 10.12 `trigger` for Downstream Pipelines

`trigger` starts another pipeline in the same project or a different GitLab project.

**Child pipeline example:**

```yaml
trigger-deployment:
  stage: deploy

  trigger:
    # Child pipeline configuration file.
    include: '/ci/deployment.yml'

    # Wait for the child pipeline result.
    strategy: mirror
```

**Multi-project pipeline example:**

```yaml
trigger-other-project:
  stage: deploy

  trigger:
    # Target GitLab project.
    project: 'devops/deployment-project'

    # Target branch.
    branch: main

    # Wait for downstream pipeline status.
    strategy: mirror
```

## 10.13 Trigger Variables

```yaml
trigger-deployment:
  stage: deploy

  trigger:
    project: 'devops/deployment-project'
    branch: main

  variables:
    # Variables passed to the downstream pipeline.
    APP_NAME: "spring-boot-api"
    DEPLOY_ENV: "production"
```

---

# 11. 🧮 Parallel, Matrix & Retry Strategies

This section explains parallel job execution, matrix configurations, failure handling, and strategies for testing applications across multiple environments.

## 11.1 `parallel`

`parallel` creates multiple job instances so independent work can run concurrently.

```yaml
unit-test:
  stage: test

  # Create four job instances.
  parallel: 4

  script:
    - echo "Running parallel test instance"
    - mvn test
```

## 11.2 `parallel:matrix`

`parallel:matrix` generates job combinations from configured variable values.

```yaml
test-java:
  stage: test

  parallel:
    matrix:

      # First dimension.
      - JAVA_VERSION:
          - "17"
          - "21"

        # Second dimension.
        OPERATING_SYSTEM:
          - ubuntu
          - alpine

  script:
    - echo "Java version: $JAVA_VERSION"
    - echo "Operating system: $OPERATING_SYSTEM"
```

This configuration creates four combinations:

| Java Version | Operating System |
|---|---|
| 17 | ubuntu |
| 17 | alpine |
| 21 | ubuntu |
| 21 | alpine |

## 11.3 `parallel:matrix:exclude`

`exclude` removes selected combinations from a matrix where supported.

```yaml
test-java:
  stage: test

  parallel:
    matrix:

      - JAVA_VERSION:
          - "17"
          - "21"

        OS:
          - ubuntu
          - alpine

        # Exclude an unsupported combination.
        exclude:
          - JAVA_VERSION: "17"
            OS: alpine

  script:
    - echo "Testing $JAVA_VERSION on $OS"
```

## 11.4 `parallel:matrix:include`

`include` adds extra matrix combinations or values.

```yaml
test-java:
  stage: test

  parallel:
    matrix:

      - JAVA_VERSION:
          - "17"
          - "21"

        OS:
          - ubuntu

        # Add an additional matrix configuration.
        include:
          - JAVA_VERSION: "25"
            OS: ubuntu

  script:
    - echo "Java: $JAVA_VERSION"
    - echo "OS: $OS"
```

## 11.5 `retry`

`retry` configures automatic retries when a job encounters selected failures.

```yaml
integration-test:
  stage: test

  retry:
    max: 2

    when:
      - runner_system_failure
      - api_failure

  script:
    - mvn verify
```

## 11.6 `allow_failure`

`allow_failure` allows selected jobs to fail without blocking overall pipeline success.

```yaml
code-analysis:
  stage: test

  allow_failure: true

  script:
    - mvn checkstyle:check
```

## 11.7 `parallel` vs `parallel:matrix`

| Feature | `parallel` | `parallel:matrix` |
|---|---|---|
| Purpose | Creates multiple identical job instances. | Creates jobs with different variable combinations. |
| Configuration | Uses an integer count. | Uses one or more matrix variable lists. |
| Variables | Does not automatically assign unique matrix values. | Assigns values from configured matrix combinations. |
| Example | `parallel: 4` | Java 17/21 across Ubuntu/Alpine. |

---

# 12. 📦 Cache, Artifacts, Environments & Services

This section explains dependency caching, generated build artifacts, deployment environments, and additional containers required by Java applications.

## 12.1 `cache`

`cache` stores reusable files between compatible jobs and pipelines to reduce repeated dependency downloads.

For Maven, the local dependency repository is commonly stored inside `.m2/repository`.

```yaml
# Global cache configuration.
cache:

  # Cache key.
  key: maven-cache

  # Cache Maven dependencies.
  paths:
    - .m2/repository/
```

## 12.2 `cache:key`

`key` identifies the cache that GitLab should restore or update for a job.

```yaml
build-java:
  stage: build

  cache:
    key: java-maven-cache

    paths:
      - .m2/repository/

  script:
    - mvn clean package
```

## 12.3 `cache:key:files`

`files` creates a cache key based on the content of specified files.

```yaml
build-java:
  stage: build

  cache:
    key:
      files:
        # Recreate cache when Maven dependency configuration changes.
        - pom.xml

    paths:
      - .m2/repository/

  script:
    - mvn clean package
```

## 12.4 `cache:policy`

`policy` controls whether a job downloads, uploads, or performs both operations for its configured cache.

```yaml
build-java:
  stage: build

  cache:
    key: maven-cache
    paths:
      - .m2/repository/

    # Download and upload cache.
    policy: pull-push

  script:
    - mvn clean package
```

Common values:

| Value | Definition |
|---|---|
| `pull` | Downloads an existing cache without uploading updated cache contents. |
| `push` | Uploads cache contents without downloading an existing cache first. |
| `pull-push` | Downloads an existing cache and uploads updated contents after execution. |

## 12.5 `cache:untracked`

`untracked` includes untracked repository files in the configured cache.

```yaml
build-java:
  stage: build

  cache:
    untracked: true

    paths:
      - .m2/repository/

  script:
    - mvn clean package
```

## 12.6 `cache:when`

`when` determines whether GitLab uploads cache contents after the job finishes.

```yaml
build-java:
  stage: build

  cache:
    key: maven-cache
    paths:
      - .m2/repository/

    # Upload cache after job completion.
    when: on_success

  script:
    - mvn clean package
```

## 12.7 `artifacts`

`artifacts` stores generated files after a job finishes so they can be downloaded or passed to later jobs.

```yaml
build-java:
  stage: build

  script:
    - mvn clean package -DskipTests

  artifacts:

    # Store generated JAR files.
    paths:
      - target/*.jar

    # Keep artifacts for seven days.
    expire_in: 7 days
```

## 12.8 Artifact Keywords

| Keyword | Definition |
|---|---|
| `paths` | Lists generated files and directories that GitLab should save as artifacts. |
| `exclude` | Excludes matching files or directories from the selected artifact paths. |
| `expire_in` | Defines how long GitLab retains the configured job artifacts. |
| `name` | Assigns a custom downloadable archive name to the generated artifact collection. |
| `when` | Controls which job completion conditions cause artifact uploading to occur. |
| `reports` | Stores supported structured reports for GitLab's reporting and analysis interfaces. |
| `expose_as` | Provides a convenient link to selected artifacts in supported merge request interfaces. |
| `access` | Restricts which users can access supported downloadable job artifacts. |
| `untracked` | Includes untracked repository files in the artifact archive when enabled. |
| `public` | Controls public artifact access in versions where this legacy setting is supported. |

## 12.9 `artifacts:exclude`

```yaml
build-java:
  stage: build

  script:
    - mvn clean package

  artifacts:
    paths:
      - target/

    # Exclude temporary files.
    exclude:
      - target/classes/
      - target/test-classes/
```

## 12.10 `artifacts:expire_in`

```yaml
build-java:
  stage: build

  script:
    - mvn clean package

  artifacts:
    paths:
      - target/*.jar

    # Artifact expiration duration.
    expire_in: 1 week
```

## 12.11 `artifacts:when`

```yaml
test-java:
  stage: test

  script:
    - mvn test

  artifacts:
    when: always

    paths:
      - target/surefire-reports/

    reports:
      junit:
        - target/surefire-reports/TEST-*.xml
```

## 12.12 `artifacts:reports`

Common supported report types include:

| Report | Purpose |
|---|---|
| `junit` | Publishes supported JUnit test results for GitLab test reporting. |
| `coverage_report` | Publishes supported code coverage reports for merge request visualization. |
| `dotenv` | Exposes generated environment variables to eligible downstream jobs. |
| `codequality` | Publishes supported code quality findings for GitLab interfaces. |
| `sast` | Publishes supported static application security testing results. |
| `dependency_scanning` | Publishes supported dependency vulnerability analysis results. |
| `container_scanning` | Publishes supported container image vulnerability scan results. |
| `terraform` | Publishes supported Terraform report information in GitLab. |

Example:

```yaml
test-java:
  stage: test

  script:
    - mvn test

  artifacts:
    reports:

      # JUnit report location.
      junit:
        - target/surefire-reports/TEST-*.xml
```

## 12.13 Cache vs Artifacts

| Feature | Cache | Artifacts |
|---|---|---|
| Purpose | Speeds up repeated job execution. | Shares generated output between jobs or users. |
| Typical content | Maven dependencies and downloaded packages. | JAR files, test reports, and build outputs. |
| Persistence | Reused by compatible jobs and pipelines. | Stored according to configured artifact retention. |
| Dependency | Primarily performance optimization. | Can be a required input for downstream jobs. |
| Example | `.m2/repository/` | `target/*.jar` |

## 12.14 `services`

`services` starts additional containers that jobs can access over the configured job network.

Example with PostgreSQL:

```yaml
integration-test:
  stage: test

  image: maven:3.9-eclipse-temurin-17

  services:

    # Start PostgreSQL container.
    - name: postgres:16

      # Alias used by the Java application.
      alias: postgres-db

  variables:

    # Database connection configuration.
    POSTGRES_DB: appdb
    POSTGRES_USER: appuser
    POSTGRES_PASSWORD: testpassword

  script:
    - mvn verify
```

The application can use `postgres-db` as the database hostname when its job and service network configuration supports that alias.

## 12.15 `services:name`, `alias`, `command`, `entrypoint`

```yaml
integration-test:
  stage: test

  services:

    - name: postgres:16

      # Service hostname.
      alias: database

      # Optional container command.
      command:
        - postgres

      # Optional container entrypoint override.
      entrypoint:
        - docker-entrypoint.sh

  script:
    - mvn verify
```

## 12.16 `services:variables`

```yaml
integration-test:
  stage: test

  services:
    - name: postgres:16
      alias: database

      # Variables supplied to the service container.
      variables:
        POSTGRES_DB: appdb
        POSTGRES_USER: appuser
        POSTGRES_PASSWORD: testpassword

  script:
    - mvn verify
```

## 12.17 `services:pull_policy`

```yaml
integration-test:
  stage: test

  services:
    - name: postgres:16

      # Control service image pulling.
      pull_policy: if-not-present

  script:
    - mvn verify
```

## 12.18 `services:docker`

Where supported, `services:docker` provides additional Docker-specific service configuration.

```yaml
integration-test:
  stage: test

  services:
    - name: postgres:16
      alias: database

      # Docker-specific service configuration.
      docker:
        platform: linux/amd64

  script:
    - mvn verify
```

## 12.19 `container`

`container` configures a container execution environment for supported job configurations.

```yaml
build-java:
  stage: build

  # Use a container image for the job.
  image: maven:3.9-eclipse-temurin-17

  script:
    - mvn clean package
```

**Note:** GitLab's normal CI job container is configured with `image`. Do not confuse this with the separate Kubernetes executor configuration or Docker-in-Docker service setup.

## 12.20 `pages`

`pages` configures a job to publish supported static website content using GitLab Pages.

```yaml
publish-documentation:
  stage: deploy

  script:
    - mkdir -p public
    - echo "<h1>Java API Documentation</h1>" > public/index.html

  pages:
    publish: public

  artifacts:
    paths:
      - public/
```

---

# 13. ♻️ Includes, YAML Anchors & Reusable Components

This section explains how to organize large CI/CD configurations, reuse common job definitions, and create modular pipelines.

## 13.1 Why Reusable Configuration?

Reusable configuration reduces duplicated YAML, improves consistency, and makes large CI/CD pipelines easier to maintain.

## 13.2 `include:local`

```yaml
# Main .gitlab-ci.yml

include:
  - local: '/ci/build.yml'
  - local: '/ci/test.yml'
  - local: '/ci/deploy.yml'
```

Example `ci/build.yml`:

```yaml
stages:
  - build

build-java:
  stage: build
  image: maven:3.9-eclipse-temurin-17

  script:
    - mvn clean package -DskipTests
```

## 13.3 `include:project`

```yaml
include:
  - project: 'devops/shared-pipelines'

    # Select the configuration branch or tag.
    ref: main

    # Configuration file path.
    file: '/templates/java-build.yml'
```

## 13.4 `include:remote`

```yaml
include:
  - remote: 'https://example.com/gitlab/java-template.yml'
```

Only include trusted remote configurations and apply appropriate integrity and version controls.

## 13.5 `include:template`

```yaml
include:
  # Import a predefined GitLab template.
  - template: Jobs/Secret-Detection.gitlab-ci.yml
```

The exact available templates depend on GitLab version and product configuration.

## 13.6 `include:component`

```yaml
include:
  - component: $CI_SERVER_FQDN/devops/components/java-build@1.0
```

A CI/CD component is a versioned reusable configuration unit that can be shared across projects.

## 13.7 `extends`

`extends` allows a job to inherit configuration from another job or hidden template.

```yaml
# Hidden template job.
.java-template:
  image: maven:3.9-eclipse-temurin-17

  before_script:
    - java -version
    - mvn -version

# Actual build job.
build-java:
  extends: .java-template

  stage: build

  script:
    - mvn clean package
```

## 13.8 Multiple `extends`

```yaml
.base-template:
  image: eclipse-temurin:17

.java-template:
  before_script:
    - java -version

.maven-template:
  before_script:
    - mvn -version

build-java:
  extends:
    - .base-template
    - .java-template
    - .maven-template

  stage: build

  script:
    - mvn clean package
```

**Important:** GitLab uses reverse deep merging for `extends`; values for the same key are not automatically concatenated in every case.

## 13.9 Hidden Jobs

A hidden job begins with a dot and is generally used as a reusable configuration template rather than an ordinary executable job.

```yaml
# Hidden reusable configuration.
.java-build:
  image: maven:3.9-eclipse-temurin-17

  variables:
    MAVEN_OPTS: "-Dmaven.repo.local=.m2/repository"

# Actual job.
build-java:
  extends: .java-build

  stage: build

  script:
    - mvn clean package
```

## 13.10 YAML Anchors

YAML anchors allow configuration blocks to be reused within the same YAML document.

```yaml
# Define a reusable command list.
.java-setup: &java-setup
  - java -version
  - mvn -version

build-java:
  stage: build

  before_script:
    # Reuse the anchored commands.
    - *java-setup

  script:
    - mvn clean package
```

## 13.11 YAML Merge Key

The YAML merge key `<<` can reuse mapping values from an anchor.

```yaml
# Define a reusable configuration mapping.
.default-java: &default-java
  image: maven:3.9-eclipse-temurin-17
  timeout: 20 minutes

build-java:
  # Merge reusable configuration.
  <<: *default-java

  stage: build

  script:
    - mvn clean package

test-java:
  # Reuse the same configuration.
  <<: *default-java

  stage: test

  script:
    - mvn test
```

## 13.12 `!reference`

`!reference` reuses supported configuration sections from another job or configuration location.

```yaml
# Hidden reusable setup.
.setup:
  script:
    - java -version
    - mvn -version

build-java:
  stage: build

  before_script:
    # Reuse the setup script section.
    - !reference [.setup, script]

  script:
    - mvn clean package
```

Validate references against the current GitLab CI/CD configuration rules, particularly when using `include`.

## 13.13 `extends` vs YAML Anchors vs `!reference`

| Feature | `extends` | YAML Anchors | `!reference` |
|---|---|---|---|
| Purpose | Inherits reusable job configuration. | Reuses YAML values and mappings. | Reuses selected GitLab configuration sections. |
| Scope | Job configuration. | YAML document scope. | Supported GitLab configuration scope. |
| Readability | Usually clear for reusable jobs. | Useful for compact YAML reuse. | Useful for selected configuration reuse. |
| Merge behavior | Uses GitLab reverse deep merge. | Uses YAML alias and merge behavior. | Copies the selected referenced configuration. |

## 13.14 `trigger:include` Child Pipeline

```yaml
# Parent pipeline.
stages:
  - build
  - deploy

build-java:
  stage: build
  script:
    - mvn clean package

child-deployment:
  stage: deploy

  trigger:
    # Child pipeline file.
    include:
      - local: '/ci/child-deploy.yml'

    # Wait for child pipeline result.
    strategy: mirror
```

Child configuration:

```yaml
# /ci/child-deploy.yml

stages:
  - deploy

deploy-java:
  stage: deploy

  script:
    - echo "Child pipeline deployment"
```

## 13.15 `trigger:forward`

`forward` controls which supported pipeline variables are forwarded to a downstream pipeline.

```yaml
trigger-child:
  stage: deploy

  trigger:
    include: '/ci/child.yml'

    # Forward supported variable categories.
    forward:
      yaml_variables: true
      pipeline_variables: false
```

---

# 14. 🛡️ Security, Best Practices & Common Mistakes

This section covers security controls, credential protection, runner safety, configuration validation, and common GitLab CI/CD mistakes.

## 14.1 Least Privilege

Give pipeline jobs only the permissions required to complete their tasks.

- Avoid sharing administrator credentials with ordinary jobs.
- Use separate deployment credentials for different environments.
- Prefer short-lived identity credentials where supported.
- Restrict protected variables and protected environments.

## 14.2 Never Hardcode Secrets

❌ Incorrect:

```yaml
deploy-job:
  script:
    - aws configure set aws_secret_access_key "MY_SECRET_KEY"
```

✅ Better:

```yaml
deploy-job:
  script:
    # Use a securely configured GitLab variable.
    - test -n "$AWS_SECRET_ACCESS_KEY"
    - echo "AWS credentials are configured"
```

Do not print credentials into pipeline logs.

## 14.3 Use Protected Variables

Store production credentials as protected GitLab CI/CD variables and restrict their availability to protected branches or tags.

## 14.4 Prefer OIDC Where Supported

Use GitLab ID tokens with a correctly configured AWS IAM OIDC provider and role trust policy instead of long-lived AWS access keys where practical.

## 14.5 Protect Production Environments

```yaml
deploy-production:
  stage: deploy

  environment:
    name: production

  when: manual

  script:
    - echo "Production deployment"
```

Configure protected environment permissions in GitLab settings.

## 14.6 Avoid Untrusted Pipeline Execution

Be careful when running jobs for external contributions, untrusted branches, or merge request pipelines.

Never expose privileged production secrets to untrusted code.

## 14.7 Pin Container Images

Prefer stable, versioned container images instead of relying on mutable `latest` tags for production pipelines.

```yaml
build-java:
  image: maven:3.9-eclipse-temurin-17

  script:
    - mvn clean package
```

## 14.8 Validate CI/CD Configuration

Use GitLab CI Lint to detect invalid YAML and configuration errors before relying on a pipeline run.

- [GitLab CI Lint](https://docs.gitlab.com/ci/yaml/lint/)

## 14.9 Avoid Unnecessary Pipeline Execution

Use `workflow:rules` and job-level `rules` to prevent duplicate pipelines and unnecessary jobs.

## 14.10 Use Cache Correctly

Cache dependencies such as Maven's local repository, not every generated output indiscriminately.

```yaml
cache:
  key:
    files:
      - pom.xml

  paths:
    - .m2/repository/
```

## 14.11 Use Artifacts for Build Outputs

```yaml
build-java:
  stage: build

  script:
    - mvn clean package -DskipTests

  artifacts:
    paths:
      - target/*.jar
```

## 14.12 Use `needs` Carefully

Explicit dependencies can speed up pipelines, but every required artifact must be available to downstream jobs.

## 14.13 Common Mistakes

| Mistake | Correct Approach |
|---|---|
| Incorrect YAML indentation | Maintain consistent indentation and validate the configuration. |
| Missing `stages` entry | Define every custom stage before assigning jobs to it. |
| Missing `script` | Add a script or a supported trigger configuration to normal jobs. |
| Wrong image | Select an image containing the required Java and Maven tools. |
| Missing artifact paths | Configure paths that actually exist after the build completes. |
| Incorrect cache paths | Cache the actual Maven repository directory used by the job. |
| Hardcoded credentials | Store secrets in GitLab's supported secure variable settings. |
| Unnecessary `latest` images | Pin predictable image versions for reproducible execution. |
| Incorrect `needs` dependency | Reference a job that exists in the current pipeline. |
| Mixing `needs` and `dependencies` | Prefer `needs:artifacts` for explicit dependency and artifact control. |
| Exposing production secrets | Restrict protected variables and protected environments. |
| Duplicate pipelines | Configure workflow rules for intended pipeline sources. |
| Using outdated `only` and `except` | Prefer the more flexible `rules` configuration. |
| Running deployment on every branch | Restrict deployment jobs using branch and environment rules. |

## 14.14 Production Checklist

- [ ] Use protected branches and tags.
- [ ] Store credentials in GitLab CI/CD settings.
- [ ] Use least-privilege AWS IAM permissions.
- [ ] Prefer OIDC-based authentication where supported.
- [ ] Protect production deployment environments.
- [ ] Pin container image versions.
- [ ] Validate pipeline configuration.
- [ ] Store JAR files as artifacts where required.
- [ ] Cache Maven dependencies appropriately.
- [ ] Restrict runner access to trusted projects.
- [ ] Avoid printing secrets in logs.
- [ ] Keep deployment and build jobs logically separated.

---

# 15. 🏗️ Complete Java + Docker + AWS EC2 CI/CD Pipeline

This section combines the concepts into a practical Java Spring Boot pipeline that builds, tests, packages, containerizes, and deploys an application to AWS EC2.

## 15.1 Project Architecture

```text
               👨‍💻 Developer
                    |
                    | git push
                    v
             📦 GitLab Repository
                    |
                    v
              .gitlab-ci.yml
                    |
                    v
               🚀 Pipeline
                    |
          +---------+---------+
          |                   |
          v                   v
      🏗️ Build              🔍 Test
          |                   |
          +---------+---------+
                    |
                    v
               📦 Package
                    |
                    v
                app.jar
                    |
                    v
              🐳 Docker Build
                    |
                    v
             📦 Container Registry
                    |
                    v
               ☁️ AWS EC2
                    |
                    v
              🐳 Docker Run
                    |
                    v
            🌐 Spring Boot API
```

## 15.2 Java Application Structure

```text
SpringBoot-Application/
│
├── .gitlab-ci.yml
│
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/example/
│   │   │       └── Application.java
│   │   │
│   │   └── resources/
│   │
│   └── test/
│       └── java/
│
├── pom.xml
│
├── Dockerfile
│
└── README.md
```

## 15.3 Maven Configuration

Example `pom.xml`:

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
         https://maven.apache.org/xsd/maven-4.0.0.xsd">

    <modelVersion>4.0.0</modelVersion>

    <groupId>com.example</groupId>
    <artifactId>spring-boot-api</artifactId>
    <version>1.0.0</version>

    <properties>
        <java.version>17</java.version>
        <maven.compiler.source>17</maven.compiler.source>
        <maven.compiler.target>17</maven.compiler.target>
    </properties>

</project>
```

This is a minimal Maven configuration example. A real Spring Boot application should also declare its Spring Boot parent or dependency management and required dependencies.

## 15.4 Dockerfile

```dockerfile
# Use a Java 17 runtime image.
FROM eclipse-temurin:17-jre

# Set the working directory inside the container.
WORKDIR /app

# Copy the generated application JAR.
COPY target/*.jar app.jar

# Document the application port.
EXPOSE 8080

# Start the Spring Boot application.
ENTRYPOINT ["java", "-jar", "app.jar"]
```

## 15.5 GitLab CI/CD Variables

Configure these values under:

```text
GitLab Project
    |
    v
Settings
    |
    v
CI/CD
    |
    v
Variables
```

| Variable | Example | Protected |
|---|---|---|
| `AWS_DEFAULT_REGION` | `ap-south-1` | Optional |
| `AWS_ROLE_ARN` | AWS IAM role ARN | Yes |
| `EC2_HOST` | EC2 public IP or DNS | Yes |
| `EC2_USER` | `ubuntu` | Yes |
| `EC2_SSH_PRIVATE_KEY` | SSH private key | Yes |
| `CI_REGISTRY_USER` | GitLab registry username | Use predefined variable where available |
| `CI_REGISTRY_PASSWORD` | GitLab registry password | Use predefined variable where available |

Use GitLab OIDC with AWS IAM for production authentication where practical. The SSH example below is a simplified deployment approach.

## 15.6 Complete `.gitlab-ci.yml`

```yaml
# ==========================================================
# 🚀 GITLAB CI/CD PIPELINE
# Java Spring Boot + Maven + Docker + AWS EC2
# ==========================================================


# ----------------------------------------------------------
# 1. GLOBAL PIPELINE CONFIGURATION
# ----------------------------------------------------------

# Define pipeline stages in execution order.
stages:
  - build
  - test
  - package
  - docker
  - deploy


# ----------------------------------------------------------
# 2. GLOBAL VARIABLES
# ----------------------------------------------------------

variables:

  # Java application configuration.
  JAVA_VERSION: "17"

  # Maven local dependency repository.
  MAVEN_OPTS: "-Dmaven.repo.local=$CI_PROJECT_DIR/.m2/repository"

  # Docker image name.
  APP_NAME: "spring-boot-api"

  # AWS deployment region.
  AWS_DEFAULT_REGION: "ap-south-1"

  # GitLab registry image path.
  DOCKER_IMAGE: "$CI_REGISTRY_IMAGE"


# ----------------------------------------------------------
# 3. DEFAULT CONFIGURATION
# ----------------------------------------------------------

default:

  # Default Maven and Java environment.
  image: maven:3.9-eclipse-temurin-17

  # Common setup commands.
  before_script:
    - java -version
    - mvn -version

  # Retry selected runner failures.
  retry:
    max: 2
    when:
      - runner_system_failure
      - stuck_or_timeout_failure


# ----------------------------------------------------------
# 4. GLOBAL CACHE
# ----------------------------------------------------------

cache:

  # Generate a cache key from Maven configuration.
  key:
    files:
      - pom.xml

  # Cache Maven downloaded dependencies.
  paths:
    - .m2/repository/


# ----------------------------------------------------------
# 5. PIPELINE WORKFLOW
# ----------------------------------------------------------

workflow:

  rules:

    # Run pipelines for merge requests.
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'

    # Run pipelines for main branch pushes.
    - if: '$CI_COMMIT_BRANCH == "main"'

    # Run pipelines for develop branch pushes.
    - if: '$CI_COMMIT_BRANCH == "develop"'

    # Run pipelines for Git tags.
    - if: '$CI_COMMIT_TAG'

    # Reject other pipeline sources.
    - when: never


# ----------------------------------------------------------
# 6. BUILD JOB
# ----------------------------------------------------------

build-java:

  # Assign job to build stage.
  stage: build

  # Job execution image.
  image: maven:3.9-eclipse-temurin-17

  # Job timeout.
  timeout: 20 minutes

  script:

    # Display Java version.
    - java -version

    # Display Maven version.
    - mvn -version

    # Compile Java source code.
    - mvn clean compile

  # Save compiled output if required by later jobs.
  artifacts:
    paths:
      - target/classes/

    expire_in: 1 day


# ----------------------------------------------------------
# 7. TEST JOB
# ----------------------------------------------------------

test-java:

  stage: test

  image: maven:3.9-eclipse-temurin-17

  # Wait for build job.
  needs:
    - job: build-java
      artifacts: true

  script:

    # Execute Java unit tests.
    - mvn test

  # Store JUnit test reports.
  artifacts:

    when: always

    reports:
      junit:
        - target/surefire-reports/TEST-*.xml

    paths:
      - target/surefire-reports/

    expire_in: 7 days


# ----------------------------------------------------------
# 8. PACKAGE JOB
# ----------------------------------------------------------

package-java:

  stage: package

  image: maven:3.9-eclipse-temurin-17

  # Wait for successful test execution.
  needs:
    - job: test-java
      artifacts: false

  script:

    # Create executable JAR.
    - mvn clean package -DskipTests

    # Verify generated JAR.
    - ls -lh target/*.jar

  # Save application JAR.
  artifacts:
    paths:
      - target/*.jar

    expire_in: 7 days


# ----------------------------------------------------------
# 9. DOCKER BUILD JOB
# ----------------------------------------------------------

docker-build:

  stage: docker

  # Docker CLI image.
  image: docker:27-cli

  # Start Docker daemon as a service.
  services:
    - name: docker:27-dind
      alias: docker

  # Docker connection configuration.
  variables:
    DOCKER_HOST: "tcp://docker:2375"
    DOCKER_TLS_CERTDIR: ""

  # Download application artifact.
  needs:
    - job: package-java
      artifacts: true

  script:

    # Build Docker image.
    - docker build -t "$DOCKER_IMAGE:$CI_COMMIT_SHORT_SHA" .

    # Authenticate with GitLab Container Registry.
    - echo "$CI_REGISTRY_PASSWORD" | docker login "$CI_REGISTRY" -u "$CI_REGISTRY_USER" --password-stdin

    # Push image to GitLab Container Registry.
    - docker push "$DOCKER_IMAGE:$CI_COMMIT_SHORT_SHA"

  rules:

    # Build and push Docker image only for main.
    - if: '$CI_COMMIT_BRANCH == "main"'

  # Note: This example requires a runner configured to support Docker-in-Docker.


# ----------------------------------------------------------
# 10. DEPLOYMENT JOB
# ----------------------------------------------------------

deploy-production:

  stage: deploy

  # Use an image containing SSH and AWS CLI tools.
  image: amazon/aws-cli:2

  # Wait for Docker image build.
  needs:
    - docker-build

  # Serialize production deployments.
  resource_group: production

  # Associate the job with production environment.
  environment:
    name: production
    url: https://example.com

  # Require manual production approval.
  when: manual

  # Run only for main branch.
  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'
      when: manual

  before_script:

    # Install SSH client in supported image environments.
    - yum install -y openssh-clients

    # Prepare SSH directory.
    - mkdir -p ~/.ssh
    - chmod 700 ~/.ssh

    # Write private key to file.
    - printf '%s\n' "$EC2_SSH_PRIVATE_KEY" > ~/.ssh/id_rsa

    # Restrict private key permissions.
    - chmod 600 ~/.ssh/id_rsa

    # Add EC2 host key.
    - ssh-keyscan -H "$EC2_HOST" >> ~/.ssh/known_hosts

  script:

    # Pull the published application image on EC2.
    - >
      ssh -i ~/.ssh/id_rsa "$EC2_USER@$EC2_HOST"
      "docker pull $DOCKER_IMAGE:$CI_COMMIT_SHORT_SHA"

    # Stop the previous application container if present.
    - >
      ssh -i ~/.ssh/id_rsa "$EC2_USER@$EC2_HOST"
      "docker stop spring-api || true"

    # Remove the previous application container if present.
    - >
      ssh -i ~/.ssh/id_rsa "$EC2_USER@$EC2_HOST"
      "docker rm spring-api || true"

    # Start the new application container.
    - >
      ssh -i ~/.ssh/id_rsa "$EC2_USER@$EC2_HOST"
      "docker run -d
      --name spring-api
      --restart unless-stopped
      -p 8080:8080
      $DOCKER_IMAGE:$CI_COMMIT_SHORT_SHA"

  # Deployment should not run for untrusted branches.
  allow_failure: false
```

**Important implementation notes:**

1. The example assumes a GitLab Runner that supports Docker-in-Docker for the image build.
2. The runner must have the appropriate privileged configuration for Docker-in-Docker where required.
3. The EC2 instance must have Docker installed and running.
4. The EC2 instance must be able to authenticate to the GitLab Container Registry before pulling a private image. Configure a suitable deploy token or another supported authentication method.
5. The EC2 security group must permit the required application traffic.
6. SSH host verification should use trusted, pre-provisioned host keys in production rather than blindly trusting scanned keys.
7. The deployment example uses SSH for simplicity. AWS Systems Manager Session Manager with suitable IAM permissions is a strong alternative that avoids opening inbound SSH access.
8. The example uses an AWS CLI image for the deployment job and installs an SSH client. Verify package-manager compatibility with the selected image before running it.
9. Configure protected variables and production environment permissions before using this pipeline in a real project.
10. For production, use a controlled deployment strategy with health checks and rollback rather than immediately replacing the running container.

## 15.7 Pipeline Execution Flow

| Order | Job | Stage | Task |
|---|---|---|---|
| 1 | `build-java` | build | Compiles Java source code. |
| 2 | `test-java` | test | Executes unit tests and publishes JUnit reports. |
| 3 | `package-java` | package | Generates the executable Spring Boot JAR. |
| 4 | `docker-build` | docker | Builds and pushes the Docker image to GitLab Registry. |
| 5 | `deploy-production` | deploy | Pulls the image and deploys the container to AWS EC2. |

## 15.8 Final Pipeline Flow

```text
                 👨‍💻 Developer
                      |
                      v
                 git push
                      |
                      v
              📦 GitLab Repository
                      |
                      v
               🚀 GitLab Pipeline
                      |
                      v
                🏗️ BUILD JAVA
                      |
                      v
                 🔍 TEST JAVA
                      |
                      v
               📦 PACKAGE JAR
                      |
                      v
                🐳 DOCKER BUILD
                      |
                      v
           📦 GitLab Container Registry
                      |
                      v
             👤 Manual Approval
                      |
                      v
              ☁️ AWS EC2 Deployment
                      |
                      v
               🐳 Docker Container
                      |
                      v
              🌐 Spring Boot API
```

## 15.9 Quick Reference – Important GitLab CI/CD Keywords

| Keyword | Location | Purpose |
|---|---|---|
| `stages` | Global | Defines pipeline stage order. |
| `workflow` | Global | Controls pipeline creation. |
| `rules` | Job / Workflow | Controls pipeline or job conditions. |
| `variables` | Global / Job | Defines environment variables. |
| `default` | Global | Defines reusable default job settings. |
| `include` | Global | Imports external configuration. |
| `image` | Default / Job | Defines the execution container image. |
| `services` | Default / Job | Starts supporting service containers. |
| `stage` | Job | Assigns a job to a stage. |
| `script` | Job | Defines commands executed by a job. |
| `before_script` | Global / Job | Executes setup commands. |
| `after_script` | Global / Job | Executes cleanup commands. |
| `tags` | Job | Selects compatible runners. |
| `timeout` | Job | Sets job execution timeout. |
| `retry` | Job | Configures automatic retries. |
| `allow_failure` | Job | Allows non-blocking job failures. |
| `when` | Job / Rules | Controls job execution timing. |
| `needs` | Job | Defines explicit job dependencies. |
| `dependencies` | Job | Selects downloaded artifacts from earlier jobs. |
| `artifacts` | Job / Default | Stores generated files and reports. |
| `cache` | Job / Default | Reuses files between compatible executions. |
| `environment` | Job | Associates deployment with an environment. |
| `resource_group` | Job | Controls concurrent access to resources. |
| `extends` | Job | Inherits reusable job configuration. |
| `inherit` | Job | Controls inherited global configuration. |
| `parallel` | Job | Creates multiple job instances. |
| `coverage` | Job | Extracts coverage information from logs. |
| `release` | Job | Creates a GitLab release. |
| `trigger` | Job | Starts a downstream pipeline. |
| `id_tokens` | Job | Requests identity tokens for authentication. |
| `secrets` | Job | Requests supported external secret values. |
| `identity` | Job | Configures supported workload identity federation. |
| `pages` | Job | Publishes static website content. |

---

# 📚 Official Documentation & References

Use these official resources to verify syntax, supported keywords, and version-specific behavior.

1. [GitLab CI/CD Documentation](https://docs.gitlab.com/ci/)
2. [CI/CD YAML Syntax Reference](https://docs.gitlab.com/ci/yaml/)
3. [GitLab CI/CD Variables](https://docs.gitlab.com/ci/variables/)
4. [GitLab Pipeline Basics](https://docs.gitlab.com/ci/pipelines/)
5. [GitLab CI/CD Jobs](https://docs.gitlab.com/ci/jobs/)
6. [GitLab CI/CD Artifacts](https://docs.gitlab.com/ci/jobs/job_artifacts/)
7. [GitLab CI/CD Caching](https://docs.gitlab.com/ci/caching/)
8. [GitLab Runner Documentation](https://docs.gitlab.com/runner/)
9. [GitLab CI/CD Components](https://docs.gitlab.com/ci/components/)
10. [GitLab CI Lint](https://docs.gitlab.com/ci/yaml/lint/)
11. [GitLab OIDC with AWS](https://docs.gitlab.com/ci/cloud_services/aws/)
12. [GitLab CI/CD Examples](https://docs.gitlab.com/ci/examples/)



</p>
