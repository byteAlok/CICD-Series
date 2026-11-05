 <div align="center">

# 🚀 Azure DevOps CI/CD – Complete Pipeline Keywords & Syntax Reference

📚 **Documentation Type:** A beginner-friendly reference for Azure DevOps YAML pipelines, stages, jobs, tasks, variables, conditions, templates, and automation.

![Azure DevOps](https://img.shields.io/badge/Azure%20DevOps-CI%2FCD-0078D4?logo=azuredevops&logoColor=white)
![YAML](https://img.shields.io/badge/Config-YAML-CB171E?logo=yaml&logoColor=white)
![Level](https://img.shields.io/badge/Level-Beginner%20to%20Advanced-success)
![.NET](https://img.shields.io/badge/.NET-8.0-512BD4?logo=dotnet&logoColor=white)
![ASP.NET Core](https://img.shields.io/badge/ASP.NET%20Core-Web%20API-512BD4?logo=dotnet&logoColor=white)
![Azure](https://img.shields.io/badge/Azure-Deployment-0078D4?logo=microsoftazure&logoColor=white)

</div>

---

> 🛠️ **Technology:** Azure DevOps Pipelines + YAML  
> 🎯 **Focus:** Pipeline keywords, stages, jobs, agents, tasks, variables, conditions, artifacts, caching, templates, security, and deployment  
> 💻 **Examples:** ASP.NET Core Web API + .NET 8, 9 or 10 + NuGet + Azure App Service  
> 👨‍💻 **Maintained by:** byteAlok | Cloud and DevOps Learner  
> 🧪 **Examples:** Dummy repository names and placeholder values only

---

# 📚 Table of Contents

1. [📖 Introduction & Azure Pipeline File Structure](#1--introduction--azure-pipeline-file-structure)
2. [🧱 YAML Basics & Pipeline Anatomy](#2--yaml-basics--pipeline-anatomy)
3. [🚀 Pipeline Triggers & Run Configuration](#3--pipeline-triggers--run-configuration)
4. [⚙️ Global Keywords & Pipeline Configuration](#4--global-keywords--pipeline-configuration)
5. [🖥️ Agents, Pools & Execution Environments](#5--agents-pools--execution-environments)
6. [🧩 Stages & Stage-Level Keywords](#6--stages--stage-level-keywords)
7. [🪜 Jobs & Job-Level Keywords](#7--jobs--job-level-keywords)
8. [🔌 Steps, Tasks & Scripts](#8--steps-tasks--scripts)
9. [🔐 Variables, Parameters & Secrets](#9--variables-parameters--secrets)
10. [🧠 Expressions, Conditions & Runtime Logic](#10--expressions-conditions--runtime-logic)
11. [🔀 Dependencies, Outputs & Job Communication](#11--dependencies-outputs--job-communication)
12. [📦 Artifacts, Cache, Containers & Services](#12--artifacts-cache-containers--services)
13. [♻️ Templates, Reusability & Advanced YAML](#13--templates-reusability--advanced-yaml)
14. [🛡️ Security, Environments & Best Practices](#14--security-environments--best-practices)
15. [🏗️ Complete .NET CI/CD Pipeline – Azure App Service Deployment](#15--complete-net-cicd-pipeline--azure-app-service-deployment)

---

# 1. 📖 Introduction & Azure Pipeline File Structure

This section introduces Azure DevOps Pipelines, explains how CI/CD works, and shows where pipeline YAML files are stored in a repository.

## 🔹 What is Azure DevOps?

Azure DevOps is a Microsoft platform that provides tools for planning, developing, building, testing, and deploying software applications.

It includes services such as:

| Service | Purpose |
|---|---|
| 🗂️ Azure Boards | Work items, tasks, bugs, and project planning |
| 📁 Azure Repos | Git repositories and source code management |
| 🚀 Azure Pipelines | Continuous Integration and Continuous Delivery |
| 🧪 Azure Test Plans | Manual and exploratory testing |
| 📦 Azure Artifacts | Package and dependency management |

## 🔹 What is Azure Pipelines?

Azure Pipelines is a CI/CD service that automates application building, testing, packaging, and deployment using YAML or Classic pipeline configuration.

### CI/CD workflow

```text
Developer
   |
   | Push Code
   v
Azure Repos / GitHub
   |
   v
Azure Pipelines
   |
   +----------------------+
   |                      |
   v                      v
Continuous            Continuous
Integration           Delivery
   |                      |
   v                      v
Restore Packages       Deployment
   |
   v
Build Application
   |
   v
Run Tests
   |
   v
Publish Artifact
   |
   v
Deploy Application
   |
   v
Azure App Service
```

## 🔹 What is a YAML Pipeline?

A YAML pipeline is a pipeline definition written in a YAML file that describes triggers, agents, stages, jobs, tasks, variables, and deployment steps.

The default filename is:

```text
azure-pipelines.yml
```

## 🔹 Basic repository structure

```text
MyDotNetApplication/
│
├── src/
│   └── MyWebApi/
│       ├── Controllers/
│       ├── Models/
│       ├── Program.cs
│       └── MyWebApi.csproj
│
├── tests/
│   └── MyWebApi.Tests/
│       └── MyWebApi.Tests.csproj
│
├── azure-pipelines.yml
│
└── README.md
```

## 🔹 Azure Pipeline configuration location

By default, Azure DevOps searches for the YAML file at the repository root.

```text
Repository
   |
   ├── src/
   ├── tests/
   └── azure-pipelines.yml
```

You can also store it in a subdirectory:

```text
Repository
   |
   ├── .azure-pipelines/
   │   └── azure-pipelines.yml
   │
   ├── src/
   └── tests/
```

When creating the pipeline, select the corresponding YAML path.

## 🔹 Important Azure Pipelines concepts

| Concept | Definition |
|---|---|
| Pipeline | A pipeline is an automated workflow that builds, tests, and deploys an application. |
| Agent | An agent is a machine or execution environment that runs pipeline jobs and tasks. |
| Agent Pool | An agent pool is a collection of agents available to execute pipeline jobs. |
| Stage | A stage is a logical division of a pipeline representing a major delivery phase. |
| Job | A job is a group of steps executed together on an agent or server. |
| Step | A step is an individual operation performed inside a job. |
| Task | A task is a reusable predefined action that performs a specific pipeline operation. |
| Artifact | An artifact is a file or package produced by a pipeline and made available for later use. |
| Environment | An environment represents a deployment target such as Development, Staging, or Production. |
| Service Connection | A service connection securely connects Azure Pipelines to an external service or Azure subscription. |

## 🔹 Azure DevOps pipeline types

| Type | Description |
|---|---|
| YAML Pipeline | Pipeline configuration is maintained as code in a YAML file. |
| Classic Build Pipeline | A visual interface is used to configure build and integration steps. |
| Classic Release Pipeline | A visual release workflow is used to deploy application artifacts. |

**Recommendation:** Prefer YAML pipelines because they support version control, reusable templates, code reviews, and repeatable automation.

---

# 2. 🧱 YAML Basics & Pipeline Anatomy

This section covers YAML syntax, indentation, lists, mappings, strings, and the basic structure of an Azure DevOps pipeline.

## 🔹 YAML syntax

YAML is a human-readable data serialization format that uses indentation, key-value pairs, lists, and nested structures.

### 1. Key-value pairs

```yaml
name: MyPipeline
project: MyDotNetApplication
framework: net8.0
```

### 2. Lists

```yaml
branches:
  - main
  - development
  - feature/*
```

### 3. Nested mappings

```yaml
pool:
  vmImage: ubuntu-latest
```

### 4. Boolean values

```yaml
enabled: true
continueOnError: false
```

### 5. Comments

```yaml
# This is a YAML comment
name: MyPipeline
```

### 6. Multiline scripts

```yaml
steps:
  - script: |
      echo "Starting build"
      echo "Restoring packages"
      echo "Build completed"
    displayName: "Run multiple commands"
```

## 🔹 Pipeline hierarchy

An Azure DevOps pipeline follows this hierarchy:

```text
Pipeline
   |
   v
Stages
   |
   v
Jobs
   |
   v
Steps
   |
   +---- Script
   |
   +---- Task
   |
   +---- Checkout
   |
   +---- Publish
```

### Example: Complete basic pipeline structure

```yaml
# Pipeline name
name: MyDotNetPipeline

# Trigger configuration
trigger:
  - main

# Agent configuration
pool:
  vmImage: ubuntu-latest

# Pipeline variables
variables:
  buildConfiguration: Release

# Stage configuration
stages:

  - stage: Build
    displayName: "Build Stage"

    # Jobs inside the stage
    jobs:

      - job: BuildJob
        displayName: "Build Application"

        # Steps inside the job
        steps:

          - script: echo "Starting build"
            displayName: "Start Message"

          - script: dotnet --version
            displayName: "Check .NET Version"
```

## 🔹 Important YAML rules

| Rule | Explanation |
|---|---|
| Indentation | YAML uses indentation to represent nested structures. |
| Spaces | Use spaces instead of tabs for consistent YAML formatting. |
| Colon | A colon separates a key from its corresponding value. |
| Hyphen | A hyphen represents an item inside a YAML list. |
| Comments | The `#` symbol is used to write comments in YAML files. |
| Case sensitivity | Azure Pipelines identifiers and values should be written with their documented casing. |

---

# 3. 🚀 Pipeline Triggers & Run Configuration

Triggers determine when Azure Pipelines starts automatically. You can configure branch triggers, path filters, scheduled execution, and pull request validation.

## 🔹 1. `trigger`

The `trigger` keyword defines which branch updates automatically start a pipeline.

```yaml
# Run when code is pushed to main
trigger:
  - main
```

Multiple branches:

```yaml
trigger:
  branches:
    include:
      - main
      - development
      - release/*
    exclude:
      - feature/experimental
```

## 🔹 2. `trigger: none`

Disables automatic continuous integration triggers.

```yaml
trigger: none
```

The pipeline can still be started manually or by other configured pipeline resources.

## 🔹 3. Branch filters

Branch filters restrict pipeline execution to selected branches.

```yaml
trigger:
  branches:
    include:
      - main
      - development

    exclude:
      - feature/*
```

## 🔹 4. Path filters

Path filters start a pipeline only when files inside specified paths change.

```yaml
trigger:
  branches:
    include:
      - main

  paths:
    include:
      - src/**
      - tests/**

    exclude:
      - README.md
      - docs/**
```

## 🔹 5. `pr`

The `pr` keyword configures pull request validation triggers for supported repository providers.

```yaml
# Example for GitHub or Bitbucket Cloud repositories
pr:
  branches:
    include:
      - main
      - development

  paths:
    include:
      - src/**
      - tests/**
```

> ⚠️ **Important:** Azure Repos Git does not use YAML `pr` triggers for pull request validation. Configure a build validation branch policy in Azure Repos instead.

## 🔹 6. Scheduled triggers

The `schedules` keyword defines cron-based pipeline execution.

```yaml
schedules:
  - cron: "0 2 * * 1-5"
    displayName: "Weekday Nightly Build"
    branches:
      include:
        - main
    always: true
```

**Cron format:**

```text
Minute Hour Day-of-Month Month Day-of-Week
```

Example:

```text
0 2 * * 1-5
```

This schedule runs at 02:00 UTC from Monday to Friday.

## 🔹 7. `always`

The `always` property controls whether a scheduled pipeline runs even when there have been no source changes since the last successful scheduled run.

```yaml
schedules:
  - cron: "0 0 * * *"
    displayName: "Daily Build"
    branches:
      include:
        - main

    always: true
```

## 🔹 8. `batch`

The `batch` property controls whether Azure Pipelines waits for a running CI pipeline to finish before starting another run for the same branch.

```yaml
trigger:
  batch: true

  branches:
    include:
      - main
```

## 🔹 Trigger keywords summary

| Keyword | Purpose |
|---|---|
| `trigger` | Configures continuous integration triggers for branch changes. |
| `trigger: none` | Disables automatic CI triggers for the pipeline. |
| `branches` | Defines branch names that can start a pipeline. |
| `include` | Includes matching branches or paths in trigger configuration. |
| `exclude` | Excludes matching branches or paths from trigger configuration. |
| `paths` | Filters pipeline triggers according to changed file paths. |
| `pr` | Configures pull request validation triggers for supported repository providers. |
| `schedules` | Defines scheduled pipeline execution using cron expressions. |
| `cron` | Specifies the date and time pattern for scheduled execution. |
| `always` | Allows scheduled execution even when source code has not changed. |
| `batch` | Batches CI changes while a previous run for the same branch is running. |

---

# 4. ⚙️ Global Keywords & Pipeline Configuration

Global keywords configure pipeline-level behavior such as naming, variables, resources, parameters, and default execution settings.

## 🔹 1. `name`

The `name` keyword defines the pipeline run number format displayed in Azure DevOps.

```yaml
name: MyDotNetPipeline_$(Date:yyyyMMdd).$(Rev:r)
```

Example output:

```text
MyDotNetPipeline_20261106.1
MyDotNetPipeline_20261106.2
```

## 🔹 2. `appendCommitMessageToRunName`

Controls whether the commit message is appended to the pipeline run name.

```yaml
appendCommitMessageToRunName: false
```

## 🔹 3. `pool`

Defines the default agent pool for pipeline jobs unless a stage or job overrides it.

```yaml
pool:
  vmImage: ubuntu-latest
```

## 🔹 4. `variables`

Defines pipeline-level variables that can be reused across stages and jobs.

```yaml
variables:
  buildConfiguration: Release
  solution: "**/*.sln"
  projectPath: "src/MyWebApi/MyWebApi.csproj"
```

Usage:

```yaml
steps:
  - script: dotnet build "$(projectPath)" --configuration $(buildConfiguration)
    displayName: "Build .NET Application"
```

## 🔹 5. `parameters`

Defines typed input values that can be supplied when a pipeline is started or invoked through a template.

```yaml
parameters:
  - name: buildConfiguration
    displayName: "Build Configuration"
    type: string
    default: Release

    values:
      - Debug
      - Release
```

Usage:

```yaml
steps:
  - script: dotnet build --configuration ${{ parameters.buildConfiguration }}
    displayName: "Build Application"
```

## 🔹 6. `resources`

Declares external resources that a pipeline can consume, such as repositories, pipeline artifacts, containers, and packages.

```yaml
resources:

  repositories:
    - repository: sharedTemplates
      type: git
      name: DevOpsProject/PipelineTemplates
      ref: refs/heads/main

  pipelines:
    - pipeline: buildPipeline
      source: MyApplication-CI
      trigger: true

  containers:
    - container: dotnetContainer
      image: mcr.microsoft.com/dotnet/sdk:8.0
```

> Note: Repository resource types and names depend on the connected provider and Azure DevOps project configuration.

## 🔹 7. `lockBehavior`

Controls how stages request an exclusive lock when using an exclusive lock check.

```yaml
lockBehavior: sequential
```

Common values:

| Value | Meaning |
|---|---|
| `sequential` | Runs waiting stages sequentially after the lock becomes available. |
| `runLatest` | Prioritizes the latest run and cancels older waiting runs where applicable. |

## 🔹 Global keywords summary

| Keyword | Purpose |
|---|---|
| `name` | Defines a readable and customizable pipeline run number. |
| `appendCommitMessageToRunName` | Controls commit-message text appended to the run name. |
| `pool` | Defines the default agent pool for pipeline execution. |
| `variables` | Defines reusable values available to pipeline stages and jobs. |
| `parameters` | Defines typed inputs evaluated during pipeline template expansion. |
| `resources` | Declares external repositories, pipelines, containers, and packages. |
| `lockBehavior` | Controls how exclusive stage locks are handled between pipeline runs. |

---

# 5. 🖥️ Agents, Pools & Execution Environments

Agents execute pipeline jobs. Azure DevOps supports Microsoft-hosted agents, self-hosted agents, and different execution environments.

## 🔹 What is an Agent?

An agent is a machine or execution environment that receives pipeline jobs and executes their steps using installed tools and configured permissions.

## 🔹 1. Microsoft-hosted agents

Microsoft-hosted agents are managed virtual machines provided by Microsoft that execute jobs without requiring users to maintain the machine.

```yaml
pool:
  vmImage: ubuntu-latest
```

Other common images:

```yaml
pool:
  vmImage: windows-latest
```

```yaml
pool:
  vmImage: macos-latest
```

### Common agent images

| Image | Operating System |
|---|---|
| `ubuntu-latest` | Latest supported Ubuntu image |
| `ubuntu-24.04` | Ubuntu 24.04 |
| `windows-latest` | Latest supported Windows image |
| `windows-2022` | Windows Server 2022 |
| `macos-latest` | Latest supported macOS image |

> Microsoft updates hosted images over time. Pin a specific image when reproducibility is important.

## 🔹 2. Self-hosted agents

Self-hosted agents are machines managed by your organization that execute pipeline jobs using locally installed tools and configured permissions.

```yaml
pool:
  name: MySelfHostedPool
```

You can also target an agent using demands:

```yaml
pool:
  name: MySelfHostedPool

  demands:
    - Agent.OS -equals Linux
    - dotnet
```

## 🔹 3. Agent pool

An agent pool is a logical collection of agents that allows Azure DevOps to assign jobs to available execution machines.

```yaml
pool:
  name: Default
```

## 🔹 4. Job-level pool override

A job can use a different agent pool from the pipeline-level default.

```yaml
pool:
  vmImage: ubuntu-latest

jobs:

  - job: LinuxBuild
    pool:
      vmImage: ubuntu-latest
    steps:
      - script: echo "Linux Build"

  - job: WindowsBuild
    pool:
      vmImage: windows-latest
    steps:
      - script: echo "Windows Build"
```

## 🔹 5. `demands`

The `demands` keyword selects self-hosted agents based on their capabilities.

```yaml
pool:
  name: MySelfHostedPool

  demands:
    - Agent.OS -equals Linux
    - Agent.Version -gtVersion 3.0
```

Demands can be useful when jobs require a particular installed tool or machine capability.

## 🔹 6. `workspace`

The `workspace` keyword controls how the agent workspace is cleaned before a job starts.

```yaml
jobs:

  - job: BuildJob

    workspace:
      clean: all

    steps:
      - script: echo "Workspace cleaned"
```

Common values:

| Value | Purpose |
|---|---|
| `outputs` | Cleans pipeline output directories. |
| `resources` | Cleans source and other resource directories. |
| `all` | Cleans the entire job workspace. |

## 🔹 Agent keywords summary

| Keyword | Purpose |
|---|---|
| `pool` | Selects the agent pool used to execute a pipeline job. |
| `vmImage` | Selects a Microsoft-hosted virtual machine image. |
| `name` | Selects a named agent pool or self-hosted pool. |
| `demands` | Requires an agent to satisfy specified capabilities. |
| `workspace` | Configures workspace cleanup behavior for a job. |
| `clean` | Defines which workspace directories are cleaned before execution. |

---

# 6. 🧩 Stages & Stage-Level Keywords

Stages represent major phases of a CI/CD pipeline. They group related jobs and define the order in which build, testing, and deployment activities execute.

## 🔹 What is a Stage?

A stage is a logical pipeline boundary that groups related jobs and controls their execution order, conditions, and dependencies.

Example:

```text
Pipeline
   |
   +---- Build Stage
   |
   +---- Test Stage
   |
   +---- Deploy Stage
```

## 🔹 1. `stages`

The `stages` keyword defines the major phases of a multi-stage pipeline.

```yaml
stages:

  - stage: Build
    displayName: "Build Stage"

    jobs:
      - job: BuildJob
        steps:
          - script: echo "Building Application"

  - stage: Test
    displayName: "Test Stage"

    jobs:
      - job: TestJob
        steps:
          - script: echo "Testing Application"
```

## 🔹 2. `stage`

The `stage` keyword identifies an individual stage in a pipeline.

```yaml
stages:

  - stage: Build
    displayName: "Build Application"

    jobs:
      - job: BuildJob
        steps:
          - script: dotnet build
```

## 🔹 3. `displayName`

The `displayName` keyword provides a human-readable name for a stage, job, or step.

```yaml
- stage: Build
  displayName: "🏗️ Build and Compile Application"
```

## 🔹 4. `dependsOn`

Defines which stages must complete before the current stage can execute.

```yaml
stages:

  - stage: Build
    jobs:
      - job: BuildJob
        steps:
          - script: echo "Build"

  - stage: Test
    dependsOn: Build

    jobs:
      - job: TestJob
        steps:
          - script: echo "Test"

  - stage: Deploy
    dependsOn: Test

    jobs:
      - job: DeployJob
        steps:
          - script: echo "Deploy"
```

### Multiple dependencies

```yaml
stages:

  - stage: Build
    jobs:
      - job: BuildJob
        steps:
          - script: echo "Build"

  - stage: SecurityScan
    dependsOn: Build
    jobs:
      - job: SecurityJob
        steps:
          - script: echo "Security Scan"

  - stage: Deploy
    dependsOn:
      - Build
      - SecurityScan

    jobs:
      - job: DeployJob
        steps:
          - script: echo "Deploy"
```

## 🔹 5. `condition`

Defines an expression that must evaluate to true before a stage is executed.

```yaml
- stage: Deploy
  dependsOn: Build

  condition: succeeded()

  jobs:
    - job: DeployJob
      steps:
        - script: echo "Deploying"
```

## 🔹 6. `variables`

Defines variables available within a particular stage.

```yaml
- stage: Deploy
  variables:
    environmentName: Production

  jobs:
    - job: DeployJob
      steps:
        - script: echo "Deploying to $(environmentName)"
```

## 🔹 7. `isSkippable`

Controls whether a stage can be skipped manually when queuing or managing a pipeline run.

```yaml
- stage: OptionalTests
  isSkippable: true

  jobs:
    - job: TestJob
      steps:
        - script: echo "Optional Tests"
```

## 🔹 8. `lockBehavior`

Controls the handling of exclusive lock requests for a stage.

```yaml
- stage: Production
  lockBehavior: sequential

  jobs:
    - job: DeployJob
      steps:
        - script: echo "Production Deployment"
```

## 🔹 9. Parallel stages

Stages without dependencies can run in parallel when the required agent capacity and parallel-job availability exist.

```yaml
stages:

  - stage: Build
    jobs:
      - job: BuildJob
        steps:
          - script: echo "Build"

  - stage: TestLinux
    dependsOn: Build

    jobs:
      - job: LinuxTests
        pool:
          vmImage: ubuntu-latest
        steps:
          - script: echo "Linux Tests"

  - stage: TestWindows
    dependsOn: Build

    jobs:
      - job: WindowsTests
        pool:
          vmImage: windows-latest
        steps:
          - script: echo "Windows Tests"

  - stage: Deploy
    dependsOn:
      - TestLinux
      - TestWindows

    jobs:
      - job: DeployJob
        steps:
          - script: echo "Deploy"
```

## 🔹 Stage keywords summary

| Keyword | Purpose |
|---|---|
| `stages` | Defines the collection of stages in a pipeline. |
| `stage` | Identifies one individual pipeline stage. |
| `displayName` | Provides a readable name for the stage. |
| `dependsOn` | Defines the stages that must complete before the current stage. |
| `condition` | Controls stage execution using a Boolean expression. |
| `variables` | Defines values that are available within a specific stage. |
| `isSkippable` | Controls whether a stage can be manually skipped. |
| `lockBehavior` | Controls exclusive lock handling for the stage. |

---

# 7. 🪜 Jobs & Job-Level Keywords

Jobs are execution units inside stages. A job contains steps and runs on an agent or, in supported cases, directly on the Azure Pipelines server.

## 🔹 What is a Job?

A job is a collection of steps that execute together on the same agent or supported server execution environment.

## 🔹 1. `jobs`

Defines a collection of jobs inside a stage.

```yaml
stages:

  - stage: Build

    jobs:

      - job: BuildJob
        steps:
          - script: echo "Build Job"

      - job: QualityJob
        steps:
          - script: echo "Quality Job"
```

## 🔹 2. `job`

Defines an individual agent job with a unique identifier.

```yaml
jobs:

  - job: BuildJob
    displayName: "Build .NET Application"

    steps:
      - script: dotnet build
```

## 🔹 3. `displayName`

Defines a readable name for a job.

```yaml
- job: TestJob
  displayName: "🧪 Execute Unit Tests"
```

## 🔹 4. `dependsOn`

Defines job dependencies inside a stage.

```yaml
jobs:

  - job: BuildJob
    steps:
      - script: echo "Build"

  - job: TestJob
    dependsOn: BuildJob

    steps:
      - script: echo "Test"
```

## 🔹 5. `condition`

Controls whether a job executes based on an expression.

```yaml
- job: DeployJob
  condition: succeeded()

  steps:
    - script: echo "Deploying"
```

## 🔹 6. `continueOnError`

Allows the pipeline to continue according to the task or job failure-handling behavior.

```yaml
- job: OptionalScan

  continueOnError: true

  steps:
    - script: echo "Running optional scan"
```

Use this carefully. Do not ignore failures from critical build, test, or security checks.

## 🔹 7. `timeoutInMinutes`

Defines the maximum time a job can run before Azure Pipelines terminates it.

```yaml
- job: BuildJob

  timeoutInMinutes: 30

  steps:
    - script: dotnet build
```

## 🔹 8. `cancelTimeoutInMinutes`

Defines how long a job is allowed to finish cancellation-related work after cancellation is requested.

```yaml
- job: DeployJob

  cancelTimeoutInMinutes: 2

  steps:
    - script: echo "Deployment"
```

## 🔹 9. Job-level variables

Defines variables that are available only inside a particular job.

```yaml
jobs:

  - job: BuildJob

    variables:
      configuration: Release

    steps:
      - script: dotnet build --configuration $(configuration)
```

## 🔹 10. `strategy`

Defines an execution strategy for a job, such as matrix execution or parallel execution.

### Matrix strategy

```yaml
jobs:

  - job: BuildMatrix

    strategy:
      matrix:

        Linux:
          imageName: ubuntu-latest

        Windows:
          imageName: windows-latest

    pool:
      vmImage: $(imageName)

    steps:
      - script: echo "Running on $(imageName)"
```

### Parallel strategy

```yaml
jobs:

  - job: ParallelTests

    strategy:
      parallel: 3

    steps:
      - script: echo "Parallel job execution"
```

## 🔹 11. `container`

Runs an agent job inside a specified container image.

```yaml
resources:
  containers:
    - container: dotnetSdk
      image: mcr.microsoft.com/dotnet/sdk:8.0

jobs:

  - job: ContainerBuild

    container: dotnetSdk

    steps:
      - script: dotnet --version
```

## 🔹 12. `services`

Defines service containers that are available to a container job.

```yaml
resources:
  containers:
    - container: dotnetSdk
      image: mcr.microsoft.com/dotnet/sdk:8.0

    - container: sqlServer
      image: mcr.microsoft.com/mssql/server:2022-latest

      env:
        ACCEPT_EULA: Y
        MSSQL_SA_PASSWORD: $(SqlServerPassword)

jobs:

  - job: IntegrationTests

    container: dotnetSdk

    services:
      database: sqlServer

    steps:
      - script: dotnet test
```

> Service-container networking, image compatibility, and agent requirements must be configured correctly. Keep database passwords in secret variables, not in the YAML file.

## 🔹 13. `workspace`

Configures the workspace cleanup behavior for a job.

```yaml
- job: CleanBuild

  workspace:
    clean: all

  steps:
    - script: dotnet build
```

## 🔹 Job keywords summary

| Keyword | Purpose |
|---|---|
| `jobs` | Defines jobs belonging to a stage or pipeline. |
| `job` | Identifies an individual agent or server job. |
| `displayName` | Provides a human-readable job name. |
| `dependsOn` | Defines dependencies between jobs. |
| `condition` | Controls whether a job should execute. |
| `continueOnError` | Controls how job failure affects pipeline continuation. |
| `timeoutInMinutes` | Defines the maximum execution time for a job. |
| `cancelTimeoutInMinutes` | Defines the cancellation timeout for a job. |
| `variables` | Defines job-specific variables. |
| `strategy` | Configures matrix or parallel job execution. |
| `matrix` | Creates multiple job variations using different variable values. |
| `parallel` | Configures multiple parallel job copies. |
| `container` | Selects a container image for a container job. |
| `services` | Defines supporting service containers for a container job. |
| `workspace` | Configures the agent workspace for a job. |
| `clean` | Selects the workspace cleanup mode. |

---

# 8. 🔌 Steps, Tasks & Scripts

Steps are the individual operations executed inside a job. Azure Pipelines supports scripts, predefined tasks, checkout operations, artifact operations, and reusable step templates.

## 🔹 What is a Step?

A step is an individual executable operation inside a job that performs a command, task, or pipeline action.

## 🔹 1. `steps`

Defines the list of operations that a job executes.

```yaml
jobs:

  - job: BuildJob

    steps:

      - script: echo "Step 1"

      - script: echo "Step 2"

      - script: echo "Step 3"
```

## 🔹 2. `script`

Runs a command using the agent's default command shell.

```yaml
steps:
  - script: dotnet --version
    displayName: "Check .NET Version"
```

## 🔹 3. `bash`

Runs a Bash script on a supported agent.

```yaml
steps:
  - bash: |
      echo "Starting .NET Build"
      dotnet restore
      dotnet build --configuration Release

    displayName: "Build Using Bash"
```

## 🔹 4. `pwsh`

Runs a PowerShell Core script.

```yaml
steps:
  - pwsh: |
      Write-Host "Starting .NET Build"
      dotnet restore
      dotnet build --configuration Release

    displayName: "Build Using PowerShell Core"
```

## 🔹 5. `powershell`

Runs a Windows PowerShell script, typically on a Windows agent.

```yaml
steps:
  - powershell: |
      Write-Host "Checking .NET SDK"
      dotnet --version

    displayName: "PowerShell Script"
```

## 🔹 6. `task`

Runs a predefined Azure Pipelines task.

```yaml
steps:
  - task: UseDotNet@2

    inputs:
      packageType: sdk
      version: "8.x"

    displayName: "Install .NET SDK"
```

The `@2` indicates the major version of the task.

## 🔹 7. `checkout`

Checks out a repository so that the job can access its source code.

```yaml
steps:
  - checkout: self
    clean: true
    fetchDepth: 1
```

Common checkout options:

```yaml
steps:
  - checkout: self

    clean: true
    fetchDepth: 0
    persistCredentials: false
    submodules: false
```

## 🔹 8. `workingDirectory`

Specifies the directory in which a script or task command should execute.

```yaml
steps:
  - script: dotnet build MyWebApi.csproj
    workingDirectory: "$(Build.SourcesDirectory)/src/MyWebApi"
    displayName: "Build from Project Directory"
```

## 🔹 9. `env`

Provides environment variables to an individual step.

```yaml
steps:
  - script: dotnet test

    env:
      ASPNETCORE_ENVIRONMENT: Testing
      DOTNET_NOLOGO: true

    displayName: "Run Tests"
```

## 🔹 10. `condition`

Executes a step only when its condition evaluates to true.

```yaml
steps:
  - script: echo "Running only on main"

    condition: eq(variables['Build.SourceBranch'], 'refs/heads/main')
```

## 🔹 11. `continueOnError`

Allows the pipeline to continue when a non-critical step fails.

```yaml
steps:
  - script: echo "Optional analysis"

    continueOnError: true
```

## 🔹 12. `enabled`

Controls whether a step is enabled.

```yaml
steps:
  - script: echo "This step is enabled"

    enabled: true
```

## 🔹 13. `timeoutInMinutes`

Sets the maximum execution time for an individual step.

```yaml
steps:
  - script: dotnet test

    timeoutInMinutes: 10
```

## 🔹 14. `retryCountOnTaskFailure`

Retries a task when it fails, up to the configured number of retries.

```yaml
steps:
  - task: DotNetCoreCLI@2

    inputs:
      command: test
      projects: "**/*Tests.csproj"

    retryCountOnTaskFailure: 2
```

## 🔹 15. `target`

Specifies whether a step runs on the agent or Azure Pipelines server.

```yaml
steps:
  - script: echo "Runs on the agent"
    target: host

  - task: ManualValidation@1
    inputs:
      notifyUsers: ""
      instructions: "Review this pipeline."
      onTimeout: reject
    target: server
```

> Server-side tasks have specific requirements. For example, `ManualValidation@1` must run in an agentless server job.

## 🔹 16. .NET task examples

### Install .NET SDK

```yaml
- task: UseDotNet@2

  inputs:
    packageType: sdk
    version: "8.x"

  displayName: "Install .NET 8 SDK"
```

### Restore NuGet packages

```yaml
- task: DotNetCoreCLI@2

  inputs:
    command: restore
    projects: "**/*.sln"

  displayName: "Restore NuGet Packages"
```

### Build application

```yaml
- task: DotNetCoreCLI@2

  inputs:
    command: build
    projects: "src/MyWebApi/MyWebApi.csproj"
    arguments: "--configuration Release --no-restore"

  displayName: "Build Application"
```

### Run unit tests

```yaml
- task: DotNetCoreCLI@2

  inputs:
    command: test
    projects: "tests/**/*Tests.csproj"
    arguments: "--configuration Release --no-build"

  displayName: "Run Unit Tests"
```

### Publish application

```yaml
- task: DotNetCoreCLI@2

  inputs:
    command: publish
    publishWebProjects: false
    projects: "src/MyWebApi/MyWebApi.csproj"
    arguments: "--configuration Release --output $(Build.ArtifactStagingDirectory)"
    zipAfterPublish: true

  displayName: "Publish Application"
```

## 🔹 Steps and tasks summary

| Keyword / Task | Purpose |
|---|---|
| `steps` | Defines the operations executed by a job. |
| `script` | Executes commands using the agent's default shell. |
| `bash` | Executes a Bash script on a supported agent. |
| `pwsh` | Executes a PowerShell Core script. |
| `powershell` | Executes a Windows PowerShell script. |
| `task` | Executes a predefined or installed pipeline task. |
| `checkout` | Checks out source code from a repository. |
| `workingDirectory` | Specifies the working directory for a script. |
| `env` | Supplies environment variables to a step. |
| `condition` | Controls whether an individual step executes. |
| `continueOnError` | Allows subsequent execution after a step failure. |
| `enabled` | Enables or disables a step. |
| `timeoutInMinutes` | Defines the maximum execution time for a step. |
| `retryCountOnTaskFailure` | Retries a failed task a configured number of times. |
| `target` | Selects the execution target for a step. |
| `UseDotNet@2` | Installs or selects a .NET SDK or runtime. |
| `DotNetCoreCLI@2` | Runs .NET CLI commands such as restore, build, test, and publish. |

---

# 9. 🔐 Variables, Parameters & Secrets

Variables and parameters make pipelines reusable and configurable. Secret variables protect sensitive information such as passwords, tokens, and deployment credentials.

## 🔹 What is a Variable?

A variable is a named value that can be reused throughout a pipeline without repeatedly writing the same configuration value.

## 🔹 1. Pipeline-level variables

```yaml
variables:
  buildConfiguration: Release
  dotnetVersion: "8.x"
  projectPath: "src/MyWebApi/MyWebApi.csproj"
```

Usage:

```yaml
steps:
  - script: dotnet build "$(projectPath)" --configuration $(buildConfiguration)
```

## 🔹 2. List-based variables

```yaml
variables:

  - name: buildConfiguration
    value: Release

  - name: dotnetVersion
    value: "8.x"

  - name: applicationName
    value: MyWebApi
```

## 🔹 3. Stage-level variables

```yaml
stages:

  - stage: Build

    variables:
      configuration: Release

    jobs:
      - job: BuildJob
        steps:
          - script: dotnet build --configuration $(configuration)
```

## 🔹 4. Job-level variables

```yaml
jobs:

  - job: TestJob

    variables:
      testConfiguration: Release

    steps:
      - script: dotnet test --configuration $(testConfiguration)
```

## 🔹 5. Variable groups

Variable groups store reusable variables in Azure DevOps Library so that multiple pipelines can consume shared configuration.

```yaml
variables:
  - group: MyApplication-Variables
```

Example with a variable group and local variables:

```yaml
variables:

  - group: MyApplication-Variables

  - name: buildConfiguration
    value: Release

  - name: dotnetVersion
    value: "8.x"
```

Create the group from:

```text
Azure DevOps
   |
   v
Pipelines
   |
   v
Library
   |
   v
Variable Groups
```

## 🔹 6. Secret variables

Secret variables protect sensitive values by masking them in supported pipeline logs and restricting their exposure.

Example YAML:

```yaml
steps:

  - script: |
      echo "Deployment started"

    env:
      API_TOKEN: $(API_TOKEN)

    displayName: "Use Secret Variable"
```

Create `API_TOKEN` as a secret variable in the pipeline UI or a secured variable group.

> Never print secrets using `echo`, never commit them to YAML, and do not assume log masking protects every possible way a secret could be exposed.

## 🔹 7. `isSecret`

Marks a variable as secret when using supported variable-definition syntax.

```yaml
variables:

  - name: API_TOKEN
    value: "Use-a-secured-value-in-the-UI"
    isSecret: true
```

For real credentials, prefer creating the secret in the Azure DevOps UI or a secure variable group rather than storing the value in source control.

## 🔹 8. `readonly`

Marks a variable as read-only in supported YAML variable definitions.

```yaml
variables:

  - name: buildConfiguration
    value: Release
    readonly: true
```

## 🔹 9. `group`

References a variable group created in the Azure DevOps Library.

```yaml
variables:
  - group: Production-Secrets
```

## 🔹 10. `template` variable

Imports variables from a reusable YAML template.

```yaml
variables:
  - template: templates/variables.yml
```

Example `templates/variables.yml`:

```yaml
variables:
  buildConfiguration: Release
  dotnetVersion: "8.x"
```

## 🔹 11. Parameters

Parameters are typed inputs evaluated during template expansion, before the pipeline starts executing jobs.

```yaml
parameters:

  - name: environment
    type: string
    default: Development

    values:
      - Development
      - Staging
      - Production
```

Usage:

```yaml
steps:
  - script: echo "Selected environment: ${{ parameters.environment }}"
```

## 🔹 12. Parameter data types

```yaml
parameters:

  - name: enableTests
    type: boolean
    default: true

  - name: retryCount
    type: number
    default: 2

  - name: environment
    type: string
    default: Development

  - name: selectedStages
    type: object
    default:
      - Build
      - Test
```

Common parameter types:

| Type | Purpose |
|---|---|
| `string` | Accepts a text value. |
| `number` | Accepts a numeric value. |
| `boolean` | Accepts true or false. |
| `object` | Accepts structured YAML data such as lists and mappings. |
| `step` | Accepts a single pipeline step. |
| `stepList` | Accepts a list of pipeline steps. |
| `job` | Accepts a job definition. |
| `jobList` | Accepts a list of jobs. |
| `stage` | Accepts a stage definition. |
| `stageList` | Accepts a list of stages. |

## 🔹 13. Variable syntax

Azure Pipelines supports three important variable syntaxes.

| Syntax | Name | Evaluation |
|---|---|---|
| `$(variableName)` | Macro syntax | Evaluated at runtime before a task executes. |
| `${{ variables.variableName }}` | Template expression | Evaluated during template compilation. |
| `$[variables.variableName]` | Runtime expression | Evaluated at runtime and commonly used in conditions and variable assignments. |

Examples:

```yaml
variables:
  buildConfiguration: Release

steps:

  # Macro syntax
  - script: echo "$(buildConfiguration)"

  # Template expression
  - script: echo "${{ variables.buildConfiguration }}"

  # Runtime expression
  - script: echo "Runtime value: $[variables.buildConfiguration]"
```

> Runtime expressions are commonly used as complete right-hand-side values. Avoid treating them as interchangeable with macro syntax.

## 🔹 14. Environment variable mapping

Variables can be passed to scripts as environment variables.

```yaml
variables:
  applicationName: MyWebApi

steps:

  - bash: |
      echo "Application: $APP_NAME"

    env:
      APP_NAME: $(applicationName)
```

## 🔹 Variables and parameters summary

| Keyword | Purpose |
|---|---|
| `variables` | Defines reusable pipeline configuration values. |
| `name` | Defines a variable or parameter identifier. |
| `value` | Assigns a value to a variable. |
| `group` | Imports variables from an Azure DevOps variable group. |
| `template` | Imports variables from a YAML template file. |
| `isSecret` | Marks a variable as secret in supported definitions. |
| `readonly` | Prevents supported variables from being modified. |
| `parameters` | Defines typed pipeline and template inputs. |
| `type` | Specifies the data type of a parameter. |
| `default` | Provides a default parameter value. |
| `values` | Restricts a parameter to an allowed set of values. |
| `env` | Maps pipeline values to script environment variables. |

---

# 10. 🧠 Expressions, Conditions & Runtime Logic

Expressions and conditions control pipeline execution. They allow pipelines to make decisions based on branch names, job results, variable values, and runtime information.

## 🔹 1. `condition`

Defines a Boolean expression that determines whether a stage, job, or step executes.

```yaml
- script: echo "Deploying"

  condition: succeeded()
```

## 🔹 2. `succeeded()`

Returns true when the relevant preceding pipeline execution has succeeded.

```yaml
condition: succeeded()
```

## 🔹 3. `failed()`

Returns true when the relevant preceding execution has failed.

```yaml
condition: failed()
```

## 🔹 4. `succeededOrFailed()`

Returns true when the preceding execution completed successfully or unsuccessfully, excluding skipped and canceled cases.

```yaml
condition: succeededOrFailed()
```

## 🔹 5. `always()`

Returns true regardless of the preceding execution result, including failure or cancellation, subject to pipeline cancellation and execution constraints.

```yaml
- script: echo "Cleanup"

  condition: always()
```

## 🔹 6. `canceled()`

Returns true when the pipeline or relevant execution has been canceled.

```yaml
condition: canceled()
```

## 🔹 7. `eq()`

Checks whether two values are equal.

```yaml
condition: eq(variables['Build.SourceBranch'], 'refs/heads/main')
```

## 🔹 8. `ne()`

Checks whether two values are not equal.

```yaml
condition: ne(variables['Build.SourceBranch'], 'refs/heads/main')
```

## 🔹 9. `and()`

Returns true when all supplied expressions are true.

```yaml
condition: and(
  succeeded(),
  eq(variables['Build.SourceBranch'], 'refs/heads/main')
)
```

## 🔹 10. `or()`

Returns true when at least one supplied expression is true.

```yaml
condition: or(
  eq(variables['Build.SourceBranch'], 'refs/heads/main'),
  eq(variables['Build.SourceBranch'], 'refs/heads/development')
)
```

## 🔹 11. `not()`

Reverses the result of a Boolean expression.

```yaml
condition: not(
  eq(variables['Build.SourceBranch'], 'refs/heads/main')
)
```

## 🔹 12. `in()`

Checks whether a value matches one of the supplied values.

```yaml
condition: in(
  variables['Build.SourceBranch'],
  'refs/heads/main',
  'refs/heads/development'
)
```

## 🔹 13. `contains()`

Checks whether a string contains a specified substring.

```yaml
condition: contains(
  variables['Build.SourceBranch'],
  'feature'
)
```

## 🔹 14. `startsWith()`

Checks whether a string begins with a specified value.

```yaml
condition: startsWith(
  variables['Build.SourceBranch'],
  'refs/heads/release/'
)
```

## 🔹 15. `endsWith()`

Checks whether a string ends with a specified value.

```yaml
condition: endsWith(
  variables['Build.SourceBranch'],
  '/main'
)
```

## 🔹 16. `containsValue()`

Checks whether a value exists in a supported collection such as an object or array.

```yaml
parameters:
  - name: allowedBranches
    type: object
    default:
      - refs/heads/main
      - refs/heads/development

steps:
  - script: echo "Allowed branch"

    condition: containsValue(
      parameters.allowedBranches,
      variables['Build.SourceBranch']
    )
```

## 🔹 17. `format()`

Creates a formatted string using supplied values.

```yaml
variables:
  buildLabel: $[format('{0}-{1}', 'Release', variables['Build.BuildId'])]
```

## 🔹 18. `coalesce()`

Returns the first non-empty value from the supplied arguments.

```yaml
variables:
  selectedEnvironment: $[coalesce(variables['targetEnvironment'], 'Development')]
```

## 🔹 19. `counter()`

Generates a counter value that increments for a particular prefix.

```yaml
variables:
  buildCounter: $[counter('myBuild', 1)]
```

## 🔹 20. `iif()`

Returns one value when a condition is true and another when it is false.

```yaml
variables:
  buildMode: $[iif(eq(variables['Build.SourceBranch'], 'refs/heads/main'), 'Production', 'Development')]
```

## 🔹 21. Compile-time conditional insertion

`${{ if }}` adds YAML elements during template compilation when the condition is true.

```yaml
parameters:
  - name: deployProduction
    type: boolean
    default: false

stages:

  - stage: Build
    jobs:
      - job: BuildJob
        steps:
          - script: echo "Build"

  - ${{ if eq(parameters.deployProduction, true) }}:
      - stage: Production
        jobs:
          - job: DeployJob
            steps:
              - script: echo "Production deployment"
```

## 🔹 22. `each`

The `each` expression iterates over an object or list during template expansion.

```yaml
parameters:
  - name: environments
    type: object
    default:
      - Development
      - Staging
      - Production

stages:

  - ${{ each environment in parameters.environments }}:
      - stage: ${{ environment }}
        displayName: "Deploy to ${{ environment }}"

        jobs:
          - job: DeployJob
            steps:
              - script: echo "Deploying to ${{ environment }}"
```

## 🔹 23. `insert`

The `insert` expression inserts a mapping into another mapping during template expansion.

```yaml
parameters:
  - name: additionalVariables
    type: object
    default:
      applicationName: MyWebApi
      configuration: Release

variables:
  ${{ insert }}: ${{ parameters.additionalVariables }}

steps:
  - script: echo "$(applicationName) - $(configuration)"
```

## 🔹 Important expression functions summary

| Function / Keyword | Purpose |
|---|---|
| `condition` | Controls execution using a Boolean expression. |
| `succeeded()` | Checks whether preceding execution succeeded. |
| `failed()` | Checks whether preceding execution failed. |
| `succeededOrFailed()` | Checks for successful or failed completion. |
| `always()` | Runs regardless of the preceding result when execution permits. |
| `canceled()` | Checks whether the execution was canceled. |
| `eq()` | Compares two values for equality. |
| `ne()` | Compares two values for inequality. |
| `and()` | Requires all supplied expressions to be true. |
| `or()` | Requires at least one supplied expression to be true. |
| `not()` | Reverses a Boolean expression result. |
| `in()` | Checks whether a value matches one of several alternatives. |
| `contains()` | Checks whether a string contains a substring. |
| `startsWith()` | Checks whether a string starts with a specified value. |
| `endsWith()` | Checks whether a string ends with a specified value. |
| `containsValue()` | Checks whether a value exists in a collection. |
| `format()` | Creates a formatted string from supplied values. |
| `coalesce()` | Returns the first non-empty supplied value. |
| `counter()` | Generates an incrementing value for a specified prefix. |
| `iif()` | Selects a value based on a Boolean expression. |
| `${{ if }}` | Conditionally inserts YAML during template compilation. |
| `${{ each }}` | Iterates through objects or lists during template compilation. |
| `${{ insert }}` | Inserts a mapping into another mapping. |

> **Important:** A custom `condition` replaces the default success condition. When you need both, explicitly include `succeeded()` in the expression, for example `and(succeeded(), eq(...))`.

---

# 11. 🔀 Dependencies, Outputs & Job Communication

Dependencies control execution order between stages and jobs. Output variables allow one job to pass selected values to another job or stage.

## 🔹 1. `dependsOn`

Defines which jobs or stages must complete before the current execution unit starts.

```yaml
jobs:

  - job: BuildJob
    steps:
      - script: echo "Build"

  - job: TestJob
    dependsOn: BuildJob
    steps:
      - script: echo "Test"
```

## 🔹 2. Multiple job dependencies

```yaml
jobs:

  - job: BuildJob
    steps:
      - script: echo "Build"

  - job: UnitTest
    dependsOn: BuildJob
    steps:
      - script: echo "Unit Tests"

  - job: SecurityScan
    dependsOn: BuildJob
    steps:
      - script: echo "Security Scan"

  - job: PackageJob
    dependsOn:
      - UnitTest
      - SecurityScan

    steps:
      - script: echo "Packaging"
```

## 🔹 3. `dependsOn: []`

Removes the default dependency on the preceding stage or job, allowing an independent execution path.

```yaml
jobs:

  - job: FirstJob
    steps:
      - script: echo "First"

  - job: IndependentJob
    dependsOn: []
    steps:
      - script: echo "Runs independently"
```

## 🔹 4. `isOutput`

Marks a variable as an output variable so that another job can consume it through the dependency context.

```yaml
jobs:

  - job: BuildJob

    steps:
      - bash: |
          echo "##vso[task.setvariable variable=appVersion;isOutput=true]1.0.0"

        name: SetVersion
        displayName: "Set Application Version"

  - job: DeployJob

    dependsOn: BuildJob

    variables:
      appVersion: $[ dependencies.BuildJob.outputs['SetVersion.appVersion'] ]

    steps:
      - script: echo "Deploying version $(appVersion)"
```

## 🔹 5. Output variables across stages

Use `stageDependencies` to access output variables from a job in an earlier stage.

```yaml
stages:

  - stage: Build

    jobs:
      - job: BuildJob

        steps:
          - bash: |
              echo "##vso[task.setvariable variable=appVersion;isOutput=true]1.0.0"

            name: SetVersion

  - stage: Deploy
    dependsOn: Build

    variables:
      appVersion: $[ stageDependencies.Build.BuildJob.outputs['SetVersion.appVersion'] ]

    jobs:
      - job: DeployJob

        steps:
          - script: echo "Deploying version $(appVersion)"
```

## 🔹 6. `stageDependencies`

Provides access to dependency outputs and execution information across stages.

```yaml
variables:
  previousVersion: $[ stageDependencies.Build.BuildJob.outputs['SetVersion.appVersion'] ]
```

## 🔹 7. `dependencies`

Provides dependency information for jobs or stages, depending on the expression context.

```yaml
variables:
  buildResult: $[ dependencies.BuildJob.result ]
```

## 🔹 8. `dependsOn` with conditions

```yaml
jobs:

  - job: BuildJob
    steps:
      - script: echo "Build"

  - job: DeployJob

    dependsOn: BuildJob

    condition: and(
      succeeded(),
      eq(variables['Build.SourceBranch'], 'refs/heads/main')
    )

    steps:
      - script: echo "Deploying from main"
```

## 🔹 9. `isReadOnly`

Marks a logging-command-created variable as read-only for later pipeline steps.

```yaml
steps:

  - bash: |
      echo "##vso[task.setvariable variable=appName;isReadOnly=true]MyWebApi"
    displayName: "Set Read-Only Variable"

  - script: echo "$(appName)"
    displayName: "Read Variable"
```

## 🔹 Dependency keywords summary

| Keyword | Purpose |
|---|---|
| `dependsOn` | Defines execution dependencies between stages or jobs. |
| `dependencies` | Provides access to dependency results and output data in supported contexts. |
| `stageDependencies` | Provides access to outputs and results from earlier stages. |
| `isOutput` | Marks a logging-command variable as available to dependent jobs or stages. |
| `isReadOnly` | Marks a logging-command variable as read-only. |
| `condition` | Controls execution based on dependency results and other expressions. |

---

# 12. 📦 Artifacts, Cache, Containers & Services

Artifacts preserve build outputs for later jobs and deployment stages. Caching reduces repeated dependency downloads, while containers and service containers provide consistent execution environments.

## 🔹 1. What is an Artifact?

An artifact is a collection of files produced by a pipeline and stored for later jobs, pipeline runs, or deployments.

Example:

```text
Build Stage
   |
   v
.NET Publish
   |
   v
Publish Artifact
   |
   v
Pipeline Artifact
   |
   v
Deploy Stage
   |
   v
Download Artifact
```

## 🔹 2. `PublishPipelineArtifact@1`

Publishes files as a pipeline artifact.

```yaml
steps:

  - task: PublishPipelineArtifact@1

    inputs:
      targetPath: "$(Build.ArtifactStagingDirectory)"
      artifact: "drop"
      publishLocation: pipeline

    displayName: "Publish Build Artifact"
```

## 🔹 3. `DownloadPipelineArtifact@2`

Downloads a previously published pipeline artifact.

```yaml
steps:

  - task: DownloadPipelineArtifact@2

    inputs:
      artifact: drop
      path: "$(Pipeline.Workspace)/drop"

    displayName: "Download Build Artifact"
```

## 🔹 4. `publish` shortcut

The `publish` keyword publishes a path as a pipeline artifact.

```yaml
steps:

  - publish: "$(Build.ArtifactStagingDirectory)"
    artifact: drop

    displayName: "Publish Application"
```

## 🔹 5. `download` shortcut

The `download` keyword downloads pipeline artifacts or supported pipeline resource artifacts.

```yaml
steps:

  - download: current
    artifact: drop

    displayName: "Download Current Pipeline Artifact"
```

## 🔹 6. `DownloadBuildArtifacts@1`

Downloads build artifacts from supported Azure DevOps build runs.

```yaml
steps:

  - task: DownloadBuildArtifacts@1

    inputs:
      buildType: current
      downloadType: single
      artifactName: drop
      downloadPath: "$(System.ArtifactsDirectory)"

    displayName: "Download Build Artifacts"
```

> `DownloadBuildArtifacts@1` is the older build-artifact workflow. For modern Azure DevOps Services YAML pipelines, prefer Pipeline Artifacts when they meet your requirements.

## 🔹 7. `Cache@2`

Caches files or directories to reduce repeated dependency downloads and improve pipeline performance.

```yaml
steps:

  - task: Cache@2

    inputs:
      key: 'nuget | "$(Agent.OS)" | **/packages.lock.json,!**/bin/**,!**/obj/**'
      restoreKeys: |
        nuget | "$(Agent.OS)"
      path: "$(NUGET_PACKAGES)"

    displayName: "Cache NuGet Packages"
```

A corresponding NuGet configuration:

```yaml
variables:
  NUGET_PACKAGES: $(Pipeline.Workspace)/.nuget/packages
```

Then restore packages:

```yaml
- task: DotNetCoreCLI@2

  inputs:
    command: restore
    projects: "**/*.sln"

  displayName: "Restore NuGet Packages"
```

> For reliable lock-file-based caching, commit the appropriate NuGet lock files and ensure the cache key matches the files used by your restore process.

## 🔹 8. `container`

Defines a container image used by an agent job.

```yaml
resources:
  containers:
    - container: dotnetSdk
      image: mcr.microsoft.com/dotnet/sdk:8.0

jobs:

  - job: BuildJob

    container: dotnetSdk

    steps:
      - script: dotnet --version
```

## 🔹 9. `services`

Defines supporting containers such as databases that a container job can use.

```yaml
resources:
  containers:

    - container: dotnetSdk
      image: mcr.microsoft.com/dotnet/sdk:8.0

    - container: database
      image: postgres:16

      env:
        POSTGRES_USER: appuser
        POSTGRES_PASSWORD: $(DatabasePassword)
        POSTGRES_DB: appdb

jobs:

  - job: IntegrationTests

    container: dotnetSdk

    services:
      postgres: database

    steps:
      - script: dotnet test
```

## 🔹 10. Pipeline artifacts vs Build artifacts

| Feature | Pipeline Artifacts | Build Artifacts |
|---|---|---|
| Task | `PublishPipelineArtifact@1` | `PublishBuildArtifacts@1` |
| Download task | `DownloadPipelineArtifact@2` | `DownloadBuildArtifacts@1` |
| Usage | Modern Azure DevOps Services pipelines | Older build-artifact workflows and compatible scenarios |
| Recommendation | Preferred for Azure DevOps Services YAML pipelines | Use when compatibility or an existing workflow requires it |

## 🔹 11. Important artifact and cache paths

| Variable | Purpose |
|---|---|
| `$(Build.SourcesDirectory)` | Directory containing the checked-out source code. |
| `$(Build.ArtifactStagingDirectory)` | Directory used to stage files before publishing. |
| `$(Pipeline.Workspace)` | Workspace directory available to the pipeline job. |
| `$(System.ArtifactsDirectory)` | Directory used for downloaded build artifacts. |
| `$(Agent.TempDirectory)` | Temporary directory available to the agent job. |

---

# 13. ♻️ Templates, Reusability & Advanced YAML

Templates help organize large pipelines by separating reusable stages, jobs, steps, and variables into different YAML files.

## 🔹 What is a Template?

A template is a reusable YAML file that allows pipeline configuration to be shared across multiple pipelines or execution stages.

### Example folder structure

```text
MyDotNetApplication/
│
├── azure-pipelines.yml
│
└── templates/
    │
    ├── variables.yml
    │
    ├── build-steps.yml
    │
    ├── test-job.yml
    │
    ├── build-stage.yml
    │
    └── deploy-stage.yml
```

## 🔹 1. Step template

A step template reuses a group of steps inside different jobs.

`templates/build-steps.yml`

```yaml
steps:

  - task: UseDotNet@2

    inputs:
      packageType: sdk
      version: "8.x"

  - script: dotnet restore
    displayName: "Restore Packages"

  - script: dotnet build --configuration Release
    displayName: "Build Application"
```

Use it:

```yaml
steps:
  - template: templates/build-steps.yml
```

## 🔹 2. Job template

A job template reuses an entire job definition.

`templates/test-job.yml`

```yaml
jobs:

  - job: TestJob
    displayName: "Run Unit Tests"

    steps:

      - task: UseDotNet@2

        inputs:
          packageType: sdk
          version: "8.x"

      - script: dotnet test --configuration Release
        displayName: "Execute Tests"
```

Use it:

```yaml
jobs:
  - template: templates/test-job.yml
```

## 🔹 3. Stage template

A stage template reuses an entire stage definition.

`templates/build-stage.yml`

```yaml
stages:

  - stage: Build
    displayName: "Build Stage"

    jobs:

      - job: BuildJob

        steps:
          - script: dotnet build
```

Use it:

```yaml
stages:
  - template: templates/build-stage.yml
```

## 🔹 4. Variable template

A variable template stores reusable pipeline variables.

`templates/variables.yml`

```yaml
variables:

  buildConfiguration: Release
  dotnetVersion: "8.x"
  projectPath: "src/MyWebApi/MyWebApi.csproj"
```

Use it:

```yaml
variables:
  - template: templates/variables.yml
```

## 🔹 5. Template parameters

Parameters allow templates to receive configurable values from the parent pipeline.

`templates/build-steps.yml`

```yaml
parameters:

  - name: configuration
    type: string
    default: Release

steps:

  - script: dotnet build --configuration ${{ parameters.configuration }}

    displayName: "Build Application"
```

Parent pipeline:

```yaml
steps:

  - template: templates/build-steps.yml

    parameters:
      configuration: Debug
```

## 🔹 6. `extends`

The `extends` keyword allows a pipeline to inherit its structure from a base pipeline template.

`templates/base-pipeline.yml`

```yaml
parameters:

  - name: buildConfiguration
    type: string
    default: Release

stages:

  - stage: Build

    jobs:
      - job: BuildJob

        steps:
          - script: dotnet build --configuration ${{ parameters.buildConfiguration }}
```

Main pipeline:

```yaml
extends:
  template: templates/base-pipeline.yml

  parameters:
    buildConfiguration: Release
```

## 🔹 7. `@self`

References a template from the repository containing the current pipeline.

```yaml
resources:
  repositories:
    - repository: sharedTemplates
      type: git
      name: DevOpsProject/PipelineTemplates
      ref: refs/heads/main

steps:
  - template: build-steps.yml@sharedTemplates
```

For a template in the same repository, use a relative path:

```yaml
steps:
  - template: templates/build-steps.yml
```

## 🔹 8. External repository templates

Templates can be stored in a separate Azure Repos Git repository and imported as a repository resource.

```yaml
resources:

  repositories:
    - repository: commonTemplates
      type: git
      name: DevOpsProject/CommonTemplates
      ref: refs/heads/main

steps:

  - template: dotnet/build-steps.yml@commonTemplates
```

## 🔹 9. YAML anchors and aliases

YAML anchors and aliases reuse YAML nodes within a YAML document. They are different from Azure Pipelines templates.

```yaml
variables: &buildVariables
  buildConfiguration: Release
  dotnetVersion: "8.x"

stages:

  - stage: Build

    variables:
      <<: *buildVariables

    jobs:
      - job: BuildJob
        steps:
          - script: dotnet build --configuration $(buildConfiguration)

  - stage: Test

    variables:
      <<: *buildVariables

    jobs:
      - job: TestJob
        steps:
          - script: dotnet test --configuration $(buildConfiguration)
```

> YAML anchor support depends on the YAML parser and Azure Pipelines implementation. For large reusable pipeline structures, prefer official Azure Pipelines templates.

## 🔹 10. `templateContext`

Passes additional contextual information to jobs, deployment jobs, or stages defined through templates.

```yaml
jobs:

  - template: templates/test-job.yml

    parameters:
      # Template-specific parameters belong here.
      configuration: Release
```

For advanced `templateContext` usage, define the appropriate context in the template schema and follow the official documentation.

## 🔹 Reusability keywords summary

| Keyword | Purpose |
|---|---|
| `template` | Imports reusable pipeline YAML configuration. |
| `extends` | Builds a pipeline by extending a base template. |
| `parameters` | Supplies configurable inputs to templates. |
| `resources.repositories` | Declares external repositories used by the pipeline. |
| `@alias` | References a repository resource when importing a template. |
| `&anchor` | Defines a reusable YAML node. |
| `*alias` | References a YAML anchor. |
| `<<` | Merges an anchored mapping into another mapping. |
| `templateContext` | Passes contextual information to supported template-defined pipeline objects. |

---

# 14. 🛡️ Security, Environments & Best Practices

Security is an important part of CI/CD. Azure DevOps provides service connections, secret variables, environments, approvals, checks, and access controls to protect deployment workflows.

## 🔹 1. Service Connections

A service connection securely connects Azure Pipelines to an external service such as an Azure subscription, container registry, or Kubernetes cluster.

Example:

```text
Azure DevOps
   |
   v
Project Settings
   |
   v
Service Connections
   |
   v
New Service Connection
   |
   v
Azure Resource Manager
```

Create a service connection named:

```text
Azure-Production-Connection
```

Use it in a deployment task:

```yaml
- task: AzureWebApp@1

  inputs:
    azureSubscription: "Azure-Production-Connection"
    appType: webAppLinux
    appName: "my-dotnet-production-app"
    package: "$(Pipeline.Workspace)/drop/**/*.zip"
```

The service connection should use an appropriately secured identity, preferably workload identity federation where supported, rather than long-lived credentials.

## 🔹 2. Azure Key Vault

Azure Key Vault securely stores secrets, certificates, and keys that applications or deployment pipelines need.

Example using the Azure Key Vault task:

```yaml
- task: AzureKeyVault@2

  inputs:
    azureSubscription: "Azure-Production-Connection"
    KeyVaultName: "my-production-keyvault"
    SecretsFilter: "*"
    RunAsPreJob: false

  displayName: "Fetch Secrets from Key Vault"
```

The service connection identity must have the required Key Vault permissions.

## 🔹 3. `environment`

An environment represents a logical deployment target and can provide deployment history, resource visibility, approvals, and checks.

```yaml
jobs:

  - deployment: DeployProduction

    environment: Production

    strategy:
      runOnce:
        deploy:
          steps:
            - script: echo "Deploying to Production"
```

## 🔹 4. Deployment job

A deployment job is a special job designed to deploy an application to an Azure DevOps environment and track deployment history.

```yaml
jobs:

  - deployment: DeployWebApi

    displayName: "Deploy Web API"

    environment: Production

    strategy:
      runOnce:

        deploy:
          steps:
            - script: echo "Deploying Application"
```

## 🔹 5. `runOnce`

The `runOnce` strategy executes deployment lifecycle steps once for a deployment.

```yaml
strategy:

  runOnce:

    deploy:
      steps:
        - script: echo "Deploy Application"
```

## 🔹 6. `preDeploy`

Runs deployment preparation steps before the deployment operation.

```yaml
strategy:

  runOnce:

    preDeploy:
      steps:
        - script: echo "Preparing deployment"

    deploy:
      steps:
        - script: echo "Deploying application"
```

## 🔹 7. `deploy`

Contains the actual deployment steps of a deployment strategy.

```yaml
strategy:

  runOnce:

    deploy:
      steps:
        - script: echo "Deploying .NET Application"
```

## 🔹 8. `routeTraffic`

Runs steps that route traffic to the newly deployed application version in supported deployment strategies.

```yaml
strategy:

  runOnce:

    deploy:
      steps:
        - script: echo "Deploying new version"

    routeTraffic:
      steps:
        - script: echo "Routing traffic"
```

## 🔹 9. `postRouteTraffic`

Runs verification or monitoring steps after traffic has been routed.

```yaml
strategy:

  runOnce:

    postRouteTraffic:
      steps:
        - script: echo "Checking application health"
```

## 🔹 10. `on: failure` and `on: success`

Defines lifecycle actions that execute after deployment completion or failure.

```yaml
strategy:

  runOnce:

    deploy:
      steps:
        - script: echo "Deploying"

    on:
      failure:
        steps:
          - script: echo "Deployment failed"

      success:
        steps:
          - script: echo "Deployment successful"
```

## 🔹 11. Approvals and checks

Approvals and checks control whether a stage or deployment can access a protected resource.

Configure them from:

```text
Azure DevOps
   |
   v
Pipelines
   |
   v
Environments
   |
   v
Production
   |
   v
Approvals and Checks
```

Common checks include:

| Check | Purpose |
|---|---|
| Approval | Requires an authorized person to approve deployment. |
| Branch control | Restricts deployments to permitted source branches. |
| Business hours | Allows deployment only during approved time windows. |
| Required template | Requires a pipeline to use a specified template. |
| Exclusive lock | Prevents conflicting deployments to the same protected resource. |
| REST API check | Uses an external REST API to evaluate whether a resource can be used. |

## 🔹 12. `ManualValidation@1`

Pauses a pipeline for manual review in an agentless server job.

```yaml
jobs:

  - job: ManualApproval
    pool: server

    steps:

      - task: ManualValidation@1

        inputs:
          notifyUsers: ""
          instructions: "Review the application before production deployment."
          onTimeout: reject

        timeoutInMinutes: 60
```

For production deployments, environment approvals and checks are often preferable because they are configured outside the pipeline YAML and protect the environment resource.

## 🔹 13. `deployment` strategy types

Azure Pipelines supports deployment strategies such as:

| Strategy | Purpose |
|---|---|
| `runOnce` | Deploys an application version once. |
| `rolling` | Updates resources in batches. |
| `canary` | Gradually routes traffic to a new application version. |

Availability and supported lifecycle hooks depend on the deployment target and strategy.

## 🔹 14. Security best practices

| Practice | Recommendation |
|---|---|
| Secrets | Store passwords, tokens, and credentials in secured variable groups or Key Vault. |
| Service Connections | Use least-privilege identities and workload identity federation where supported. |
| Branch Protection | Restrict production deployments to approved branches. |
| Approvals | Require approval before production deployment. |
| Agent Security | Avoid exposing production credentials to untrusted pull request jobs. |
| Permissions | Give pipelines only the permissions required for their tasks. |
| Logs | Never print secrets or sensitive environment values. |
| Artifacts | Deploy the tested artifact rather than rebuilding different code during deployment. |
| Dependencies | Pin task, SDK, and container versions when reproducibility is important. |
| Environments | Separate Development, Staging, and Production deployment targets. |

---

# 15. 🏗️ Complete .NET CI/CD Pipeline – Azure App Service Deployment

This section combines the important Azure Pipelines keywords and tasks into a practical ASP.NET Core Web API CI/CD pipeline.

## 🔹 Project scenario

Assume we have developed an ASP.NET Core Web API using .NET 8 and want to deploy it to Azure App Service automatically whenever code is pushed to the `main` branch.

### Technology stack

| Technology | Purpose |
|---|---|
| ASP.NET Core Web API | Application development |
| .NET 8 SDK | Application build and test |
| NuGet | Dependency management |
| Azure Repos / GitHub | Source code management |
| Azure Pipelines | CI/CD automation |
| Azure App Service | Application hosting |
| Azure Resource Manager Service Connection | Secure Azure authentication |

## 🔹 Repository structure

```text
MyDotNetApplication/
│
├── src/
│   └── MyWebApi/
│       ├── Controllers/
│       ├── Models/
│       ├── Program.cs
│       └── MyWebApi.csproj
│
├── tests/
│   └── MyWebApi.Tests/
│       └── MyWebApi.Tests.csproj
│
├── azure-pipelines.yml
│
└── README.md
```

## 🔹 Pipeline workflow

```text
Developer Pushes Code
        |
        v
Azure Repos / GitHub
        |
        v
Azure Pipeline Trigger
        |
        v
Build Stage
        |
        +---- Install .NET SDK
        |
        +---- Restore NuGet Packages
        |
        +---- Build ASP.NET Core API
        |
        +---- Publish Application
        |
        +---- Publish Pipeline Artifact
        |
        v
Test Stage
        |
        +---- Download Build Artifact
        |
        +---- Run Unit Tests
        |
        v
Deploy Stage
        |
        +---- Download Pipeline Artifact
        |
        +---- Azure Service Connection
        |
        +---- Deploy to Azure App Service
        |
        v
Application Live
```

## 🔹 1. Required Azure resources

Before running the pipeline, create the following resources:

```text
Azure Subscription
      |
      +---- Resource Group
      |
      +---- App Service Plan
      |
      +---- Azure App Service
      |
      +---- Service Connection in Azure DevOps
```

Example resource names:

| Resource | Example Name |
|---|---|
| Resource Group | `rg-dotnet-production` |
| App Service Plan | `asp-dotnet-production` |
| App Service | `my-dotnet-production-app` |
| Service Connection | `Azure-Production-Connection` |
| Azure DevOps Environment | `Production` |

These are placeholder names. Replace them with your actual Azure resource names.

## 🔹 2. Complete `azure-pipelines.yml`

Create this file in the root directory of your repository.

**File:** `azure-pipelines.yml`

```yaml
# ============================================================
# 🚀 AZURE DEVOPS CI/CD PIPELINE
# ASP.NET Core Web API - .NET 8
# Build + Test + Publish + Deploy to Azure App Service
# ============================================================


# ------------------------------------------------------------
# 1. PIPELINE NAME
# ------------------------------------------------------------

name: DotNet-CICD-$(Date:yyyyMMdd).$(Rev:r)


# ------------------------------------------------------------
# 2. TRIGGERS
# ------------------------------------------------------------

# Automatically run when code is pushed to main.
trigger:

  branches:
    include:
      - main


# ------------------------------------------------------------
# 3. GLOBAL VARIABLES
# ------------------------------------------------------------

variables:

  # .NET SDK version
  dotnetVersion: "8.x"

  # Build configuration
  buildConfiguration: Release

  # Application project path
  projectPath: "src/MyWebApi/MyWebApi.csproj"

  # Unit test project path
  testProjectPath: "tests/MyWebApi.Tests/MyWebApi.Tests.csproj"

  # Artifact name
  artifactName: drop

  # NuGet package cache
  NUGET_PACKAGES: $(Pipeline.Workspace)/.nuget/packages


# ------------------------------------------------------------
# 4. DEFAULT AGENT POOL
# ------------------------------------------------------------

pool:

  vmImage: ubuntu-latest


# ------------------------------------------------------------
# 5. PIPELINE STAGES
# ------------------------------------------------------------

stages:


  # ==========================================================
  # STAGE 1: BUILD
  # ==========================================================

  - stage: Build

    displayName: "🏗️ Build and Publish .NET Application"

    jobs:

      - job: BuildJob

        displayName: "Build ASP.NET Core Web API"

        timeoutInMinutes: 30

        steps:


          # --------------------------------------------------
          # CHECKOUT SOURCE CODE
          # --------------------------------------------------

          - checkout: self

            clean: true

            fetchDepth: 1

            displayName: "Checkout Source Code"


          # --------------------------------------------------
          # INSTALL .NET SDK
          # --------------------------------------------------

          - task: UseDotNet@2

            inputs:

              packageType: sdk

              version: $(dotnetVersion)

            displayName: "Install .NET 8 SDK"


          # --------------------------------------------------
          # CACHE NUGET PACKAGES
          # --------------------------------------------------

          - task: Cache@2

            inputs:

              key: 'nuget | "$(Agent.OS)" | **/packages.lock.json,!**/bin/**,!**/obj/**'

              restoreKeys: |
                nuget | "$(Agent.OS)"

              path: $(NUGET_PACKAGES)

            displayName: "Cache NuGet Packages"


          # --------------------------------------------------
          # RESTORE NUGET PACKAGES
          # --------------------------------------------------

          - task: DotNetCoreCLI@2

            inputs:

              command: restore

              projects: $(projectPath)

            displayName: "Restore NuGet Packages"


          # --------------------------------------------------
          # BUILD APPLICATION
          # --------------------------------------------------

          - task: DotNetCoreCLI@2

            inputs:

              command: build

              projects: $(projectPath)

              arguments: >
                --configuration $(buildConfiguration)
                --no-restore

            displayName: "Build ASP.NET Core Application"


          # --------------------------------------------------
          # PUBLISH APPLICATION
          # --------------------------------------------------

          - task: DotNetCoreCLI@2

            inputs:

              command: publish

              publishWebProjects: false

              projects: $(projectPath)

              arguments: >
                --configuration $(buildConfiguration)
                --no-build
                --output $(Build.ArtifactStagingDirectory)/publish

              zipAfterPublish: false

            displayName: "Publish .NET Application"


          # --------------------------------------------------
          # CREATE ZIP PACKAGE
          # --------------------------------------------------

          - task: ArchiveFiles@2

            inputs:

              rootFolderOrFile: $(Build.ArtifactStagingDirectory)/publish

              includeRootFolder: false

              archiveType: zip

              archiveFile: $(Build.ArtifactStagingDirectory)/$(artifactName)/MyWebApi.zip

              replaceExistingArchive: true

            displayName: "Create Deployment ZIP"


          # --------------------------------------------------
          # PUBLISH PIPELINE ARTIFACT
          # --------------------------------------------------

          - task: PublishPipelineArtifact@1

            inputs:

              targetPath: $(Build.ArtifactStagingDirectory)/$(artifactName)

              artifact: $(artifactName)

              publishLocation: pipeline

            displayName: "Publish Pipeline Artifact"



  # ==========================================================
  # STAGE 2: TEST
  # ==========================================================

  - stage: Test

    displayName: "🧪 Run Unit Tests"

    dependsOn: Build

    condition: succeeded()

    jobs:

      - job: TestJob

        displayName: "Execute .NET Unit Tests"

        timeoutInMinutes: 20

        steps:


          # --------------------------------------------------
          # CHECKOUT SOURCE CODE
          # --------------------------------------------------

          - checkout: self

            displayName: "Checkout Test Source Code"


          # --------------------------------------------------
          # INSTALL .NET SDK
          # --------------------------------------------------

          - task: UseDotNet@2

            inputs:

              packageType: sdk

              version: $(dotnetVersion)

            displayName: "Install .NET SDK"


          # --------------------------------------------------
          # RESTORE TEST DEPENDENCIES
          # --------------------------------------------------

          - task: DotNetCoreCLI@2

            inputs:

              command: restore

              projects: $(testProjectPath)

            displayName: "Restore Test Dependencies"


          # --------------------------------------------------
          # BUILD TEST PROJECT
          # --------------------------------------------------

          - task: DotNetCoreCLI@2

            inputs:

              command: build

              projects: $(testProjectPath)

              arguments: >
                --configuration $(buildConfiguration)
                --no-restore

            displayName: "Build Test Project"


          # --------------------------------------------------
          # RUN UNIT TESTS
          # --------------------------------------------------

          - task: DotNetCoreCLI@2

            inputs:

              command: test

              projects: $(testProjectPath)

              arguments: >
                --configuration $(buildConfiguration)
                --no-build
                --logger trx
                --collect "XPlat Code Coverage"

              publishTestResults: true

            displayName: "Run Unit Tests"



  # ==========================================================
  # STAGE 3: DEPLOY
  # ==========================================================

  - stage: Deploy

    displayName: "🚀 Deploy to Azure App Service"

    dependsOn:
      - Build
      - Test

    condition: and(
      succeeded(),
      eq(variables['Build.SourceBranch'], 'refs/heads/main')
    )

    jobs:


      # ------------------------------------------------------
      # DEPLOYMENT JOB
      # ------------------------------------------------------

      - deployment: DeployProduction

        displayName: "Deploy ASP.NET Core API to Production"

        environment: Production


        # ----------------------------------------------------
        # DEPLOYMENT STRATEGY
        # ----------------------------------------------------

        strategy:

          runOnce:

            deploy:

              steps:


                # ------------------------------------------------
                # DOWNLOAD PIPELINE ARTIFACT
                # ------------------------------------------------

                - download: current

                  artifact: $(artifactName)

                  displayName: "Download Application Artifact"


                # ------------------------------------------------
                # DEPLOY TO AZURE APP SERVICE
                # ------------------------------------------------

                - task: AzureWebApp@1

                  inputs:

                    azureSubscription: "Azure-Production-Connection"

                    appType: webAppLinux

                    appName: "my-dotnet-production-app"

                    package: "$(Pipeline.Workspace)/$(artifactName)/MyWebApi.zip"

                    deploymentMethod: auto

                  displayName: "Deploy to Azure App Service"


                # ------------------------------------------------
                # DEPLOYMENT COMPLETED
                # ------------------------------------------------

                - script: echo "Application deployed successfully!"

                  displayName: "Deployment Success Message"
```

## 🔹 3. Configure Azure DevOps Service Connection

The service connection allows Azure Pipelines to authenticate with your Azure subscription and deploy the application.

Steps:

```text
Azure DevOps
    |
    v
Project Settings
    |
    v
Service Connections
    |
    v
New Service Connection
    |
    v
Azure Resource Manager
    |
    v
Workload Identity Federation
    |
    v
Select Subscription
    |
    v
Save
```

Use this name:

```text
Azure-Production-Connection
```

The name must match the `azureSubscription` value in the YAML file.

## 🔹 4. Configure Azure DevOps Environment

Create an environment named:

```text
Production
```

Navigate to:

```text
Azure DevOps
    |
    v
Pipelines
    |
    v
Environments
    |
    v
New Environment
    |
    v
Production
```

You can configure approvals and checks for production deployments.

## 🔹 5. Configure the Azure App Service

Create an App Service compatible with the application package.

For this example:

```text
Runtime Stack: .NET 8
Operating System: Linux
App Service Name: my-dotnet-production-app
```

The `appType` value in the deployment task is:

```yaml
appType: webAppLinux
```

For a Windows App Service, use the corresponding Windows app type supported by the task.

## 🔹 6. Run the Pipeline

After committing the YAML file:

```bash
git add azure-pipelines.yml

git commit -m "Add Azure DevOps CI/CD pipeline"

git push origin main
```

Then:

```text
Azure DevOps
    |
    v
Pipelines
    |
    v
New Pipeline
    |
    v
Select Repository
    |
    v
Existing Azure Pipelines YAML File
    |
    v
/azure-pipelines.yml
    |
    v
Run
```

## 🔹 7. Expected pipeline result

<pre>
Build Stage       ✅ Completed
      |
      v
Test Stage        ✅ Completed
      |
      v
Deploy Stage      ✅ Completed
      |
      v
Azure App Service 🚀 Application Deployed
</pre>

## 🔹 8. Important notes about this example

1. Replace the repository paths with your actual `.csproj` paths.
2. Replace `Azure-Production-Connection` with your actual Azure service connection name.
3. Replace `my-dotnet-production-app` with your actual App Service name.
4. The example uses Linux App Service deployment. Choose the matching task configuration for your hosting operating system.
5. The pipeline publishes a ZIP package and deploys that same package after testing.
6. The test stage restores and builds the test project independently. For large projects, you can optimize this by publishing and reusing compiled test outputs.
7. The NuGet cache works best when package lock files are committed and consistently maintained.
8. Configure the `Production` environment and add approval checks if production deployment requires manual authorization.
9. The example assumes the test project and application project are compatible with the selected .NET SDK.
10. Pipeline Artifacts are intended for Azure DevOps Services. Check compatibility if you use Azure DevOps Server.

---

# 📚 Official Documentation & References

Use the official Microsoft documentation to verify task inputs, supported YAML keywords, and platform-specific features.

| Topic | Documentation |
|---|---|
| Azure Pipelines Overview | [Microsoft Learn](https://learn.microsoft.com/en-us/azure/devops/pipelines/) |
| YAML Schema | [YAML Schema Reference](https://learn.microsoft.com/en-us/azure/devops/pipelines/yaml-schema/) |
| Pipeline Triggers | [Pipeline Triggers](https://learn.microsoft.com/en-us/azure/devops/pipelines/build/triggers) |
| Stages & Dependencies | [Stages](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/stages) |
| Jobs | [Jobs](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/phases) |
| Variables | [Variables](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/variables) |
| Expressions | [Expressions](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/expressions) |
| Conditions | [Conditions](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/conditions) |
| Templates | [Templates](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/templates) |
| Runtime Parameters | [Parameters](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/runtime-parameters) |
| Artifacts | [Pipeline Artifacts](https://learn.microsoft.com/en-us/azure/devops/pipelines/artifacts/pipeline-artifacts) |
| Caching | [Pipeline Caching](https://learn.microsoft.com/en-us/azure/devops/pipelines/release/caching) |
| Environments | [Environments](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/environments) |
| Service Connections | [Service Connections](https://learn.microsoft.com/en-us/azure/devops/pipelines/library/service-endpoints) |
| Azure Key Vault | [Key Vault Integration](https://learn.microsoft.com/en-us/azure/devops/pipelines/release/azure-key-vault) |
| .NET Core Pipeline | [Build .NET Core Apps](https://learn.microsoft.com/en-us/azure/devops/pipelines/ecosystems/dotnet-core) |
| Azure App Service Deployment | [Azure App Service Deployment](https://learn.microsoft.com/en-us/azure/devops/pipelines/tasks/reference/azure-web-app-v1) |
| Azure Pipeline Tasks | [Task Reference](https://learn.microsoft.com/en-us/azure/devops/pipelines/tasks/reference/) |

