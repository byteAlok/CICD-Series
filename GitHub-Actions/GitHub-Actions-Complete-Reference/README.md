<div align="center">

# 🚀 GitHub Actions – Complete Workflow Keywords & Syntax Reference

📚 **Documentation Type:** A beginner-friendly reference for GitHub Actions, workflow events, YAML keywords, nested configurations, expressions, variables, and CI/CD automation.

![GitHub Actions](https://img.shields.io/badge/GitHub-Actions-2088FF?logo=githubactions&logoColor=white)
![YAML](https://img.shields.io/badge/Config-YAML-CB171E?logo=yaml&logoColor=white)
![Level](https://img.shields.io/badge/Level-Beginner%20to%20Advanced-success)
![Topic](https://img.shields.io/badge/Topic-CI%2FCD-blueviolet)

</div>

---
 
> 🛠️ **Technology:** GitHub Actions + YAML  
> 🎯 **Focus:** Workflow keywords, events, nested properties, expressions, variables, reusable workflows, security and deployment  
> 👨‍💻 **Maintained by:** byteAlok | Cloud and DevOps Learner  
> 🧪 **Examples:** Dummy repository names and placeholder values only

---

# 📑 Table of Contents

1. [📘 Introduction](#-1-introduction)
2. [🏗️ Workflow File Structure](#-2-workflow-file-structure)
3. [🏷️ Top-Level Keywords](#-3-top-level-keywords)
4. [⚡ Events – on](#-4-events--on)
5. [🧩 Jobs](#-5-jobs)
6. [🔧 Steps](#-6-steps)
7. [🔍 uses vs run vs with](#-7-uses-vs-run-vs-with)
8. [🪜 Nested Structures](#-8-nested-structures)
9. [🧠 Conditions and Expressions](#-9-conditions-and-expressions)
10. [🌍 Variables and Contexts](#-10-variables-and-contexts)
11. [📤 Outputs and Environment Files](#-11-outputs-and-environment-files)
12. [📦 Strategy and Matrix](#-12-strategy-and-matrix)
13. [🗂️ Artifacts and Caching](#-13-artifacts-and-caching)
14. [🏁 Complete Workflow Example](#-14-complete-workflow-example)
15. [🔗 Important References](#-15-important-references)

---

# 📘 1. Introduction

### 🎯 What you will learn

- What GitHub Actions is and how workflow automation works.
- What workflows, events, jobs, steps, and runners represent.
- How GitHub Actions helps automate CI/CD pipelines.
- How workflow execution progresses after a developer pushes code.

## 1.1 What is GitHub Actions?

GitHub Actions is a CI/CD automation platform that executes configured workflows when specified repository events occur.

It helps developers automate repetitive software development activities without manually executing every command.

### ⚙️ Common use cases

| Use case | Explanation |
|---|---|
| Continuous Integration | Automatically builds and tests application code whenever developers contribute changes. |
| Continuous Delivery | Automatically prepares tested application versions for controlled deployment environments. |
| Continuous Deployment | Automatically deploys successfully tested application changes to configured environments. |
| Code Quality | Runs automated checks to identify formatting, compilation, and quality problems. |
| Security Scanning | Runs automated security tools to identify vulnerabilities in application dependencies and code. |
| Release Automation | Automates application packaging, versioning, and publishing during release activities. |

## 1.2 Important terminology

| Term | Definition |
|---|---|
| Workflow | A YAML-defined automation process that GitHub executes after configured triggering events. |
| Event | An activity occurring inside GitHub that can trigger workflow execution. |
| Job | A collection of related steps executed together on one configured runner environment. |
| Step | An individual task executed inside a job using commands or reusable actions. |
| Action | A reusable automation component that performs a particular development or deployment task. |
| Runner | A machine responsible for executing the jobs and commands defined inside workflows. |
| Artifact | A file or collection of files preserved from a workflow execution for later use. |
| Context | A collection of dynamic information available during workflow expression evaluation. |
| Expression | A dynamic value or condition evaluated by GitHub during workflow processing. |
| Secret | A protected configuration value used for credentials and other sensitive information. |

## 1.3 GitHub Actions execution flow

```text
                    Developer
                        |
                        | Push code
                        v
                GitHub Repository
                        |
                        | Repository Event
                        v
                 Workflow Trigger
                        |
                        v
                 GitHub Actions
                        |
                        v
                     Runner
                        |
             +----------+----------+
             |                     |
             v                     v
          Job 1                  Job 2
             |                     |
             v                     v
          Steps                 Steps
             |                     |
             v                     v
           Build                  Test
             |                     |
             +----------+----------+
                        |
                        v
                    Artifacts
                        |
                        v
                     Deploy
                        |
                        v
                  Application
```

## 1.4 Workflow file location

GitHub automatically discovers workflow YAML files stored inside the `.github/workflows/` directory.

```text
MyApplication/
│
├── .github/
│   │
│   └── workflows/
│       │
│       ├── ci.yml
│       │
│       ├── cd.yml
│       │
│       └── deploy.yml
│
├── src/
│
├── tests/
│
├── MyApplication.sln
│
└── README.md
```

### 📌 Important points

- Workflow files must use `.yml` or `.yaml` extensions.
- Workflow files must be placed inside `.github/workflows/`.
- A repository can contain multiple independent workflow files.
- Each workflow file can have different triggers, jobs, and permissions.
- Multiple workflows can execute simultaneously when their configured events occur.

## 1.5 First workflow

```yaml
# Level 1: Workflow name
name: First GitHub Actions Workflow

# Level 1: Workflow trigger
on:

  # Level 2: Trigger when code is pushed
  push:

    # Level 3: Only monitor the main branch
    branches:
      - main

# Level 1: Jobs collection
jobs:

  # Level 2: Job identifier
  hello-world:

    # Level 3: Select runner machine
    runs-on: ubuntu-latest

    # Level 3: Steps collection
    steps:

      # Level 4: First step
      - name: Display message

        # Level 5: Execute shell command
        run: echo "Hello GitHub Actions"
```

---

# 🏗️ 2. Workflow File Structure

### 🎯 What you will learn

- Understand the complete YAML hierarchy of a workflow.
- Identify top-level keywords and their responsibilities.
- Understand the difference between workflow-level, job-level, and step-level configuration.
- Learn how indentation determines the relationship between YAML properties.

## 2.1 Complete workflow hierarchy

```text
Workflow
│
├── name
│
├── run-name
│
├── on
│   ├── push
│   │   ├── branches
│   │   └── paths
│   │
│   └── pull_request
│       └── types
│
├── permissions
│   ├── contents
│   └── packages
│
├── env
│   ├── APP_NAME
│   └── ENVIRONMENT
│
├── defaults
│   └── run
│       ├── shell
│       └── working-directory
│
├── concurrency
│   ├── group
│   └── cancel-in-progress
│
└── jobs
    │
    ├── build
    │   ├── name
    │   ├── runs-on
    │   ├── permissions
    │   ├── env
    │   ├── outputs
    │   ├── strategy
    │   └── steps
    │       ├── name
    │       ├── id
    │       ├── uses
    │       ├── with
    │       ├── run
    │       ├── env
    │       └── if
    │
    └── deploy
        ├── needs
        ├── environment
        └── steps
```

## 2.2 Workflow-level keywords

Workflow-level keywords are written at the root of the YAML document and configure the complete workflow.

| Keyword | Location | Definition |
|---|---|---|
| `name` | Root | Defines the readable name displayed for the workflow in GitHub Actions. |
| `run-name` | Root | Defines a dynamic name displayed for each individual workflow execution. |
| `on` | Root | Specifies which repository events or manual actions trigger workflow execution. |
| `permissions` | Root | Controls which GitHub resources the workflow's automatically provided token can access. |
| `env` | Root | Defines environment variables available throughout the configured workflow scope. |
| `defaults` | Root | Specifies default execution settings inherited by applicable workflow jobs and steps. |
| `concurrency` | Root | Prevents unnecessary simultaneous workflow or job executions within matching concurrency groups. |
| `jobs` | Root | Contains independently executable jobs that together perform the workflow's automation tasks. |

## 2.3 Complete basic structure

```yaml
# ==================================================
# LEVEL 1: WORKFLOW CONFIGURATION
# ==================================================

# Workflow display name
name: ASP.NET Core CI/CD

# Dynamic workflow execution name
run-name: Build ${{ github.ref_name }} by ${{ github.actor }}


# ==================================================
# LEVEL 1: EVENT CONFIGURATION
# ==================================================

on:

  # Level 2: Push event
  push:

    # Level 3: Branch filter
    branches:
      - main


# ==================================================
# LEVEL 1: TOKEN PERMISSIONS
# ==================================================

permissions:

  # Level 2: Repository content permission
  contents: read


# ==================================================
# LEVEL 1: GLOBAL ENVIRONMENT VARIABLES
# ==================================================

env:

  # Level 2: Application name
  APP_NAME: DemoApplication


# ==================================================
# LEVEL 1: DEFAULT COMMAND CONFIGURATION
# ==================================================

defaults:

  # Level 2: Run configuration
  run:

    # Level 3: Default shell
    shell: bash


# ==================================================
# LEVEL 1: CONCURRENCY CONFIGURATION
# ==================================================

concurrency:

  # Level 2: Group identifier
  group: ${{ github.workflow }}-${{ github.ref }}

  # Level 2: Cancel previous matching execution
  cancel-in-progress: true


# ==================================================
# LEVEL 1: JOBS
# ==================================================

jobs:

  # Level 2: Job identifier
  build:

    # Level 3: Runner
    runs-on: ubuntu-latest

    # Level 3: Steps
    steps:

      # Level 4: Step
      - name: Display application name

        # Level 5: Shell command
        run: echo "$APP_NAME"
```

## 2.4 YAML indentation

YAML indentation determines which property belongs to which parent keyword.

```yaml
# Correct indentation
jobs:

  # Job identifier
  build:

    # Belongs to build job
    runs-on: ubuntu-latest

    # Belongs to build job
    steps:

      # Belongs to steps array
      - name: Build application

        # Belongs to this step
        run: dotnet build
```

### ❌ Incorrect structure

```yaml
jobs:
build:
runs-on: ubuntu-latest
steps:
- name: Build
run: dotnet build
```

**Remember:** YAML uses indentation to represent nesting, so incorrect spacing can change the structure or make a workflow invalid.

---

# 🏷️ 3. Top-Level Keywords

### 🎯 What you will learn

- Understand every commonly used root-level workflow keyword.
- Explore nested properties of permissions, environment variables, defaults, and concurrency.
- Understand workflow-wide settings and their effect on jobs.
- See practical YAML examples with comments showing exact keyword locations.

## 3.1 `name`

Defines the workflow's display name inside the GitHub Actions interface.

### Syntax

```yaml
# Level 1: Root
name: My CI Pipeline
```

### Example

```yaml
# Workflow display name
name: ASP.NET Core Build Pipeline

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Display message
        run: echo "Building application"
```

### Important points

- `name` is optional.
- It appears in the Actions tab.
- It does not determine the workflow filename.
- It does not define a job name.
- Job names can be configured separately.

## 3.2 `run-name`

Defines a custom name for an individual workflow execution using static or dynamic values.

### Syntax

```yaml
# Level 1: Root
run-name: My workflow execution
```

### Dynamic example

```yaml
# Level 1: Root
run-name: Build ${{ github.ref_name }} by ${{ github.actor }}
```

### Common contexts

| Context | Meaning |
|---|---|
| `github.actor` | Identifies the GitHub user associated with the workflow execution. |
| `github.ref_name` | Identifies the short branch or tag name associated with the execution. |
| `github.workflow` | Contains the configured workflow name or workflow filename. |
| `github.run_number` | Contains the sequential run number assigned to the workflow. |

```yaml
# Level 1
run-name: "${{ github.workflow }} - Run #${{ github.run_number }}"
```

## 3.3 `permissions`

Controls the access level granted to the workflow's `GITHUB_TOKEN`.

### Available permission properties

| Property | Definition | Example |
|---|---|---|
| `actions` | Controls access to repository workflow runs and GitHub Actions resources. | `actions: read` |
| `attestations` | Controls access to creating and managing artifact attestations. | `attestations: write` |
| `checks` | Controls access to creating and managing repository check runs. | `checks: write` |
| `contents` | Controls access to reading or modifying repository contents and commits. | `contents: read` |
| `deployments` | Controls access to creating and managing repository deployment records. | `deployments: write` |
| `discussions` | Controls access to reading or modifying repository discussions and comments. | `discussions: write` |
| `id-token` | Controls access to requesting OpenID Connect tokens for external cloud authentication. | `id-token: write` |
| `issues` | Controls access to reading or modifying repository issues and their comments. | `issues: write` |
| `packages` | Controls access to downloading or publishing packages through GitHub Packages. | `packages: write` |
| `pages` | Controls access to managing GitHub Pages deployments and related resources. | `pages: write` |
| `pull-requests` | Controls access to reading or modifying repository pull requests. | `pull-requests: write` |
| `security-events` | Controls access to uploading and managing supported code scanning security results. | `security-events: write` |
| `statuses` | Controls access to creating or modifying commit status information. | `statuses: write` |

### Permission values

| Value | Meaning |
|---|---|
| `read` | Allows reading supported resources without granting modification permissions. |
| `write` | Allows reading and modifying supported resources through the workflow token. |
| `none` | Prevents the workflow token from accessing the specified permission category. |

### Example 1: Workflow-level permissions

```yaml
# Level 1: Root
permissions:

  # Level 2: Repository read access
  contents: read

  # Level 2: Package publishing access
  packages: write
```

### Example 2: Job-level permissions

```yaml
jobs:

  deploy:

    # Level 3: Job-level permission override
    permissions:

      # Level 4
      contents: read

      # Level 4
      deployments: write

    runs-on: ubuntu-latest

    steps:
      - name: Deploy
        run: echo "Deploying application"
```

### Example 3: Disable all permissions

```yaml
# Level 1
permissions: {}

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Display message
        run: echo "No additional token permissions required"
```

### Example 4: Read-only permission for all unspecified categories

```yaml
permissions:
  contents: read
  packages: none
  issues: none
```

**Security recommendation:** Grant only the minimum token permissions needed by each workflow or job.

## 3.4 `env`

Defines environment variables at workflow, job, or individual step level.

### Environment variable hierarchy

```text
Workflow env
    |
    +---- Available to all jobs
              |
              +---- Job env
                        |
                        +---- Available to all steps in that job
                                  |
                                  +---- Step env
                                            |
                                            +---- Available only to that step
```

### Example 1: Workflow-level variables

```yaml
# Level 1: Root
env:

  # Level 2
  APP_NAME: DemoApp

  # Level 2
  ENVIRONMENT: production

  # Level 2
  VERSION: "1.0.0"

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Read workflow variables
        run: |
          echo "$APP_NAME"
          echo "$ENVIRONMENT"
          echo "$VERSION"
```

### Example 2: Job-level variables

```yaml
jobs:
  build:
    runs-on: ubuntu-latest

    # Level 3: Job-level env
    env:

      # Level 4
      BUILD_CONFIGURATION: Release

      # Level 4
      OUTPUT_DIRECTORY: ./publish

    steps:
      - name: Build application
        run: |
          dotnet build --configuration "$BUILD_CONFIGURATION"
```

### Example 3: Step-level variables

```yaml
jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Build application

        # Step-level env
        env:
          CONFIGURATION: Release

        run: dotnet build --configuration "$CONFIGURATION"
```

### Example 4: Same variable name at different levels

A more specific environment variable can override a less specific variable with the same name.

```yaml
# Workflow-level variable
env:
  APP_ENV: development

jobs:
  deploy:
    runs-on: ubuntu-latest

    # Job-level override
    env:
      APP_ENV: staging

    steps:
      - name: Display overridden value

        # Step-level override
        env:
          APP_ENV: production

        run: echo "$APP_ENV"
```

Expected output:

```text
production
```

## 3.5 `defaults`

Defines default settings for commands executed through `run` steps.

### Nested properties

| Parent | Nested property | Definition |
|---|---|---|
| `defaults` | `run` | Groups default configuration settings for shell-based workflow steps. |
| `defaults.run` | `shell` | Specifies which command interpreter executes the configured run commands. |
| `defaults.run` | `working-directory` | Specifies the default directory where shell commands execute. |

### Workflow-level example

```yaml
# Level 1
defaults:

  # Level 2
  run:

    # Level 3
    shell: bash

    # Level 3
    working-directory: ./src

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Build
        run: dotnet build
```

### Job-level example

```yaml
jobs:
  build:
    runs-on: ubuntu-latest

    # Level 3
    defaults:

      # Level 4
      run:

        # Level 5
        shell: bash

        # Level 5
        working-directory: ./src

    steps:
      - name: Build
        run: dotnet build
```

### Important points

- `defaults.run` applies to `run` steps.
- It does not configure an action's internal shell.
- A step can override applicable defaults.
- Workflow-level defaults can be overridden by job-level defaults.

## 3.6 `concurrency`

Controls simultaneous workflow or job executions belonging to matching concurrency groups.

### Nested properties

| Property | Definition |
|---|---|
| `group` | Identifies executions that belong to the same concurrency group. |
| `cancel-in-progress` | Cancels an existing running execution when another matching execution starts. |

### Example 1: Workflow-level concurrency

```yaml
# Level 1
concurrency:

  # Level 2
  group: production-deployment

  # Level 2
  cancel-in-progress: true
```

### Example 2: Dynamic concurrency group

```yaml
concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}
  cancel-in-progress: true
```

### Example 3: Job-level concurrency

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest

    # Job-level concurrency
    concurrency:

      # Nested property
      group: production

      # Nested property
      cancel-in-progress: false

    steps:
      - name: Deploy
        run: echo "Deploying"
```

### Example 4: Multiple branches

```yaml
concurrency:
  group: deploy-${{ github.ref }}
  cancel-in-progress: true
```

This creates separate concurrency groups for different Git references.

---

# ⚡ 4. Events – on

### 🎯 What you will learn

- Understand what the `on` keyword does.
- Learn the major GitHub repository events and their purposes.
- Understand event activity filters and nested configuration.
- Learn branch, tag, path, schedule, and manual input configuration.
- See a YAML example for each commonly used event.

An event is an activity occurring inside GitHub that can trigger workflow execution.

## 4.1 `on` keyword

The `on` keyword defines which events start the workflow.

### Basic syntax

```yaml
# Level 1: Workflow trigger
on:

  # Level 2: Push event
  push:
```

### Multiple events

```yaml
# Level 1
on:

  # Level 2
  push:

  # Level 2
  pull_request:

  # Level 2
  workflow_dispatch:
```

A workflow can listen for multiple events at the same time.

## 4.2 Event configuration keywords

| Keyword | Definition |
|---|---|
| `branches` | Selects specific Git branches whose matching events should trigger workflow execution. |
| `branches-ignore` | Excludes specified Git branches from triggering the configured repository event. |
| `tags` | Selects specific Git tags whose matching events should trigger workflow execution. |
| `tags-ignore` | Excludes specified Git tags from triggering the configured repository event. |
| `paths` | Selects workflows based on matching files changed by the triggering event. |
| `paths-ignore` | Excludes workflows when only specified matching file paths have changed. |
| `types` | Selects specific activity types supported by the configured GitHub event. |
| `cron` | Defines the UTC schedule used to trigger scheduled workflow executions. |
| `inputs` | Defines parameters that users or calling workflows can supply during execution. |
| `workflow` | Identifies another workflow whose execution can trigger the configured workflow. |
| `workflows` | Identifies one or more workflows whose execution can trigger this workflow. |
| `types` | Filters the activity types that should cause the configured event to trigger. |

## 4.3 GitHub events reference table

The following table covers commonly encountered GitHub Actions events and their activity types.

| Event | Definition | Activity types |
|---|---|---|
| `push` | Triggers workflows whenever commits are pushed to matching repository branches or tags. | Push event |
| `pull_request` | Triggers workflows when pull requests are opened, updated, reviewed, or closed. | `opened`, `synchronize`, `reopened`, `closed`, `edited`, `ready_for_review`, `converted_to_draft`, `locked`, `unlocked`, `labeled`, `unlabeled`, `assigned`, `unassigned`, `review_requested`, `review_request_removed`, `auto_merge_enabled`, `auto_merge_disabled`, `enqueued`, `dequeued` |
| `pull_request_target` | Triggers workflows using the base repository context for pull request activities. | `opened`, `synchronize`, `reopened`, `closed`, `edited`, `ready_for_review`, `labeled`, `unlabeled`, `assigned`, `unassigned`, `review_requested`, `review_request_removed` |
| `create` | Triggers workflows whenever a Git branch or tag is created in the repository. | Creation event |
| `delete` | Triggers workflows whenever a Git branch or tag is deleted from the repository. | Deletion event |
| `fork` | Triggers workflows whenever someone creates a fork of the repository. | Fork event |
| `watch` | Triggers workflows when someone stars the repository through GitHub's interface. | `started` |
| `release` | Triggers workflows when repository releases are created, published, or modified. | `published`, `created`, `released`, `edited`, `deleted`, `prereleased`, `unpublished`, `synchronize` |
| `issues` | Triggers workflows when repository issues are opened, closed, assigned, or modified. | `opened`, `closed`, `reopened`, `edited`, `deleted`, `labeled`, `unlabeled`, `assigned`, `unassigned`, `milestoned`, `demilestoned`, `transferred`, `pinned`, `unpinned`, `locked`, `unlocked` |
| `issue_comment` | Triggers workflows when comments are created, edited, or deleted on issues or pull requests. | `created`, `edited`, `deleted` |
| `discussion` | Triggers workflows when repository discussions are created, modified, answered, or closed. | `created`, `edited`, `deleted`, `answered`, `unanswered`, `closed`, `reopened`, `category_changed`, `pinned`, `unpinned`, `locked`, `unlocked`, `transferred` |
| `discussion_comment` | Triggers workflows when comments inside repository discussions are created or modified. | `created`, `edited`, `deleted` |
| `repository_dispatch` | Triggers workflows through external API requests sent to the repository dispatch endpoint. | Custom event types |
| `deployment` | Triggers workflows whenever a deployment is created for the repository. | Deployment event |
| `deployment_status` | Triggers workflows whenever the status of a deployment changes. | Deployment status event |
| `workflow_dispatch` | Allows authorized users to start a workflow manually through GitHub's interface or API. | Manual execution |
| `workflow_call` | Allows another workflow to invoke the current workflow as a reusable workflow. | Workflow invocation |
| `workflow_run` | Triggers workflows after another specified workflow starts or completes execution. | `requested`, `in_progress`, `completed` |
| `schedule` | Automatically triggers workflows according to configured cron expressions using UTC scheduling. | Scheduled execution |
| `status` | Triggers workflows when the status of a Git commit changes. | Status event |
| `check_run` | Triggers workflows when GitHub check runs are created, completed, or requested again. | `created`, `completed`, `rerequested`, `requested_action` |
| `check_suite` | Triggers workflows when GitHub check suites are created or completed. | `completed`, `requested`, `rerequested` |
| `pull_request_review` | Triggers workflows when reviewers submit, edit, or dismiss pull request reviews. | `submitted`, `edited`, `dismissed` |
| `pull_request_review_comment` | Triggers workflows when comments on pull request review conversations change. | `created`, `edited`, `deleted` |
| `commit_comment` | Triggers workflows when comments are added to individual repository commits. | `created` |
| `label` | Triggers workflows when repository labels are created, edited, or deleted. | `created`, `edited`, `deleted` |
| `milestone` | Triggers workflows when repository milestones are created, modified, or deleted. | `created`, `closed`, `opened`, `edited`, `deleted` |
| `page_build` | Triggers workflows when a GitHub Pages site build completes successfully or unsuccessfully. | Page build event |
| `public` | Triggers workflows when a private repository becomes publicly accessible. | Public event |
| `registry_package` | Triggers workflows when a package is published or updated in GitHub's package registry. | `published`, `updated` |
| `gollum` | Triggers workflows when someone creates or updates repository wiki pages. | Wiki event |
| `merge_group` | Triggers workflows when a merge group is created or updated for queued pull requests. | `checks_requested` |
| `branch_protection_rule` | Triggers workflows when repository branch protection rules are created, edited, or deleted. | `created`, `edited`, `deleted` |
| `deployment_protection_rule` | Triggers workflows when deployment protection rules require an external review decision. | `requested` |
| `repository` | Triggers workflows when repository properties or repository settings change. | `created`, `deleted`, `archived`, `unarchived`, `edited`, `renamed`, `transferred`, `publicized`, `privatized` |
| `repository_ruleset` | Triggers workflows when repository rulesets are created, edited, or deleted. | `created`, `edited`, `deleted` |
| `secret_scanning_alert` | Triggers workflows when secret scanning alerts are created or updated. | `created`, `resolved`, `reopened`, `revoked`, `validated`, `location_created` |
| `secret_scanning_alert_location` | Triggers workflows when a location associated with a secret scanning alert changes. | `created` |
| `code_scanning_alert` | Triggers workflows when code scanning alerts are created or updated. | `created`, `fixed`, `dismissed`, `reopened` |
| `package` | Triggers workflows for supported package-related repository activities. | Package event |
| `workflow_job` | Triggers workflows when workflow jobs are queued or completed. | `queued`, `in_progress`, `completed` |
| `member` | Triggers workflows when collaborators are added to or removed from repositories. | `added`, `edited`, `removed` |
| `team_add` | Triggers workflows when a team is added to the repository. | Team addition event |
| `gollum` | Triggers workflows when repository wiki pages are created or edited. | Wiki event |

**Important:** Activity types vary by event. Check the official event reference before using an uncommon activity type.

## 4.4 `push` event

Triggers a workflow whenever matching commits are pushed to the repository.

### Example 1: Every push

```yaml
on:

  # Level 2: Push event
  push:
```

### Example 2: Specific branches

```yaml
on:
  push:

    # Level 3: Include selected branches
    branches:
      - main
      - develop
      - staging
```

### Example 3: Branch patterns

```yaml
on:
  push:

    # Match every branch beginning with feature/
    branches:
      - "feature/**"
```

### Example 4: Exclude branches

```yaml
on:
  push:

    # Exclude matching branches
    branches-ignore:
      - "feature/**"
      - "temporary/**"
```

### Example 5: Include tags

```yaml
on:
  push:

    # Trigger when matching tags are pushed
    tags:
      - "v*"
      - "release-*"
```

### Example 6: Exclude tags

```yaml
on:
  push:

    # Ignore matching tags
    tags-ignore:
      - "beta-*"
      - "test-*"
```

### Example 7: Path filters

```yaml
on:
  push:

    # Trigger when application source files change
    paths:
      - "src/**"
      - "**/*.cs"
      - "*.sln"
```

### Example 8: Ignore paths

```yaml
on:
  push:

    # Ignore documentation-only changes
    paths-ignore:
      - "docs/**"
      - "*.md"
```

### Example 9: Combined branch and path filters

```yaml
on:
  push:

    # Branch condition
    branches:
      - main

    # Path condition
    paths:
      - "src/**"
      - "tests/**"
```

### Example 10: Branch and tag filters

```yaml
on:
  push:

    # Branch filter
    branches:
      - main

    # Tag filter
    tags:
      - "v*"
```

**Important:** A push event with both branch and tag filters can trigger for either matching category; it does not mean both filters must match simultaneously.

## 4.5 `pull_request` event

Triggers a workflow when specified pull request activities occur.

### Example 1: Every pull request activity by default

```yaml
on:
  pull_request:
```

### Example 2: Selected target branches

```yaml
on:
  pull_request:

    # Target branches receiving pull requests
    branches:
      - main
      - develop
```

### Example 3: Selected activity types

```yaml
on:
  pull_request:

    # Activity type filter
    types:
      - opened
      - synchronize
      - reopened
      - closed
```

### Example 4: Branch and activity filters together

```yaml
on:
  pull_request:

    # Target branch
    branches:
      - main

    # Pull request activities
    types:
      - opened
      - synchronize
      - reopened
```

### Example 5: Path filtering

```yaml
on:
  pull_request:

    # Only matching target branch
    branches:
      - main

    # Only matching changed files
    paths:
      - "src/**"
      - "tests/**"
```

### Important activity types

| Activity | Meaning |
|---|---|
| `opened` | A new pull request has been created in the repository. |
| `synchronize` | New commits have been pushed to an existing pull request branch. |
| `reopened` | A previously closed pull request has been opened again. |
| `closed` | An existing pull request has been closed or merged. |
| `edited` | The pull request title or description has been modified. |
| `ready_for_review` | A draft pull request has become ready for review. |
| `converted_to_draft` | An existing pull request has been converted into draft status. |
| `review_requested` | A reviewer or team has been requested to review the pull request. |
| `review_request_removed` | A previously requested reviewer or team has been removed. |
| `labeled` | A label has been attached to the pull request. |
| `unlabeled` | A previously attached label has been removed. |
| `assigned` | A user has been assigned to the pull request. |
| `unassigned` | A previously assigned user has been removed from the pull request. |

## 4.6 `pull_request_target` event

Runs workflows in the base repository context when supported pull request activities occur.

### Example

```yaml
on:
  pull_request_target:

    # Activity type filters
    types:
      - opened
      - synchronize
      - reopened

    # Target branches
    branches:
      - main
```

### Security considerations

- This event can receive access to privileged base-repository resources.
- Pull request content can contain untrusted code and user-controlled data.
- Never check out and execute untrusted pull request code using privileged credentials.
- Avoid exposing secrets to untrusted pull request content.

## 4.7 `create` event

Triggers when a Git branch or tag is created in the repository.

```yaml
on:

  # Level 2: Branch or tag creation
  create:
```

## 4.8 `delete` event

Triggers when a Git branch or tag is deleted from the repository.

```yaml
on:

  # Level 2: Branch or tag deletion
  delete:
```

## 4.9 `fork` event

Triggers when someone creates a fork of the configured repository.

```yaml
on:

  # Level 2: Fork event
  fork:
```

## 4.10 `watch` event

Triggers when someone stars the repository through GitHub's interface.

```yaml
on:
  watch:

    # Level 3: Star activity
    types:
      - started
```

**Remember:** `watch` with `started` represents a repository star activity.

## 4.11 `release` event

Triggers workflows when supported repository release activities occur.

```yaml
on:
  release:

    # Level 3: Release activity types
    types:
      - published
      - created
      - released
      - edited
      - deleted
      - prereleased
      - unpublished
```

### Common release activities

| Activity | Definition |
|---|---|
| `created` | A new release record has been created in the repository. |
| `published` | A release has been published and made available to users. |
| `released` | A release has entered the released lifecycle state. |
| `edited` | Existing release information has been modified by an authorized user. |
| `deleted` | An existing release record has been removed from the repository. |
| `prereleased` | A release has been marked as a prerelease version for testing. |
| `unpublished` | A previously published release has been changed to unpublished status. |

## 4.12 `issues` event

Triggers workflows when repository issues experience supported activity changes.

```yaml
on:
  issues:

    # Level 3: Issue activity filters
    types:
      - opened
      - closed
      - reopened
      - edited
      - labeled
      - unlabeled
      - assigned
      - unassigned
      - milestoned
      - demilestoned
      - deleted
      - locked
      - unlocked
```

### Example: Run only when an issue is opened

```yaml
on:
  issues:
    types:
      - opened

jobs:
  issue-created:
    runs-on: ubuntu-latest

    steps:
      - name: Display issue information
        run: echo "A new issue was opened"
```

## 4.13 `issue_comment` event

Triggers workflows when comments are created, edited, or deleted on issues or pull requests.

```yaml
on:
  issue_comment:

    # Level 3
    types:
      - created
      - edited
      - deleted
```

### Example: React to newly created comments

```yaml
on:
  issue_comment:
    types:
      - created

jobs:
  comment:
    runs-on: ubuntu-latest

    steps:
      - name: Display comment activity
        run: echo "A comment was created"
```

## 4.14 `discussion` event

Triggers workflows when repository discussions experience supported community activities.

```yaml
on:
  discussion:

    # Level 3
    types:
      - created
      - edited
      - deleted
      - answered
      - unanswered
      - closed
      - reopened
      - category_changed
      - pinned
      - unpinned
      - locked
      - unlocked
```

## 4.15 `discussion_comment` event

Triggers workflows when discussion comments are created, edited, or deleted.

```yaml
on:
  discussion_comment:

    # Level 3
    types:
      - created
      - edited
      - deleted
```

## 4.16 `repository_dispatch` event

Allows an external application to trigger a workflow using GitHub's repository dispatch API.

### Workflow configuration

```yaml
on:
  repository_dispatch:

    # Level 3: Custom event type
    types:
      - deploy_application
      - run_tests
```

### Example API request

```bash
# Send an external repository dispatch request
curl -X POST \
  -H "Accept: application/vnd.github+json" \
  -H "Authorization: Bearer YOUR_GITHUB_TOKEN" \
  https://api.github.com/repos/OWNER/REPOSITORY/dispatches \
  -d '{"event_type":"deploy_application"}'
```

### Access custom event information

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Read dispatch event
        run: echo "${{ github.event.action }}"
```

## 4.17 `deployment` event

Triggers when a deployment record is created for the repository.

```yaml
on:

  # Level 2
  deployment:
```

## 4.18 `deployment_status` event

Triggers when the status of an existing deployment changes.

```yaml
on:

  # Level 2
  deployment_status:
```

## 4.19 `workflow_dispatch` event

Allows users to manually execute a workflow through GitHub's interface or API.

### Nested input properties

| Property | Definition |
|---|---|
| `inputs` | Contains the input parameters users can provide during manual workflow execution. |
| `description` | Explains the purpose of an individual input parameter to workflow users. |
| `required` | Determines whether users must provide a value before starting the workflow. |
| `type` | Defines which input data type or selection interface GitHub should provide. |
| `default` | Supplies the initial value when users do not provide another value. |
| `options` | Defines the permitted choices available for choice-type manual workflow inputs. |

### Supported input types

| Type | Definition |
|---|---|
| `string` | Accepts a text value supplied by the user during manual workflow execution. |
| `choice` | Presents a predefined list of selectable values for the workflow input. |
| `boolean` | Accepts a true or false value from the manual workflow interface. |
| `number` | Accepts a numeric value supplied during manual workflow execution. |
| `environment` | Allows the user to select an available GitHub deployment environment. |

### Complete example

```yaml
on:
  workflow_dispatch:

    # Level 3: Input collection
    inputs:

      # Level 4: Environment input
      environment:
        description: "Select deployment environment"
        required: true
        type: choice

        # Level 5: Available choices
        options:
          - development
          - staging
          - production

      # Level 4: Version input
      version:
        description: "Application version"
        required: false
        type: string
        default: "1.0.0"

      # Level 4: Boolean input
      run_tests:
        description: "Run automated tests"
        required: true
        type: boolean
        default: true

      # Level 4: Numeric input
      replica_count:
        description: "Number of application replicas"
        required: false
        type: number
        default: 2

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Display selected inputs
        run: |
          echo "Environment: ${{ inputs.environment }}"
          echo "Version: ${{ inputs.version }}"
          echo "Tests: ${{ inputs.run_tests }}"
          echo "Replicas: ${{ inputs.replica_count }}"
```

## 4.20 `workflow_call` event

Allows another workflow to invoke the current workflow as a reusable workflow.

### Nested properties

| Property | Definition |
|---|---|
| `inputs` | Declares values that calling workflows can pass to the reusable workflow. |
| `secrets` | Declares sensitive values that calling workflows can provide securely. |
| `outputs` | Exposes reusable workflow results for the calling workflow to consume. |
| `description` | Explains the purpose of an individual reusable workflow input or secret. |
| `required` | Determines whether the calling workflow must provide the specified input or secret. |
| `type` | Defines the supported data type of a reusable workflow input. |
| `default` | Supplies the default value when an optional reusable workflow input is omitted. |
| `value` | Maps a reusable workflow output to a value produced by an internal job. |

### Reusable workflow example

```yaml
# File: .github/workflows/reusable-build.yml

name: Reusable Build Workflow

on:
  workflow_call:

    # Level 3: Input definitions
    inputs:
      environment:
        description: "Deployment environment"
        required: true
        type: string

    # Level 3: Secret definitions
    secrets:
      DEPLOY_TOKEN:
        description: "Deployment credential"
        required: true

    # Level 3: Workflow outputs
    outputs:
      build_version:
        description: "Generated application version"

        # Level 4: Output value
        value: ${{ jobs.build.outputs.version }}

jobs:
  build:
    runs-on: ubuntu-latest

    outputs:
      version: ${{ steps.version.outputs.value }}

    steps:
      - name: Generate version
        id: version
        run: echo "value=1.0.0" >> "$GITHUB_OUTPUT"
```

### Calling workflow example

```yaml
# File: .github/workflows/main.yml

name: Main Workflow

on:
  workflow_dispatch:

jobs:
  build:
    # Job-level reusable workflow invocation
    uses: ./.github/workflows/reusable-build.yml

    # Reusable workflow inputs
    with:
      environment: production

    # Reusable workflow secrets
    secrets:
      DEPLOY_TOKEN: ${{ secrets.DEPLOY_TOKEN }}
```

## 4.21 `workflow_run` event

Triggers a workflow after another selected workflow has started or completed execution.

```yaml
on:
  workflow_run:

    # Level 3: Workflow names to monitor
    workflows:
      - CI Pipeline
      - Test Pipeline

    # Level 3: Activity types
    types:
      - completed

    # Level 3: Branch filter
    branches:
      - main
```

## 4.22 `schedule` event

Runs workflows automatically according to UTC cron expressions.

### Cron syntax

```text
┌───────────── Minute       (0 - 59)
│ ┌─────────── Hour         (0 - 23)
│ │ ┌───────── Day of month (1 - 31)
│ │ │ ┌─────── Month        (1 - 12)
│ │ │ │ ┌───── Day of week  (0 - 6)
│ │ │ │ │
* * * * *
```

### Common cron examples

| Cron expression | Meaning |
|---|---|
| `0 2 * * *` | Runs every day at 02:00 UTC. |
| `0 9 * * 1` | Runs every Monday at 09:00 UTC. |
| `0 0 1 * *` | Runs on the first day of every month. |
| `*/15 * * * *` | Runs every fifteen minutes. |
| `30 18 * * 5` | Runs every Friday at 18:30 UTC. |
| `0 */6 * * *` | Runs every six hours. |

### YAML example

```yaml
on:
  schedule:

    # Run daily at 02:00 UTC
    - cron: "0 2 * * *"

    # Run every Monday at 09:00 UTC
    - cron: "0 9 * * 1"
```

**Important:** Scheduled workflows can be delayed during periods of high GitHub Actions load. The default branch must contain the scheduled workflow.

## 4.23 Other event examples

### `status`

```yaml
on:
  status:
```

### `check_run`

```yaml
on:
  check_run:
    types:
      - created
      - completed
      - rerequested
      - requested_action
```

### `check_suite`

```yaml
on:
  check_suite:
    types:
      - completed
      - requested
      - rerequested
```

### `pull_request_review`

```yaml
on:
  pull_request_review:
    types:
      - submitted
      - edited
      - dismissed
```

### `pull_request_review_comment`

```yaml
on:
  pull_request_review_comment:
    types:
      - created
      - edited
      - deleted
```

### `commit_comment`

```yaml
on:
  commit_comment:
    types:
      - created
```

### `label`

```yaml
on:
  label:
    types:
      - created
      - edited
      - deleted
```

### `milestone`

```yaml
on:
  milestone:
    types:
      - created
      - opened
      - closed
      - edited
      - deleted
```

### `page_build`

```yaml
on:
  page_build:
```

### `public`

```yaml
on:
  public:
```

### `registry_package`

```yaml
on:
  registry_package:
    types:
      - published
      - updated
```

### `gollum`

```yaml
on:
  gollum:
```

### `merge_group`

```yaml
on:
  merge_group:
    types:
      - checks_requested
```

### `branch_protection_rule`

```yaml
on:
  branch_protection_rule:
    types:
      - created
      - edited
      - deleted
```

### `deployment_protection_rule`

```yaml
on:
  deployment_protection_rule:
    types:
      - requested
```

### `repository`

```yaml
on:
  repository:
    types:
      - created
      - deleted
      - archived
      - unarchived
      - edited
      - renamed
      - transferred
```

### `repository_ruleset`

```yaml
on:
  repository_ruleset:
    types:
      - created
      - edited
      - deleted
```

### `secret_scanning_alert`

```yaml
on:
  secret_scanning_alert:
    types:
      - created
      - resolved
      - reopened
      - revoked
      - validated
      - location_created
```

### `code_scanning_alert`

```yaml
on:
  code_scanning_alert:
    types:
      - created
      - fixed
      - dismissed
      - reopened
```

### `workflow_job`

```yaml
on:
  workflow_job:
    types:
      - queued
      - in_progress
      - completed
```

### `member`

```yaml
on:
  member:
    types:
      - added
      - edited
      - removed
```

### `team_add`

```yaml
on:
  team_add:
```

---

# 🧩 5. Jobs

### 🎯 What you will learn

- Understand the `jobs` keyword and job identifiers.
- Learn all important job-level configuration properties.
- Understand job dependencies through `needs`.
- Learn job outputs, permissions, environments, and execution timeouts.
- Understand job containers and service containers.

A job represents a collection of steps executed together on one configured runner.

## 5.1 `jobs`

The `jobs` keyword contains all jobs defined inside a workflow.

### Syntax

```yaml
# Level 1: Root
jobs:

  # Level 2: Job identifier
  build:

    # Level 3: Job properties
    runs-on: ubuntu-latest

    # Level 3: Steps collection
    steps:
      - name: Display message
        run: echo "Build started"
```

## 5.2 Job identifier

A job identifier is a unique key used to identify an individual job within the workflow.

```yaml
jobs:

  # Job identifier: build
  build:
    runs-on: ubuntu-latest

    steps:
      - run: echo "Build"

  # Job identifier: test
  test:
    runs-on: ubuntu-latest

    steps:
      - run: echo "Test"
```

The identifiers `build` and `test` can be referenced by other job properties.

## 5.3 Job-level keywords

| Keyword | Definition |
|---|---|
| `name` | Defines the human-readable name displayed for an individual workflow job. |
| `runs-on` | Specifies the runner machine or runner group that executes the job. |
| `needs` | Specifies other jobs that must complete successfully before this job starts. |
| `if` | Evaluates an expression to determine whether GitHub should execute the job. |
| `steps` | Contains the ordered commands and reusable actions executed inside the job. |
| `env` | Defines environment variables accessible throughout the individual job's execution. |
| `outputs` | Exposes selected job results for dependent jobs to consume later. |
| `permissions` | Overrides workflow-level token permissions for the individual configured job. |
| `environment` | Associates a job with a repository environment and its deployment protection rules. |
| `timeout-minutes` | Sets the maximum allowed execution time before GitHub terminates the job. |
| `continue-on-error` | Allows a job failure without automatically failing the complete workflow execution. |
| `defaults` | Defines default execution settings inherited by the job's applicable run steps. |
| `strategy` | Configures matrix execution and related job scheduling behavior for multiple combinations. |
| `concurrency` | Controls simultaneous execution of jobs belonging to matching concurrency groups. |
| `container` | Runs the job inside a specified container image instead of directly on the runner. |
| `services` | Defines additional service containers such as databases required during job execution. |

## 5.4 `name`

Defines the readable name displayed for an individual job in the Actions interface.

```yaml
jobs:
  build:

    # Job display name
    name: Build Application

    runs-on: ubuntu-latest

    steps:
      - run: echo "Building"
```

## 5.5 `runs-on`

Specifies which runner machine executes the job.

### GitHub-hosted runner labels

| Runner | Definition |
|---|---|
| `ubuntu-latest` | Executes the job on the latest supported Ubuntu GitHub-hosted runner image. |
| `ubuntu-24.04` | Executes the job on the specified Ubuntu 24.04 runner image. |
| `windows-latest` | Executes the job on the latest supported Windows GitHub-hosted runner image. |
| `windows-2025` | Executes the job on the specified Windows Server 2025 runner image. |
| `macos-latest` | Executes the job on the latest supported macOS GitHub-hosted runner image. |

### Example 1: Ubuntu

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
```

### Example 2: Windows

```yaml
jobs:
  build:
    runs-on: windows-latest
```

### Example 3: macOS

```yaml
jobs:
  build:
    runs-on: macos-latest
```

### Example 4: Self-hosted runner

```yaml
jobs:
  deploy:
    runs-on: self-hosted
```

### Example 5: Self-hosted runner labels

```yaml
jobs:
  deploy:

    # Select a self-hosted runner with all matching labels
    runs-on: [self-hosted, linux, x64, production]
```

### Example 6: Runner groups

```yaml
jobs:
  deploy:
    runs-on:

      # Runner group selection
      group: production-runners

      # Additional runner labels
      labels: linux
```

## 5.6 `needs`

Defines job dependencies so dependent jobs wait for prerequisite jobs to finish.

### Example 1: Single dependency

```yaml
jobs:

  # Job 1
  build:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Building application"

  # Job 2 waits for build
  test:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - run: echo "Running tests"
```

### Example 2: Multiple dependencies

```yaml
jobs:

  build:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Build"

  test:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Test"

  # Wait for both build and test
  deploy:
    needs:
      - build
      - test

    runs-on: ubuntu-latest

    steps:
      - run: echo "Deploy"
```

### Example 3: Dependency chain

```yaml
jobs:

  # First job
  build:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Build"

  # Second job depends on build
  test:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - run: echo "Test"

  # Third job depends on test
  deploy:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - run: echo "Deploy"
```

### Example 4: Parallel jobs

```yaml
jobs:

  build:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Build"

  test:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Test"

  security:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Security scan"
```

All three jobs can run in parallel because none declares a dependency on another.

## 5.7 `if`

Controls whether a complete job should execute based on an evaluated expression.

### Example 1: Branch condition

```yaml
jobs:
  deploy:

    # Job-level condition
    if: github.ref == 'refs/heads/main'

    runs-on: ubuntu-latest

    steps:
      - run: echo "Deploying main branch"
```

### Example 2: Event condition

```yaml
jobs:
  pull-request:
    if: github.event_name == 'pull_request'
    runs-on: ubuntu-latest

    steps:
      - run: echo "Pull request workflow"
```

### Example 3: Combined conditions

```yaml
jobs:
  deploy:

    # Both conditions must be true
    if: github.ref == 'refs/heads/main' && github.event_name == 'push'

    runs-on: ubuntu-latest

    steps:
      - run: echo "Deploying"
```

### Example 4: Failure condition

```yaml
jobs:
  notify:

    # Execute after a failed dependency
    if: failure()

    runs-on: ubuntu-latest

    steps:
      - run: echo "A previous job failed"
```

## 5.8 `steps`

Contains the ordered actions and commands executed inside a job.

```yaml
jobs:
  build:
    runs-on: ubuntu-latest

    # Job-level steps collection
    steps:

      # Step 1
      - name: Checkout
        uses: actions/checkout@v4

      # Step 2
      - name: Display repository
        run: echo "Repository checked out"

      # Step 3
      - name: Build
        run: dotnet build
```

## 5.9 `env`

Defines environment variables available throughout the current job's steps.

```yaml
jobs:
  build:
    runs-on: ubuntu-latest

    # Job-level environment variables
    env:
      CONFIGURATION: Release
      APP_NAME: DemoApp

    steps:
      - name: Build
        run: dotnet build --configuration "$CONFIGURATION"

      - name: Display app
        run: echo "$APP_NAME"
```

## 5.10 `outputs`

Exposes values generated by job steps so dependent jobs can consume them.

### Output hierarchy

```text
jobs
│
├── build
│   ├── outputs
│   │   └── version
│   │       └── steps.version.outputs.value
│   │
│   └── steps
│       └── version
│           └── GITHUB_OUTPUT
│
└── deploy
    ├── needs
    │   └── build
    │
    └── steps
        └── needs.build.outputs.version
```

### Complete example

```yaml
jobs:

  build:
    runs-on: ubuntu-latest

    # Job-level outputs
    outputs:

      # Output name: version
      version: ${{ steps.generate.outputs.value }}

    steps:

      # Generate output
      - name: Generate version
        id: generate

        # Write output to GitHub output file
        run: echo "value=1.0.0" >> "$GITHUB_OUTPUT"

  deploy:
    runs-on: ubuntu-latest

    # Dependency on build
    needs: build

    steps:
      - name: Read job output

        # Read output from prerequisite job
        run: echo "${{ needs.build.outputs.version }}"
```

## 5.11 `permissions`

Defines token permissions for an individual job.

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest

    # Job-level permission configuration
    permissions:
      contents: read
      deployments: write
      id-token: write

    steps:
      - run: echo "Deploying"
```

## 5.12 `environment`

Associates a job with a repository deployment environment.

### Nested properties

| Property | Definition |
|---|---|
| `name` | Identifies the repository environment associated with the configured deployment job. |
| `url` | Specifies the URL associated with the configured deployment environment or deployment result. |

### Example

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest

    # Job-level deployment environment
    environment:

      # Environment name
      name: production

      # Deployment URL
      url: https://example.com

    steps:
      - name: Deploy
        run: echo "Production deployment"
```

### Using a dynamic URL

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest

    environment:
      name: production
      url: https://example.com/releases/${{ github.run_number }}

    steps:
      - run: echo "Deployment completed"
```

## 5.13 `timeout-minutes`

Sets the maximum execution duration allowed for a job before GitHub terminates it.

```yaml
jobs:
  build:
    runs-on: ubuntu-latest

    # Maximum job execution time
    timeout-minutes: 20

    steps:
      - run: dotnet build
```

## 5.14 `continue-on-error`

Allows a job failure without automatically failing the entire workflow execution.

```yaml
jobs:
  experimental:
    runs-on: ubuntu-latest

    # Continue workflow even if this job fails
    continue-on-error: true

    steps:
      - run: echo "Experimental job"
```

## 5.15 `defaults`

Defines default shell and working-directory settings for the job's run steps.

```yaml
jobs:
  build:
    runs-on: ubuntu-latest

    # Job-level defaults
    defaults:
      run:

        # Default shell
        shell: bash

        # Default working directory
        working-directory: ./src

    steps:
      - name: Build
        run: dotnet build
```

## 5.16 `concurrency`

Controls overlapping execution of the configured job.

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest

    # Job-level concurrency
    concurrency:

      # Matching group
      group: production-deployment

      # Cancellation policy
      cancel-in-progress: true

    steps:
      - run: echo "Deploying"
```

## 5.17 `container`

Runs a complete job inside a specified container image.

### Nested properties

| Property | Definition |
|---|---|
| `image` | Identifies the Docker image used to create the job container. |
| `credentials` | Supplies registry authentication credentials required to download private container images. |
| `env` | Defines environment variables available inside the configured job container. |
| `ports` | Publishes container ports for communication with other job processes. |
| `volumes` | Mounts directories or named volumes into the running job container. |
| `options` | Passes additional supported Docker container creation options to the runner. |

### Example

```yaml
jobs:
  build:
    runs-on: ubuntu-latest

    # Job-level container configuration
    container:

      # Container image
      image: node:22

      # Container environment variables
      env:
        NODE_ENV: production

      # Published ports
      ports:
        - 3000

      # Mounted volumes
      volumes:
        - my-volume:/data

    steps:
      - name: Display Node version
        run: node --version
```

### Private image example

```yaml
jobs:
  build:
    runs-on: ubuntu-latest

    container:
      image: ghcr.io/example/private-image:1.0

      # Registry credentials
      credentials:
        username: ${{ github.actor }}
        password: ${{ secrets.GITHUB_TOKEN }}

    steps:
      - run: echo "Running inside private image"
```

## 5.18 `services`

Defines supporting containers such as databases that a job needs during execution.

### Nested properties

| Property | Definition |
|---|---|
| `image` | Specifies the container image used to start the supporting service. |
| `credentials` | Supplies authentication information for downloading private service container images. |
| `env` | Defines environment variables required by the supporting service container. |
| `ports` | Maps service container ports to accessible runner network ports. |
| `volumes` | Mounts additional storage locations into the supporting service container. |
| `options` | Supplies additional Docker container creation options for the supporting service. |

### MySQL service example

```yaml
jobs:
  test:
    runs-on: ubuntu-latest

    # Job-level services
    services:

      # Service identifier
      mysql:

        # Service image
        image: mysql:8

        # Service environment variables
        env:
          MYSQL_ROOT_PASSWORD: example
          MYSQL_DATABASE: testdb

        # Port mapping
        ports:
          - 3306:3306

        # Additional container options
        options: >-
          --health-cmd="mysqladmin ping"
          --health-interval=10s
          --health-timeout=5s
          --health-retries=5

    steps:
      - name: Run tests
        run: echo "Run database integration tests"
```

## 5.19 `strategy`

Controls how GitHub schedules multiple executions of a job.

```yaml
jobs:
  test:
    runs-on: ubuntu-latest

    # Job-level strategy
    strategy:

      # Matrix configuration
      matrix:
        version:
          - "8.0"
          - "9.0"

      # Stop remaining jobs after a failure
      fail-fast: true

      # Maximum parallel executions
      max-parallel: 2

    steps:
      - run: echo "${{ matrix.version }}"
```

Detailed matrix examples are provided in Section 12.

---

# 🔧 6. Steps

### 🎯 What you will learn

- Understand the `steps` array and its execution order.
- Learn every commonly used step-level keyword.
- Understand step identifiers and outputs.
- Learn action inputs, shell configuration, and working directories.
- Understand conditional execution and error handling.

Steps execute sequentially inside a job and represent individual automation tasks.

## 6.1 Basic step structure

```yaml
jobs:
  build:
    runs-on: ubuntu-latest

    # Job-level steps collection
    steps:

      # Step 1
      - name: Checkout repository
        uses: actions/checkout@v4

      # Step 2
      - name: Display message
        run: echo "Hello"

      # Step 3
      - name: Build project
        run: dotnet build
```

## 6.2 Step-level keywords

| Keyword | Definition |
|---|---|
| `name` | Defines a readable description for an individual workflow execution step. |
| `id` | Assigns a unique identifier that allows other steps to reference its outputs. |
| `uses` | Executes an existing action or invokes an action from a specified location. |
| `run` | Executes one or multiple shell commands directly on the configured runner. |
| `with` | Supplies named input parameters required by the selected action or workflow. |
| `env` | Defines environment variables available only within the configured execution step. |
| `if` | Determines whether GitHub should execute the individual step based on an expression. |
| `shell` | Specifies the command interpreter used to execute the step's shell commands. |
| `working-directory` | Specifies the directory where commands inside the individual step execute. |
| `timeout-minutes` | Sets the maximum execution time allowed for the individual workflow step. |
| `continue-on-error` | Allows execution to continue when the configured step encounters an error. |

## 6.3 `name`

Defines a readable description for an individual workflow step.

```yaml
steps:
  - name: Checkout source code
    uses: actions/checkout@v4
```

## 6.4 `id`

Assigns a unique identifier so later expressions can reference the step's outputs.

```yaml
steps:
  - name: Generate application version

    # Step identifier
    id: version

    run: echo "value=1.0.0" >> "$GITHUB_OUTPUT"

  - name: Read application version

    # Access previous step output
    run: echo "${{ steps.version.outputs.value }}"
```

### Multiple identifiers

```yaml
steps:

  - name: Generate build number
    id: build_number
    run: echo "value=100" >> "$GITHUB_OUTPUT"

  - name: Generate release version
    id: release_version
    run: echo "value=1.0.0" >> "$GITHUB_OUTPUT"

  - name: Display both outputs
    run: |
      echo "${{ steps.build_number.outputs.value }}"
      echo "${{ steps.release_version.outputs.value }}"
```

## 6.5 `uses`

Executes a reusable action from a repository, local path, or supported container image.

```yaml
steps:
  - name: Checkout repository
    uses: actions/checkout@v4

  - name: Setup .NET
    uses: actions/setup-dotnet@v4
    with:
      dotnet-version: "8.0.x"
```

## 6.6 `with`

Supplies input parameters declared by the selected action.

```yaml
steps:
  - name: Checkout repository
    uses: actions/checkout@v4

    # Action input parameters
    with:

      # Fetch complete Git history
      fetch-depth: 0

      # Clean workspace before checkout
      clean: true
```

## 6.7 `run`

Executes shell commands directly on the runner.

### Single command

```yaml
steps:
  - name: Display message
    run: echo "Hello"
```

### Multiple commands

```yaml
steps:
  - name: Build application
    run: |
      dotnet restore
      dotnet build
      dotnet test
```

### Command block using `|`

```yaml
steps:
  - name: Execute multiple commands

    # YAML literal block
    run: |
      echo "First command"
      echo "Second command"
      echo "Third command"
```

### Folded block using `>`

```yaml
steps:
  - name: Execute long command

    # YAML folded block
    run: >
      echo "This is a long command
      written across multiple YAML lines"
```

### Command with shell variables

```yaml
steps:
  - name: Display environment variable
    run: |
      APP_NAME="DemoApp"
      echo "$APP_NAME"
```

## 6.8 `env`

Defines environment variables available only to the configured step.

```yaml
steps:
  - name: Build application

    # Step-level environment
    env:
      BUILD_CONFIGURATION: Release
      APP_VERSION: "1.0.0"

    run: |
      echo "$BUILD_CONFIGURATION"
      echo "$APP_VERSION"
```

## 6.9 `if`

Controls whether an individual step should execute.

### Example 1: Success condition

```yaml
steps:
  - name: Build
    run: dotnet build

  - name: Run after success
    if: success()
    run: echo "Build successful"
```

### Example 2: Failure condition

```yaml
steps:
  - name: Build
    run: dotnet build

  - name: Show error message
    if: failure()
    run: echo "Build failed"
```

### Example 3: Always execute

```yaml
steps:
  - name: Cleanup
    if: always()
    run: echo "Cleanup process"
```

### Example 4: Branch condition

```yaml
steps:
  - name: Deploy only on main
    if: github.ref == 'refs/heads/main'
    run: echo "Deploying main"
```

## 6.10 `shell`

Specifies the shell interpreter used to execute commands.

| Shell | Definition |
|---|---|
| `bash` | Executes shell commands using Bash on supported Linux and macOS runners. |
| `sh` | Executes commands using the standard POSIX-compatible shell interpreter. |
| `pwsh` | Executes PowerShell Core commands on supported runner operating systems. |
| `powershell` | Executes Windows PowerShell commands on supported Windows runner machines. |
| `cmd` | Executes commands using the Windows Command Prompt interpreter on Windows runners. |

### Bash example

```yaml
steps:
  - name: Execute Bash
    shell: bash
    run: |
      echo "Linux command"
      pwd
```

### PowerShell example

```yaml
steps:
  - name: Execute PowerShell
    shell: pwsh
    run: |
      Write-Output "PowerShell command"
      Get-Location
```

### Windows CMD example

```yaml
steps:
  - name: Execute CMD
    shell: cmd
    run: |
      echo Windows command
      cd
```

## 6.11 `working-directory`

Specifies the directory where the step's shell commands execute.

```yaml
steps:
  - name: Build application

    # Step-level working directory
    working-directory: ./src

    run: dotnet build
```

### Nested project directory

```yaml
steps:
  - name: Build API

    working-directory: ./src/MyApi

    run: dotnet build MyApi.csproj
```

## 6.12 `timeout-minutes`

Sets the maximum allowed execution time for the configured step.

```yaml
steps:
  - name: Run integration tests

    # Maximum step execution time
    timeout-minutes: 10

    run: dotnet test
```

## 6.13 `continue-on-error`

Allows later steps to execute when the configured step fails.

```yaml
steps:
  - name: Experimental security scan

    # Continue even when this step fails
    continue-on-error: true

    run: echo "Run optional security scan"

  - name: Continue workflow
    run: echo "Next step executed"
```

## 6.14 Complete step example

```yaml
steps:

  # Step 1: Checkout
  - name: Checkout repository
    id: checkout
    uses: actions/checkout@v4

  # Step 2: Setup .NET
  - name: Setup .NET
    id: setup_dotnet
    uses: actions/setup-dotnet@v4

    # Action inputs
    with:
      dotnet-version: "8.0.x"

  # Step 3: Restore dependencies
  - name: Restore dependencies
    id: restore
    run: dotnet restore

  # Step 4: Build application
  - name: Build application
    id: build
    shell: bash
    working-directory: .
    timeout-minutes: 10
    run: dotnet build --configuration Release

  # Step 5: Test application
  - name: Run tests
    id: test
    if: success()
    continue-on-error: false
    run: dotnet test --configuration Release
```

---

# 🔍 7. uses vs run vs with

### 🎯 What you will learn

- Understand the difference between `uses`, `run`, and `with`.
- Understand where actions come from.
- Learn how action inputs are declared and discovered.
- Understand local actions, repository actions, and reusable workflows.
- Learn when to use each configuration style.

## 7.1 Difference between `uses`, `run`, and `with`

| Keyword | Purpose | Example |
|---|---|---|
| `uses` | Executes a reusable automation action instead of manually implementing its functionality. | `uses: actions/checkout@v4` |
| `run` | Executes shell commands directly on the configured GitHub Actions runner. | `run: dotnet build` |
| `with` | Supplies named input parameters required by an action or reusable workflow. | `with: fetch-depth: 0` |

## 7.2 What is an Action?

An action is a reusable automation component that performs a particular task inside a workflow.

Examples include:

- Checking out repository code.
- Installing programming language SDKs.
- Building Docker images.
- Uploading workflow artifacts.
- Downloading workflow artifacts.
- Deploying applications.
- Configuring cloud credentials.

## 7.3 Where do Actions come from?

| Source | Example | Explanation |
|---|---|---|
| GitHub Marketplace | `actions/checkout@v4` | Reuses an action published and distributed through GitHub's action ecosystem. |
| Public repository | `owner/repository@v1` | References an action maintained inside another GitHub repository. |
| Local repository | `./.github/actions/build` | References an action stored inside the current repository. |
| Docker image | `docker://alpine:3.20` | Executes a supported Docker-based action using the specified container image. |

## 7.4 Repository action example

```yaml
steps:

  # Action from GitHub repository
  - name: Checkout code
    uses: actions/checkout@v4

  # Action from another repository
  - name: Upload artifact
    uses: actions/upload-artifact@v4
```

## 7.5 Local action example

```yaml
steps:

  # Reference a local action inside the current repository
  - name: Run custom build action
    uses: ./.github/actions/custom-build
```

Example folder structure:

```text
Repository/
│
├── .github/
│   │
│   ├── workflows/
│   │   └── ci.yml
│   │
│   └── actions/
│       │
│       └── custom-build/
│           └── action.yml
```

## 7.6 Docker action example

```yaml
steps:

  # Execute a Docker-based action
  - name: Run Alpine container
    uses: docker://alpine:3.20
```

## 7.7 How does `with` work?

The `with` keyword provides input values accepted by the selected action.

For example, `actions/checkout` accepts inputs such as `repository`, `ref`, `token`, `path`, and `fetch-depth`.

```yaml
steps:
  - name: Checkout repository

    # Action reference
    uses: actions/checkout@v4

    # Action input values
    with:

      # Input: fetch-depth
      fetch-depth: 0

      # Input: clean
      clean: true

      # Input: path
      path: source-code
```

## 7.8 Where can we find `with` parameters?

### Method 1: GitHub Marketplace

1. Open GitHub Marketplace.
2. Search for the required action.
3. Open its documentation.
4. Find the usage examples.
5. Check the available input parameters.

### Method 2: Action repository

Open the action repository and inspect its `action.yml` or `action.yaml` metadata file.

Example:

```yaml
# Example action.yml metadata file

# Action name
name: Custom Build Action

# Action description
description: Builds the application

# Action input definitions
inputs:

  # Input identifier
  configuration:

    # Input description
    description: "Build configuration"

    # Required setting
    required: false

    # Default value
    default: Release

  # Another input
  project-path:
    description: "Project directory"
    required: true

# Action execution configuration
runs:
  using: composite

  # Composite action steps
  steps:
    - name: Build
      shell: bash
      run: dotnet build "${{ inputs.project-path }}" --configuration "${{ inputs.configuration }}"
```

## 7.9 `with` for reusable workflows

Reusable workflows declare inputs under `on.workflow_call.inputs`.

### Reusable workflow

```yaml
# File: .github/workflows/build-reusable.yml

name: Reusable Build

on:
  workflow_call:

    # Reusable workflow inputs
    inputs:
      configuration:
        description: "Build configuration"
        required: false
        type: string
        default: Release

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Build
        run: echo "${{ inputs.configuration }}"
```

### Calling workflow

```yaml
jobs:
  build:

    # Invoke reusable workflow
    uses: ./.github/workflows/build-reusable.yml

    # Pass reusable workflow inputs
    with:
      configuration: Release
```

## 7.10 Action versus reusable workflow

| Feature | Action | Reusable workflow |
|---|---|---|
| Location | Referenced inside a job's steps. | Referenced directly under a job identifier. |
| Invocation | `steps[].uses` | `jobs.<job_id>.uses` |
| Inputs | Declared in action metadata. | Declared under `on.workflow_call.inputs`. |
| Secrets | Passed through action inputs or supported contexts. | Passed through `secrets` or `secrets: inherit`. |
| Execution | Performs a reusable task inside the current job. | Executes a separately defined workflow with its own jobs. |
| Example | `actions/checkout@v4` | `./.github/workflows/build.yml` |

## 7.11 Important syntax difference

```yaml
jobs:

  # Normal job using actions
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

        # Action inputs
        with:
          fetch-depth: 0

  # Reusable workflow invocation
  reusable-build:
    uses: ./.github/workflows/reusable-build.yml

    # Reusable workflow inputs
    with:
      configuration: Release
```

---

# 🪜 8. Nested Structures

### 🎯 What you will learn

- Understand how nested YAML properties work.
- Learn multilevel configuration structures beyond one level.
- Explore `strategy`, `matrix`, `container`, `services`, and `defaults`.
- Understand where each nested property belongs.
- See the complete hierarchy with commented examples.

## 8.1 Nested YAML structure

A nested property belongs to the parent keyword whose indentation contains it.

### Example

```yaml
jobs:

  # Level 1: Job identifier
  build:

    # Level 2: Job configuration
    strategy:

      # Level 3: Strategy configuration
      matrix:

        # Level 4: Matrix variable
        os:

          # Level 5: Matrix value
          - ubuntu-latest
          - windows-latest
```

## 8.2 `strategy` hierarchy

| Level | Keyword | Definition |
|---|---|---|
| 1 | `strategy` | Configures how GitHub schedules multiple executions of the same job. |
| 2 | `matrix` | Defines combinations of variables used to create multiple job executions. |
| 3 | `include` | Adds additional combinations or properties to the configured matrix. |
| 3 | `exclude` | Removes selected combinations from the generated matrix executions. |
| 2 | `fail-fast` | Determines whether GitHub cancels remaining matrix jobs after a failure. |
| 2 | `max-parallel` | Limits the number of matrix job executions running simultaneously. |

### Complete nested example

```yaml
jobs:
  test:
    runs-on: ubuntu-latest

    # Level 3: Strategy
    strategy:

      # Level 4: Matrix
      matrix:

        # Level 5: Operating systems
        os:
          - ubuntu-latest
          - windows-latest

        # Level 5: Runtime versions
        version:
          - "8.0"
          - "9.0"

        # Level 5: Include additional combination
        include:

          # Level 6: Additional combination
          - os: ubuntu-latest
            version: "10.0"
            experimental: true

        # Level 5: Exclude combination
        exclude:

          # Level 6: Combination to exclude
          - os: windows-latest
            version: "9.0"

      # Level 4: Fail-fast configuration
      fail-fast: false

      # Level 4: Maximum parallel jobs
      max-parallel: 2

    steps:
      - name: Display configuration
        run: |
          echo "${{ matrix.os }}"
          echo "${{ matrix.version }}"
```

## 8.3 `container` hierarchy

| Level | Keyword | Definition |
|---|---|---|
| 1 | `container` | Runs the complete job inside a specified container image. |
| 2 | `image` | Identifies the Docker image used to create the job container. |
| 2 | `credentials` | Supplies registry authentication credentials required to download private container images. |
| 3 | `username` | Identifies the username used for authenticating against the container image registry. |
| 3 | `password` | Supplies the credential used to authenticate against the container image registry. |
| 2 | `env` | Defines environment variables available inside the configured job container. |
| 2 | `ports` | Publishes container ports for communication with other job processes. |
| 2 | `volumes` | Mounts directories or named volumes into the running job container. |
| 2 | `options` | Passes additional supported Docker container creation options to the runner. |

### Example

```yaml
jobs:
  build:
    runs-on: ubuntu-latest

    # Level 2: Job container
    container:

      # Level 3: Image
      image: node:22

      # Level 3: Registry credentials
      credentials:

        # Level 4
        username: ${{ github.actor }}

        # Level 4
        password: ${{ secrets.GITHUB_TOKEN }}

      # Level 3: Environment variables
      env:

        # Level 4
        NODE_ENV: production

      # Level 3: Ports
      ports:

        # Level 4
        - 3000

      # Level 3: Volumes
      volumes:

        # Level 4
        - my-volume:/data

      # Level 3: Docker options
      options: --cpus 2

    steps:
      - name: Display Node version
        run: node --version
```

## 8.4 `services` hierarchy

| Level | Keyword | Definition |
|---|---|---|
| 1 | `services` | Defines supporting service containers required by a configured workflow job. |
| 2 | Service identifier | Assigns a unique name to an individual supporting service container. |
| 3 | `image` | Identifies the Docker image used to start the supporting service container. |
| 3 | `credentials` | Supplies registry authentication credentials for private supporting service images. |
| 4 | `username` | Identifies the registry username used for private service image authentication. |
| 4 | `password` | Supplies the registry credential required for private service image authentication. |
| 3 | `env` | Defines environment variables required by the supporting service container. |
| 3 | `ports` | Maps service container ports to accessible runner network ports. |
| 3 | `volumes` | Mounts storage locations into the supporting service container during execution. |
| 3 | `options` | Supplies additional Docker container creation options for the supporting service. |

### Example

```yaml
jobs:
  test:
    runs-on: ubuntu-latest

    # Level 2: Supporting services
    services:

      # Level 3: Service identifier
      mysql:

        # Level 4: Container image
        image: mysql:8

        # Level 4: Container environment
        env:
          MYSQL_ROOT_PASSWORD: example
          MYSQL_DATABASE: testdb

        # Level 4: Port configuration
        ports:
          - 3306:3306

        # Level 4: Health-check options
        options: >-
          --health-cmd="mysqladmin ping"
          --health-interval=10s
          --health-timeout=5s
          --health-retries=5

    steps:
      - name: Run database tests
        run: echo "Run database integration tests"
```

## 8.5 `defaults.run` hierarchy

| Level | Keyword | Definition |
|---|---|---|
| 1 | `defaults` | Groups default execution settings for applicable workflow steps. |
| 2 | `run` | Contains default properties used by shell-based workflow commands. |
| 3 | `shell` | Specifies the default command interpreter for applicable run steps. |
| 3 | `working-directory` | Specifies the default execution directory for applicable run steps. |

### Example

```yaml
jobs:
  build:
    runs-on: ubuntu-latest

    # Level 3: Defaults
    defaults:

      # Level 4: Run configuration
      run:

        # Level 5: Default shell
        shell: bash

        # Level 5: Default directory
        working-directory: ./src

    steps:
      - name: Build
        run: dotnet build
```

## 8.6 `environment` hierarchy

| Level | Keyword | Definition |
|---|---|---|
| 1 | `environment` | Associates a job with a configured repository deployment environment. |
| 2 | `name` | Identifies the repository environment used by the configured job. |
| 2 | `url` | Defines the URL associated with the deployment environment or deployment result. |

### Example

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest

    # Level 2
    environment:

      # Level 3
      name: production

      # Level 3
      url: https://example.com

    steps:
      - run: echo "Deploying"
```

## 8.7 `concurrency` hierarchy

| Level | Keyword | Definition |
|---|---|---|
| 1 | `concurrency` | Groups matching workflow or job executions to control simultaneous processing. |
| 2 | `group` | Defines the identifier used to group matching workflow or job executions. |
| 2 | `cancel-in-progress` | Determines whether an existing running execution should be cancelled when another matching execution starts. |

### Example

```yaml
concurrency:

  # Level 2
  group: ${{ github.workflow }}-${{ github.ref }}

  # Level 2
  cancel-in-progress: true
```

## 8.8 `permissions` hierarchy

```yaml
# Level 1: Workflow
permissions:

  # Level 2: Permission category
  contents: read

  # Level 2: Permission category
  packages: write

jobs:
  deploy:

    # Level 3: Job-level permission configuration
    permissions:

      # Level 4
      contents: read

      # Level 4
      id-token: write

    runs-on: ubuntu-latest

    steps:
      - run: echo "Deploy"
```

---

# 🧠 9. Conditions and Expressions

### 🎯 What you will learn

- Understand the expression syntax `${{ }}`.
- Learn context-based dynamic values.
- Understand conditions and status-check functions.
- Learn expression operators and built-in functions.
- Apply expressions inside jobs, steps, environment variables, and action inputs.

Expressions allow workflows to evaluate dynamic values and make execution decisions.

## 9.1 Expression syntax

The standard GitHub Actions expression format uses `${{ }}` around an expression.

```yaml
# Read GitHub actor
${{ github.actor }}

# Read branch name
${{ github.ref_name }}

# Read environment variable
${{ env.APP_NAME }}
```

## 9.2 Expression locations

| Location | Example |
|---|---|
| `run-name` | `run-name: Build ${{ github.ref_name }}` |
| `if` | `if: github.ref == 'refs/heads/main'` |
| `env` | `APP_NAME: ${{ vars.APP_NAME }}` |
| `with` | `with: version: ${{ env.VERSION }}` |
| `name` | `name: Build ${{ matrix.version }}` |
| `environment` | `name: ${{ inputs.environment }}` |
| `outputs` | `version: ${{ steps.version.outputs.value }}` |
| `concurrency` | `group: ${{ github.workflow }}-${{ github.ref }}` |

## 9.3 `if`

Evaluates an expression to determine whether the configured job or step should execute.

### Job-level condition

```yaml
jobs:
  deploy:

    # Job-level condition
    if: github.ref == 'refs/heads/main'

    runs-on: ubuntu-latest

    steps:
      - run: echo "Deploy"
```

### Step-level condition

```yaml
steps:
  - name: Deploy
    if: github.ref == 'refs/heads/main'
    run: echo "Deploying main"
```

### Combined condition

```yaml
steps:
  - name: Deploy
    if: github.ref == 'refs/heads/main' && success()
    run: echo "Deploying"
```

## 9.4 Status-check functions

| Function | Definition |
|---|---|
| `success()` | Returns true when all previous required steps or dependencies have completed successfully. |
| `failure()` | Returns true when a previous required step or dependency has failed. |
| `always()` | Evaluates true regardless of whether preceding steps or jobs succeeded or failed. |
| `cancelled()` | Returns true when the current workflow or job execution has been cancelled. |

### `success()`

```yaml
steps:
  - name: Build
    run: dotnet build

  - name: Run after success
    if: success()
    run: echo "Build succeeded"
```

### `failure()`

```yaml
steps:
  - name: Build
    run: dotnet build

  - name: Run after failure
    if: failure()
    run: echo "Build failed"
```

### `always()`

```yaml
steps:
  - name: Cleanup
    if: always()
    run: echo "Cleanup executed"
```

### `cancelled()`

```yaml
steps:
  - name: Cancellation handler
    if: cancelled()
    run: echo "Workflow was cancelled"
```

## 9.5 Operators

| Operator | Meaning | Example |
|---|---|---|
| `==` | Compares whether two expression values are equal to each other. | `github.ref == 'refs/heads/main'` |
| `!=` | Checks whether two expression values are different from each other. | `github.ref != 'refs/heads/main'` |
| `&&` | Evaluates true when both specified expression conditions evaluate as true. | `success() && github.ref == 'refs/heads/main'` |
| `||` | Evaluates true when either specified expression condition evaluates as true. | `failure() || cancelled()` |
| `!` | Negates the Boolean result of the expression immediately following it. | `!cancelled()` |
| `>` | Checks whether the left expression value is greater than the right. | `steps.check.outputs.count > 5` |
| `<` | Checks whether the left expression value is smaller than the right. | `steps.check.outputs.count < 5` |
| `>=` | Checks whether the left expression value is greater than or equal to the right. | `steps.check.outputs.count >= 5` |
| `<=` | Checks whether the left expression value is smaller than or equal to the right. | `steps.check.outputs.count <= 5` |
| `()` | Groups expression operations or supplies arguments to expression functions. | `contains(github.ref, 'main')` |
| `.` | Accesses a property belonging to a supported context or object. | `github.actor` |
| `[]` | Accesses a context property using bracket notation and a literal key. | `github['sha']` |
| `*` | Selects properties from supported object filters and arrays. | `github.event.issue.labels.*.name` |

## 9.6 Expression functions

| Function | Definition | Example |
|---|---|---|
| `contains()` | Checks whether a string or array contains the specified search value. | `contains(github.ref, 'main')` |
| `startsWith()` | Checks whether a string begins with the specified starting text. | `startsWith(github.ref, 'refs/heads/')` |
| `endsWith()` | Checks whether a string finishes with the specified ending text. | `endsWith(github.ref, '/main')` |
| `format()` | Creates a formatted string by replacing numbered placeholders with supplied values. | `format('Hello {0}', github.actor)` |
| `join()` | Combines array elements into a single string using a specified separator. | `join(github.event.issue.labels.*.name, ', ')` |
| `toJSON()` | Converts a supported context or expression value into a JSON string. | `toJSON(github.event)` |
| `fromJSON()` | Parses a JSON string into an expression value for further evaluation. | `fromJSON('["a","b"]')` |
| `hashFiles()` | Calculates a hash using the contents of matching repository files. | `hashFiles('**/*.csproj')` |

### `contains()`

```yaml
steps:
  - name: Check branch
    if: contains(github.ref, 'main')
    run: echo "Main branch detected"
```

### `startsWith()`

```yaml
steps:
  - name: Check feature branch
    if: startsWith(github.ref, 'refs/heads/feature/')
    run: echo "Feature branch detected"
```

### `endsWith()`

```yaml
steps:
  - name: Check tag
    if: endsWith(github.ref, 'v1.0.0')
    run: echo "Release tag detected"
```

### `format()`

```yaml
steps:
  - name: Format message
    run: echo "${{ format('Hello {0}', github.actor) }}"
```

### `join()`

```yaml
steps:
  - name: Join values
    run: echo "${{ join(github.event.issue.labels.*.name, ', ') }}"
```

### `toJSON()`

```yaml
steps:
  - name: Display event JSON
    run: echo '${{ toJSON(github.event) }}'
```

Avoid printing entire event payloads when they might contain sensitive or untrusted data.

### `fromJSON()`

```yaml
jobs:
  test:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        version: ${{ fromJSON('["8.0","9.0"]') }}

    steps:
      - run: echo "${{ matrix.version }}"
```

### `hashFiles()`

```yaml
steps:
  - name: Generate dependency hash
    run: echo "Dependency files checked"

  - name: Display project file hash
    run: echo "${{ hashFiles('**/*.csproj') }}"
```

## 9.7 Expression comparison examples

### Branch comparison

```yaml
if: github.ref == 'refs/heads/main'
```

### Event comparison

```yaml
if: github.event_name == 'push'
```

### Multiple conditions

```yaml
if: github.event_name == 'push' && github.ref == 'refs/heads/main'
```

### Excluding a branch

```yaml
if: github.ref != 'refs/heads/develop'
```

### Success and branch condition

```yaml
if: success() && github.ref == 'refs/heads/main'
```

## 9.8 Important expression rules

- Most expression values require `${{ }}` delimiters.
- The `if` property supports expression syntax without those delimiters.
- GitHub expressions are evaluated by GitHub before runner-side shell execution.
- Shell variables are evaluated later by the selected command interpreter.
- Expression property names can use dot notation or supported bracket notation.
- Untrusted event data must never be inserted into shell commands without careful handling.

---

# 🌍 10. Variables and Contexts

### 🎯 What you will learn

- Understand all important GitHub Actions contexts.
- Learn the difference between `env`, `vars`, and `secrets`.
- Understand `${{ }}` expression syntax versus shell variable syntax.
- Learn predefined GitHub runner environment variables.
- Understand how variables move between workflow, job, and step scopes.

## 10.1 What is a Context?

A context is a collection of information that GitHub makes available during workflow expression evaluation.

### Common contexts

| Context | Definition |
|---|---|
| `github` | Provides information about the current repository, event, commit, user, and workflow execution. |
| `env` | Provides access to environment variables defined in workflow, job, or step configuration. |
| `vars` | Provides access to repository, organization, or environment-level configuration variables. |
| `secrets` | Provides access to sensitive values stored through GitHub Actions secrets configuration. |
| `job` | Provides information about the currently executing job and its execution status. |
| `jobs` | Provides reusable-workflow job outputs in supported reusable workflow contexts. |
| `steps` | Provides outputs and execution information for previously executed workflow steps. |
| `runner` | Provides information about the machine currently executing the workflow job. |
| `strategy` | Provides information about the job execution strategy and current matrix execution index. |
| `matrix` | Provides values belonging to the current matrix-generated job combination. |
| `needs` | Provides results and outputs from jobs declared as dependencies. |
| `inputs` | Provides input values supplied to manual or reusable workflow execution. |

## 10.2 `github` context

Provides information about the repository and the current workflow execution event.

### Common properties

| Property | Definition |
|---|---|
| `github.actor` | Identifies the GitHub username responsible for initiating the workflow execution. |
| `github.repository` | Contains the repository owner and repository name associated with execution. |
| `github.repository_owner` | Contains the owner account associated with the current GitHub repository. |
| `github.sha` | Contains the commit SHA associated with the triggering workflow event. |
| `github.ref` | Contains the fully qualified Git reference associated with workflow execution. |
| `github.ref_name` | Contains the short branch or tag name associated with execution. |
| `github.ref_type` | Identifies whether the current Git reference represents a branch or tag. |
| `github.event_name` | Identifies the event that triggered the current workflow execution. |
| `github.workflow` | Contains the configured workflow name or workflow filename. |
| `github.run_id` | Contains the unique identifier assigned to the current workflow run. |
| `github.run_number` | Contains the sequential run number assigned to the workflow. |
| `github.job` | Contains the identifier of the currently executing workflow job. |
| `github.token` | Provides the GitHub token available through the current workflow context. |

### Example

```yaml
steps:
  - name: Display GitHub information
    run: |
      echo "Actor: ${{ github.actor }}"
      echo "Repository: ${{ github.repository }}"
      echo "Commit: ${{ github.sha }}"
      echo "Branch: ${{ github.ref_name }}"
      echo "Event: ${{ github.event_name }}"
      echo "Workflow: ${{ github.workflow }}"
```

## 10.3 `env` context

Provides access to environment variables configured in workflow, job, or step scopes.

```yaml
# Workflow-level env
env:
  APP_NAME: DemoApp

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Read env context
        run: echo "${{ env.APP_NAME }}"
```

## 10.4 `vars` context

Provides access to configuration variables created in repository, organization, or environment settings.

### Example

```yaml
steps:
  - name: Read repository variable
    run: echo "${{ vars.APP_ENVIRONMENT }}"
```

### Example using a variable in `env`

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest

    env:
      APP_ENVIRONMENT: ${{ vars.APP_ENVIRONMENT }}

    steps:
      - name: Deploy
        run: echo "$APP_ENVIRONMENT"
```

## 10.5 `secrets` context

Provides access to protected configuration values such as tokens and deployment credentials.

### Example

```yaml
steps:
  - name: Use deployment token

    env:
      DEPLOY_TOKEN: ${{ secrets.DEPLOY_TOKEN }}

    run: |
      ./deploy.sh
```

### Common secret names

| Secret name | Purpose |
|---|---|
| `AWS_ACCESS_KEY_ID` | Stores an AWS access key identifier when static credentials are required. |
| `AWS_SECRET_ACCESS_KEY` | Stores the corresponding AWS secret access key when static credentials are required. |
| `SSH_PRIVATE_KEY` | Stores the private SSH key used for authenticated server connections. |
| `DEPLOY_TOKEN` | Stores an application-specific deployment token used by automation scripts. |
| `DATABASE_PASSWORD` | Stores the password required for an application's database connection. |

**Security note:** Prefer OpenID Connect for supported cloud deployments instead of long-lived cloud access keys.

## 10.6 `job` context

Provides information about the currently executing workflow job.

```yaml
steps:
  - name: Display job status
    run: echo "${{ job.status }}"
```

Common property:

| Property | Definition |
|---|---|
| `job.status` | Identifies the current execution status of the configured workflow job. |

## 10.7 `jobs` context

Provides outputs from jobs in supported reusable workflow contexts.

```yaml
# Reusable workflow context
on:
  workflow_call:

jobs:
  build:
    runs-on: ubuntu-latest

    outputs:
      version: ${{ steps.version.outputs.value }}

    steps:
      - id: version
        run: echo "value=1.0.0" >> "$GITHUB_OUTPUT"
```

## 10.8 `steps` context

Provides information and outputs from previously executed steps.

```yaml
steps:
  - name: Generate value
    id: version
    run: echo "value=1.0.0" >> "$GITHUB_OUTPUT"

  - name: Read value
    run: echo "${{ steps.version.outputs.value }}"
```

## 10.9 `runner` context

Provides information about the machine executing the current workflow job.

| Property | Definition |
|---|---|
| `runner.os` | Identifies the operating system running the current GitHub Actions job. |
| `runner.arch` | Identifies the processor architecture of the current runner machine. |
| `runner.name` | Contains the name assigned to the current runner machine. |
| `runner.temp` | Contains the temporary directory path available on the current runner. |
| `runner.tool_cache` | Contains the runner directory used for caching installed development tools. |

### Example

```yaml
steps:
  - name: Display runner information
    run: |
      echo "OS: ${{ runner.os }}"
      echo "Architecture: ${{ runner.arch }}"
      echo "Runner name: ${{ runner.name }}"
```

## 10.10 `strategy` context

Provides information about the strategy used to execute the current job.

| Property | Definition |
|---|---|
| `strategy.job-index` | Identifies the zero-based position of the current job in matrix execution. |
| `strategy.job-total` | Contains the total number of jobs generated by the configured matrix strategy. |
| `strategy.fail-fast` | Identifies whether the current matrix strategy uses fail-fast behavior. |
| `strategy.max-parallel` | Contains the configured maximum number of parallel matrix job executions. |

### Example

```yaml
jobs:
  test:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        version:
          - "8.0"
          - "9.0"

    steps:
      - name: Display strategy information
        run: |
          echo "Job index: ${{ strategy.job-index }}"
          echo "Total jobs: ${{ strategy.job-total }}"
```

## 10.11 `matrix` context

Provides the current matrix combination values for a generated job execution.

```yaml
jobs:
  test:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        version:
          - "8.0"
          - "9.0"

    steps:
      - name: Display matrix version
        run: echo "${{ matrix.version }}"
```

## 10.12 `needs` context

Provides results and outputs from prerequisite jobs.

```yaml
jobs:
  build:
    runs-on: ubuntu-latest

    outputs:
      version: ${{ steps.info.outputs.version }}

    steps:
      - id: info
        run: echo "version=1.0.0" >> "$GITHUB_OUTPUT"

  deploy:
    needs: build
    runs-on: ubuntu-latest

    steps:
      - name: Read prerequisite output
        run: echo "${{ needs.build.outputs.version }}"
```

## 10.13 `inputs` context

Provides values supplied through manual or reusable workflow invocation.

```yaml
on:
  workflow_dispatch:
    inputs:
      environment:
        description: "Deployment environment"
        required: true
        type: string

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Display input
        run: echo "${{ inputs.environment }}"
```

## 10.14 Expression syntax versus shell syntax

This is one of the most important concepts in GitHub Actions.

GitHub expressions and shell variables are different systems.

### Syntax comparison

| Syntax | Where it works | Example |
|---|---|---|
| `${{ env.APP_NAME }}` | GitHub expression evaluation | `echo "${{ env.APP_NAME }}"` |
| `$APP_NAME` | Bash and compatible shell variable access | `echo "$APP_NAME"` |
| `${APP_NAME}` | Bash variable expansion | `echo "${APP_NAME}"` |
| `$env:APP_NAME` | PowerShell environment variable access | `Write-Output $env:APP_NAME` |
| `%APP_NAME%` | Windows CMD environment variable access | `echo %APP_NAME%` |
| `$GITHUB_SHA` | Bash access to predefined runner variables | `echo "$GITHUB_SHA"` |
| `${{ github.sha }}` | GitHub expression access to commit information | `echo "${{ github.sha }}"` |

### Example 1: Bash variable

```yaml
steps:
  - name: Bash variable
    run: |
      APP_NAME="DemoApp"

      # Bash variable syntax
      echo "$APP_NAME"

      # Alternative Bash syntax
      echo "${APP_NAME}"
```

### Example 2: GitHub expression

```yaml
steps:
  - name: GitHub expression
    run: |
      # GitHub expression
      echo "${{ github.actor }}"

      # GitHub expression
      echo "${{ github.repository }}"
```

### Example 3: PowerShell variables

```yaml
steps:
  - name: PowerShell variable
    shell: pwsh

    env:
      APP_NAME: DemoApp

    run: |
      # PowerShell environment variable syntax
      Write-Output $env:APP_NAME
```

### Example 4: CMD variables

```yaml
steps:
  - name: Windows CMD variable
    shell: cmd

    env:
      APP_NAME: DemoApp

    run: |
      REM CMD environment variable syntax
      echo %APP_NAME%
```

## 10.15 Predefined environment variables

GitHub automatically provides several environment variables to the runner.

| Variable | Definition |
|---|---|
| `GITHUB_ACTION` | Contains the identifier associated with the currently executing action. |
| `GITHUB_ACTION_PATH` | Contains the path of the currently executing action when supported. |
| `GITHUB_ACTION_REPOSITORY` | Identifies the repository containing the currently executing action. |
| `GITHUB_ACTOR` | Contains the GitHub username responsible for initiating the workflow execution. |
| `GITHUB_ACTOR_ID` | Contains the numeric account identifier associated with the workflow actor. |
| `GITHUB_API_URL` | Contains the base URL of the GitHub REST API endpoint. |
| `GITHUB_BASE_REF` | Contains the target branch name associated with a pull request event. |
| `GITHUB_ENV` | Contains the path of the environment file used to set later-step variables. |
| `GITHUB_EVENT_NAME` | Contains the name of the event that triggered workflow execution. |
| `GITHUB_EVENT_PATH` | Contains the path to the JSON file containing the event payload. |
| `GITHUB_GRAPHQL_URL` | Contains the URL of the GitHub GraphQL API endpoint. |
| `GITHUB_HEAD_REF` | Contains the source branch name associated with a pull request event. |
| `GITHUB_JOB` | Contains the identifier of the currently executing workflow job. |
| `GITHUB_OUTPUT` | Contains the path of the current step's output file. |
| `GITHUB_PATH` | Contains the path of the file used to modify PATH for later steps. |
| `GITHUB_REF` | Contains the fully qualified Git reference associated with the workflow execution. |
| `GITHUB_REF_NAME` | Contains the short branch or tag name associated with the execution. |
| `GITHUB_REF_TYPE` | Identifies whether the Git reference represents a branch or tag. |
| `GITHUB_REPOSITORY` | Contains the repository owner and repository name associated with execution. |
| `GITHUB_REPOSITORY_OWNER` | Contains the owner account associated with the current GitHub repository. |
| `GITHUB_RUN_ATTEMPT` | Contains the current attempt number for the workflow run. |
| `GITHUB_RUN_ID` | Contains the unique identifier assigned to the current workflow run. |
| `GITHUB_RUN_NUMBER` | Contains the sequential run number assigned to the workflow. |
| `GITHUB_SERVER_URL` | Contains the base URL of the GitHub server hosting the repository. |
| `GITHUB_SHA` | Contains the commit SHA associated with the triggering workflow event. |
| `GITHUB_STEP_SUMMARY` | Contains the path to the Markdown summary file for the current step. |
| `GITHUB_WORKFLOW` | Contains the name of the currently executing workflow. |
| `GITHUB_WORKFLOW_REF` | Contains the reference identifying the current workflow file and repository. |
| `GITHUB_WORKFLOW_SHA` | Contains the commit SHA associated with the workflow file version. |
| `GITHUB_WORKSPACE` | Contains the absolute path of the checked-out repository workspace on the runner. |
| `RUNNER_ARCH` | Contains the architecture of the current GitHub Actions runner machine. |
| `RUNNER_DEBUG` | Indicates whether runner diagnostic logging is enabled for the workflow execution. |
| `RUNNER_NAME` | Contains the name assigned to the current GitHub Actions runner machine. |
| `RUNNER_OS` | Contains the operating system name of the current GitHub Actions runner. |
| `RUNNER_TEMP` | Contains the temporary directory path available on the current runner. |
| `RUNNER_TOOL_CACHE` | Contains the directory used by the runner for caching installed development tools. |

### Example

```yaml
steps:
  - name: Display runner variables
    run: |
      echo "$GITHUB_SHA"
      echo "$GITHUB_REF"
      echo "$GITHUB_REPOSITORY"
      echo "$GITHUB_ACTOR"
      echo "$GITHUB_WORKSPACE"
      echo "$RUNNER_OS"
```

## 10.16 `env` versus `vars` versus `secrets`

| Feature | `env` | `vars` | `secrets` |
|---|---|---|---|
| Purpose | Defines workflow environment variables. | Stores reusable configuration values. | Stores sensitive credentials and values. |
| Defined in | Workflow YAML configuration. | GitHub repository, organization, or environment settings. | GitHub secrets settings. |
| Example | `env.APP_NAME` | `vars.APP_ENV` | `secrets.DEPLOY_TOKEN` |
| Sensitive storage | No | No | Yes |
| Scope | Workflow, job, or step | Repository, organization, or environment | Repository, organization, or environment |

### Combined example

```yaml
# Workflow environment
env:
  APP_NAME: DemoApp

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Configure deployment

        # Step-level environment variables
        env:

          # Workflow variable
          APP_NAME: ${{ env.APP_NAME }}

          # Repository configuration variable
          APP_ENVIRONMENT: ${{ vars.APP_ENVIRONMENT }}

          # Protected secret
          DEPLOY_TOKEN: ${{ secrets.DEPLOY_TOKEN }}

        run: |
          echo "$APP_NAME"
          echo "$APP_ENVIRONMENT"

          # Use DEPLOY_TOKEN without printing it
          ./deploy.sh
```

## 10.17 Accessing context properties

GitHub contexts support dot notation and bracket notation.

### Dot notation

```yaml
steps:
  - name: Dot notation
    run: echo "${{ github.sha }}"
```

### Bracket notation

```yaml
steps:
  - name: Bracket notation
    run: echo "${{ github['sha'] }}"
```

### Nested properties

```yaml
steps:
  - name: Nested context
    run: |
      echo "${{ github.event.repository.name }}"
      echo "${{ github.event.sender.login }}"
```

### Object property filters

```yaml
steps:
  - name: Read issue labels
    run: echo "${{ join(github.event.issue.labels.*.name, ', ') }}"
```

---

# 📤 11. Outputs and Environment Files

### 🎯 What you will learn

- Understand how one step passes values to another step.
- Learn how jobs publish outputs to dependent jobs.
- Understand the purpose of GitHub environment files.
- Learn `GITHUB_OUTPUT`, `GITHUB_ENV`, `GITHUB_PATH`, and `GITHUB_STEP_SUMMARY`.
- See examples of single-line and multiline values.

## 11.1 What is an Output?

An output is a value produced by one step or job that can be consumed by another workflow component.

## 11.2 `GITHUB_OUTPUT`

Stores output values generated by the current step for later expression access.

### Example 1: Single-line output

```yaml
steps:
  - name: Generate version

    # Step identifier
    id: version

    # Write a named output
    run: echo "value=1.0.0" >> "$GITHUB_OUTPUT"

  - name: Read output

    # Access previous step output
    run: echo "${{ steps.version.outputs.value }}"
```

### Example 2: Multiple outputs

```yaml
steps:
  - name: Generate application information
    id: app

    # Write multiple output values
    run: |
      echo "name=DemoApp" >> "$GITHUB_OUTPUT"
      echo "version=1.0.0" >> "$GITHUB_OUTPUT"
      echo "environment=production" >> "$GITHUB_OUTPUT"

  - name: Read outputs
    run: |
      echo "${{ steps.app.outputs.name }}"
      echo "${{ steps.app.outputs.version }}"
      echo "${{ steps.app.outputs.environment }}"
```

### Example 3: Multiline output

```yaml
steps:
  - name: Generate multiline output
    id: data

    run: |
      {
        echo "result<<EOF"
        echo "First line"
        echo "Second line"
        echo "Third line"
        echo "EOF"
      } >> "$GITHUB_OUTPUT"

  - name: Read multiline output
    run: echo "${{ steps.data.outputs.result }}"
```

## 11.3 `GITHUB_ENV`

Stores environment variables that subsequent steps can access during the same job.

### Example 1: Create a variable

```yaml
steps:
  - name: Create environment variable

    # Write variable into GITHUB_ENV
    run: echo "APP_VERSION=1.0.0" >> "$GITHUB_ENV"

  - name: Read environment variable

    # Available in subsequent step
    run: echo "$APP_VERSION"
```

### Example 2: Multiple variables

```yaml
steps:
  - name: Create variables
    run: |
      echo "APP_NAME=DemoApp" >> "$GITHUB_ENV"
      echo "APP_ENV=production" >> "$GITHUB_ENV"
      echo "APP_VERSION=1.0.0" >> "$GITHUB_ENV"

  - name: Read variables
    run: |
      echo "$APP_NAME"
      echo "$APP_ENV"
      echo "$APP_VERSION"
```

### Example 3: Multiline variable

```yaml
steps:
  - name: Create multiline environment variable
    run: |
      {
        echo "APP_CONFIG<<EOF"
        echo "First configuration"
        echo "Second configuration"
        echo "EOF"
      } >> "$GITHUB_ENV"

  - name: Read configuration
    run: echo "$APP_CONFIG"
```

**Important:** A step cannot access an environment variable that it has just written to `GITHUB_ENV`; the variable becomes available to subsequent steps.

## 11.4 `GITHUB_PATH`

Adds a directory to PATH for subsequent workflow steps.

```yaml
steps:
  - name: Add custom binary directory

    # Append directory to PATH file
    run: echo "$HOME/custom-bin" >> "$GITHUB_PATH"

  - name: Display PATH
    run: echo "$PATH"
```

## 11.5 `GITHUB_STEP_SUMMARY`

Adds Markdown content to the summary displayed for the current workflow run.

### Example 1: Basic summary

```yaml
steps:
  - name: Create workflow summary
    run: |
      echo "# Build Results" >> "$GITHUB_STEP_SUMMARY"
      echo "Build completed successfully." >> "$GITHUB_STEP_SUMMARY"
```

### Example 2: List summary

```yaml
steps:
  - name: Add result list
    run: |
      echo "## Application Results" >> "$GITHUB_STEP_SUMMARY"
      echo "- Build: Successful" >> "$GITHUB_STEP_SUMMARY"
      echo "- Tests: Passed" >> "$GITHUB_STEP_SUMMARY"
      echo "- Deployment: Pending" >> "$GITHUB_STEP_SUMMARY"
```

### Example 3: Markdown table

```yaml
steps:
  - name: Create result table
    run: |
      echo "## Pipeline Status" >> "$GITHUB_STEP_SUMMARY"
      echo "| Stage | Status |" >> "$GITHUB_STEP_SUMMARY"
      echo "|---|---|" >> "$GITHUB_STEP_SUMMARY"
      echo "| Build | Success |" >> "$GITHUB_STEP_SUMMARY"
      echo "| Test | Success |" >> "$GITHUB_STEP_SUMMARY"
```

## 11.6 Step outputs versus job outputs

| Feature | Step output | Job output |
|---|---|---|
| Defined using | `GITHUB_OUTPUT` | Job-level `outputs` mapping |
| Accessed using | `steps.<id>.outputs.<name>` | `needs.<job_id>.outputs.<name>` |
| Available to | Later steps in the same job | Dependent jobs |
| Requires | Step identifier | Job dependency and mapped output |

### Complete comparison example

```yaml
jobs:

  build:
    runs-on: ubuntu-latest

    # Job output mapping
    outputs:
      version: ${{ steps.info.outputs.version }}

    steps:

      # Step output
      - name: Generate version
        id: info
        run: echo "version=1.0.0" >> "$GITHUB_OUTPUT"

      # Read step output
      - name: Read local output
        run: echo "${{ steps.info.outputs.version }}"

  deploy:
    runs-on: ubuntu-latest

    # Depend on build job
    needs: build

    steps:

      # Read job output
      - name: Read remote job output
        run: echo "${{ needs.build.outputs.version }}"
```

## 11.7 PowerShell environment files

The syntax for writing to runner environment files differs between Bash and PowerShell.

### Bash

```yaml
steps:
  - name: Write output using Bash
    shell: bash
    run: echo "version=1.0.0" >> "$GITHUB_OUTPUT"
```

### PowerShell

```yaml
steps:
  - name: Write output using PowerShell
    shell: pwsh
    run: |
      "version=1.0.0" >> $env:GITHUB_OUTPUT
```

### PowerShell environment variable

```yaml
steps:
  - name: Write environment variable
    shell: pwsh
    run: |
      "APP_NAME=DemoApp" >> $env:GITHUB_ENV

  - name: Read variable
    shell: pwsh
    run: Write-Output $env:APP_NAME
```

---

# 📦 12. Strategy and Matrix

### 🎯 What you will learn

- Understand why matrix strategies are used.
- Learn how GitHub generates multiple jobs from one job definition.
- Understand `matrix`, `include`, and `exclude`.
- Learn `fail-fast` and `max-parallel`.
- Use matrix variables inside job steps and action inputs.

## 12.1 What is a Strategy?

A strategy defines how GitHub schedules multiple executions of the same job using configured execution rules.

## 12.2 What is a Matrix?

A matrix creates multiple job executions using combinations of specified variables.

### Example without matrix

```yaml
jobs:

  # First manually defined job
  test-8:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Testing .NET 8"

  # Second manually defined job
  test-9:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Testing .NET 9"
```

### Example with matrix

```yaml
jobs:
  test:
    runs-on: ubuntu-latest

    # Matrix configuration
    strategy:
      matrix:

        # Matrix variable
        version:
          - "8.0"
          - "9.0"

    steps:
      - name: Display version
        run: echo "${{ matrix.version }}"
```

GitHub generates separate job executions for the configured matrix values.

## 12.3 `strategy` keywords

| Keyword | Definition |
|---|---|
| `strategy` | Configures how GitHub schedules multiple executions of the same job. |
| `matrix` | Defines combinations of variables used to create multiple job executions. |
| `include` | Adds additional combinations or properties to the configured matrix. |
| `exclude` | Removes selected combinations from the generated matrix executions. |
| `fail-fast` | Determines whether GitHub cancels remaining matrix jobs after a failure. |
| `max-parallel` | Limits the number of matrix job executions running simultaneously. |

## 12.4 Multiple matrix variables

```yaml
jobs:
  test:
    runs-on: ubuntu-latest

    strategy:
      matrix:

        # First variable
        os:
          - ubuntu-latest
          - windows-latest

        # Second variable
        version:
          - "8.0"
          - "9.0"

    steps:
      - name: Display configuration
        run: |
          echo "OS: ${{ matrix.os }}"
          echo "Version: ${{ matrix.version }}"
```

Two operating systems multiplied by two versions generate four combinations.

## 12.5 `include`

Adds additional combinations or properties to the generated matrix.

```yaml
jobs:
  test:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        version:
          - "8.0"
          - "9.0"

        include:

          # Additional matrix combination
          - version: "10.0"
            experimental: true

    steps:
      - name: Display configuration
        run: |
          echo "Version: ${{ matrix.version }}"
          echo "Experimental: ${{ matrix.experimental }}"
```

## 12.6 `exclude`

Removes selected combinations from the generated matrix.

```yaml
jobs:
  test:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        os:
          - ubuntu-latest
          - windows-latest

        version:
          - "8.0"
          - "9.0"

        exclude:

          # Remove this combination
          - os: windows-latest
            version: "9.0"

    steps:
      - run: echo "${{ matrix.os }} - ${{ matrix.version }}"
```

## 12.7 `fail-fast`

Determines whether remaining matrix executions should be cancelled when one matrix execution fails.

```yaml
jobs:
  test:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        version:
          - "8.0"
          - "9.0"
          - "10.0"

      # Do not cancel remaining matrix jobs after failure
      fail-fast: false

    steps:
      - run: echo "Testing version ${{ matrix.version }}"
```

## 12.8 `max-parallel`

Limits the number of matrix-generated jobs executing simultaneously.

```yaml
jobs:
  test:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        version:
          - "8.0"
          - "9.0"
          - "10.0"
          - "11.0"

      # Execute at most two matrix jobs simultaneously
      max-parallel: 2

    steps:
      - run: echo "Testing ${{ matrix.version }}"
```

## 12.9 Matrix with different runner operating systems

```yaml
jobs:
  test:
    # Select runner using matrix value
    runs-on: ${{ matrix.os }}

    strategy:
      matrix:
        os:
          - ubuntu-latest
          - windows-latest
          - macos-latest

    steps:
      - name: Display runner
        run: echo "${{ matrix.os }}"
```

## 12.10 Matrix with setup action

```yaml
jobs:
  test:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        version:
          - "8.0.x"
          - "9.0.x"

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup .NET
        uses: actions/setup-dotnet@v4

        # Dynamic action input
        with:
          dotnet-version: ${{ matrix.version }}

      - name: Build
        run: dotnet build
```

## 12.11 Complete matrix example

```yaml
name: Multi-Version Testing

on:
  push:
    branches:
      - main

jobs:
  test:
    runs-on: ${{ matrix.os }}

    strategy:

      # Matrix configuration
      matrix:

        # Operating systems
        os:
          - ubuntu-latest
          - windows-latest

        # .NET versions
        version:
          - "8.0.x"
          - "9.0.x"

        # Additional combination
        include:
          - os: ubuntu-latest
            version: "10.0.x"
            experimental: true

        # Excluded combination
        exclude:
          - os: windows-latest
            version: "9.0.x"

      # Strategy configuration
      fail-fast: false
      max-parallel: 2

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup .NET
        uses: actions/setup-dotnet@v4
        with:
          dotnet-version: ${{ matrix.version }}

      - name: Display matrix configuration
        run: |
          echo "OS: ${{ matrix.os }}"
          echo "Version: ${{ matrix.version }}"

      - name: Build application
        run: dotnet build
```

---

# 🗂️ 13. Artifacts and Caching

### 🎯 What you will learn

- Understand the difference between artifacts and caches.
- Learn how to upload workflow-generated files.
- Learn how to download artifacts in another job.
- Understand caching and cache keys.
- Explore common artifact and cache action inputs.

## 13.1 What is an Artifact?

An artifact is a file or collection of files generated during workflow execution and preserved for later downloading or sharing.

### Common use cases

- Published application files.
- Test reports.
- Build packages.
- Deployment bundles.
- Diagnostic logs.

## 13.2 Upload artifact

Uses `actions/upload-artifact` to preserve files generated by a workflow step.

```yaml
steps:

  # Generate application files
  - name: Publish application
    run: dotnet publish -c Release -o ./publish

  # Upload generated files
  - name: Upload artifact
    uses: actions/upload-artifact@v4

    # Artifact action inputs
    with:

      # Artifact name
      name: application-build

      # Files or directory to upload
      path: ./publish/

      # Retention period in days
      retention-days: 7

      # Whether to fail when no matching files exist
      if-no-files-found: error
```

## 13.3 Artifact upload inputs

| Input | Definition |
|---|---|
| `name` | Defines the name used to identify the uploaded workflow artifact. |
| `path` | Specifies which files or directories should be included in the artifact. |
| `if-no-files-found` | Determines the action's behavior when no files match the specified paths. |
| `retention-days` | Defines how many days GitHub should retain the uploaded artifact. |
| `compression-level` | Configures the compression level used while creating the uploaded artifact. |
| `overwrite` | Determines whether an existing artifact with the same name should be replaced. |
| `include-hidden-files` | Determines whether hidden files should be included in the uploaded artifact. |

### Multiple paths

```yaml
steps:
  - name: Upload multiple directories
    uses: actions/upload-artifact@v4

    with:
      name: application-files
      path: |
        ./publish/
        ./reports/
        ./logs/
```

### Wildcard paths

```yaml
steps:
  - name: Upload reports
    uses: actions/upload-artifact@v4

    with:
      name: test-reports
      path: "**/*.trx"
```

## 13.4 Download artifact

Uses `actions/download-artifact` to retrieve previously uploaded workflow artifacts.

```yaml
steps:
  - name: Download application artifact
    uses: actions/download-artifact@v4

    # Download action inputs
    with:

      # Existing artifact name
      name: application-build

      # Destination directory
      path: ./downloaded/
```

## 13.5 Download artifacts between jobs

```yaml
jobs:

  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Publish
        run: dotnet publish -c Release -o ./publish

      - name: Upload
        uses: actions/upload-artifact@v4
        with:
          name: application-build
          path: ./publish/

  deploy:
    runs-on: ubuntu-latest

    # Wait for build job
    needs: build

    steps:
      - name: Download build artifact
        uses: actions/download-artifact@v4
        with:
          name: application-build
          path: ./publish/

      - name: Display downloaded files
        run: ls -la ./publish/
```

## 13.6 What is a Cache?

A cache stores reusable dependencies or files so later workflow executions can avoid downloading or generating them again.

### Common caching use cases

| Cached item | Purpose |
|---|---|
| NuGet packages | Avoid downloading unchanged .NET dependencies repeatedly. |
| npm packages | Reuse JavaScript package data across compatible workflow executions. |
| pip packages | Reduce repeated installation of Python dependencies during builds. |
| Maven dependencies | Reuse Java libraries downloaded during earlier Maven build executions. |
| Gradle dependencies | Reduce repeated dependency downloads during Gradle build executions. |

## 13.7 `actions/cache`

Uses the official cache action to save and restore matching files or directories.

### Important inputs

| Input | Definition |
|---|---|
| `path` | Specifies which files or directories GitHub should save in the cache. |
| `key` | Defines the unique cache identifier used to locate matching cached content. |
| `restore-keys` | Defines fallback cache identifiers when the exact cache key cannot match. |
| `enableCrossOsArchive` | Enables supported cache archive sharing between different operating system runners. |
| `lookup-only` | Checks for an existing matching cache without downloading its contents. |
| `fail-on-cache-miss` | Fails the action when the requested cache cannot be found. |

## 13.8 NuGet caching example

```yaml
steps:
  - name: Checkout
    uses: actions/checkout@v4

  - name: Setup .NET
    uses: actions/setup-dotnet@v4
    with:
      dotnet-version: "8.0.x"

  - name: Cache NuGet packages
    uses: actions/cache@v4

    # Cache configuration
    with:

      # Cache directory
      path: ~/.nuget/packages

      # Unique cache key
      key: nuget-${{ runner.os }}-${{ hashFiles('**/*.csproj') }}

      # Fallback cache key
      restore-keys: |
        nuget-${{ runner.os }}-

  - name: Restore dependencies
    run: dotnet restore
```

## 13.9 npm caching example

```yaml
steps:
  - name: Checkout
    uses: actions/checkout@v4

  - name: Setup Node
    uses: actions/setup-node@v4
    with:
      node-version: "22"
      cache: npm

  - name: Install dependencies
    run: npm ci
```

## 13.10 Artifact versus Cache

| Feature | Artifact | Cache |
|---|---|---|
| Main purpose | Preserves generated workflow files for later use or download. | Reuses dependencies and files to accelerate future workflow executions. |
| Common use | Build packages and deployment bundles. | Package manager dependencies and build caches. |
| Typical action | `actions/upload-artifact` | `actions/cache` |
| Retrieval | Download using artifact name. | Restore using matching cache key. |
| Retention | Configurable artifact retention period. | Managed by GitHub cache retention and eviction rules. |
| Cross-job use | Yes, through artifact upload and download. | Yes, when compatible jobs use the same accessible cache key. |

---

# 🏁 14. Complete Workflow Example

### 🎯 What you will learn

- Combine the most important GitHub Actions keywords into one practical workflow.
- Configure push, pull request, and manual triggers.
- Build and test an ASP.NET Core application.
- Publish an application artifact.
- Deploy the published artifact to an EC2 instance.
- Understand how CI and CD jobs depend on one another.

## 14.1 Scenario

Assume we have an ASP.NET Core Web API that needs to be built, tested, and deployed to an AWS EC2 instance.

### Project structure

```text
MyApi/
│
├── .github/
│   └── workflows/
│       └── dotnet-ci-cd.yml
│
├── src/
│   └── MyApi/
│       └── MyApi.csproj
│
├── tests/
│   └── MyApi.Tests/
│       └── MyApi.Tests.csproj
│
└── MyApi.sln
```

### Pipeline flow

```text
Developer Push
      |
      v
GitHub Repository
      |
      v
Workflow Trigger
      |
      v
Build Job
      |
      +---- Checkout
      |
      +---- Setup .NET
      |
      +---- Restore
      |
      +---- Build
      |
      +---- Test
      |
      +---- Publish
      |
      +---- Upload Artifact
      |
      v
Deploy Job
      |
      +---- Download Artifact
      |
      +---- Connect to EC2
      |
      +---- Copy Application
      |
      +---- Restart Application
      |
      v
Application Deployed
```

## 14.2 Required GitHub secrets

Create the following repository secrets before using the deployment example.

| Secret | Purpose |
|---|---|
| `EC2_HOST` | Stores the public IP address or DNS name of the target EC2 instance. |
| `EC2_USER` | Stores the Linux username used for authenticated SSH connections. |
| `EC2_SSH_KEY` | Stores the private SSH key used to authenticate with the EC2 instance. |

Do not write private SSH keys or passwords directly inside workflow YAML files.

## 14.3 Complete CI/CD workflow

**File:** `.github/workflows/dotnet-ci-cd.yml`

```yaml
# ==========================================================
# LEVEL 1: WORKFLOW NAME
# ==========================================================

name: ASP.NET Core CI/CD Pipeline


# ==========================================================
# LEVEL 1: WORKFLOW EXECUTION NAME
# ==========================================================

run-name: Deploy ${{ github.ref_name }} - Run ${{ github.run_number }}


# ==========================================================
# LEVEL 1: WORKFLOW TRIGGERS
# ==========================================================

on:

  # Level 2: Push event
  push:

    # Level 3: Monitor main branch
    branches:
      - main

  # Level 2: Pull request event
  pull_request:

    # Level 3: Target branch
    branches:
      - main

  # Level 2: Manual execution
  workflow_dispatch:


# ==========================================================
# LEVEL 1: WORKFLOW PERMISSIONS
# ==========================================================

permissions:

  # Level 2: Read repository contents
  contents: read


# ==========================================================
# LEVEL 1: GLOBAL ENVIRONMENT VARIABLES
# ==========================================================

env:

  # Level 2: .NET SDK version
  DOTNET_VERSION: "8.0.x"

  # Level 2: Build configuration
  BUILD_CONFIGURATION: Release

  # Level 2: Publish directory
  PUBLISH_DIRECTORY: ./publish


# ==========================================================
# LEVEL 1: WORKFLOW CONCURRENCY
# ==========================================================

concurrency:

  # Level 2: Group matching workflow executions
  group: ${{ github.workflow }}-${{ github.ref }}

  # Level 2: Cancel older matching execution
  cancel-in-progress: true


# ==========================================================
# LEVEL 1: JOBS
# ==========================================================

jobs:


  # ========================================================
  # JOB 1: BUILD AND TEST
  # ========================================================

  build:

    # Level 3: Job display name
    name: Build and Test Application

    # Level 3: Runner
    runs-on: ubuntu-latest

    # Level 3: Job timeout
    timeout-minutes: 20

    # Level 3: Job permissions
    permissions:
      contents: read

    # Level 3: Job steps
    steps:


      # ----------------------------------------------------
      # STEP 1: CHECKOUT
      # ----------------------------------------------------

      - name: Checkout repository

        # Action reference
        uses: actions/checkout@v4

        # Action inputs
        with:
          fetch-depth: 0


      # ----------------------------------------------------
      # STEP 2: SETUP .NET
      # ----------------------------------------------------

      - name: Setup .NET SDK

        # Action reference
        uses: actions/setup-dotnet@v4

        # Action inputs
        with:
          dotnet-version: ${{ env.DOTNET_VERSION }}


      # ----------------------------------------------------
      # STEP 3: RESTORE DEPENDENCIES
      # ----------------------------------------------------

      - name: Restore dependencies

        # Shell command
        run: dotnet restore


      # ----------------------------------------------------
      # STEP 4: BUILD APPLICATION
      # ----------------------------------------------------

      - name: Build application

        # Shell command
        run: dotnet build --configuration $BUILD_CONFIGURATION --no-restore


      # ----------------------------------------------------
      # STEP 5: RUN TESTS
      # ----------------------------------------------------

      - name: Run automated tests

        # Execute only after successful previous steps
        if: success()

        # Shell command
        run: dotnet test --configuration $BUILD_CONFIGURATION --no-build


      # ----------------------------------------------------
      # STEP 6: PUBLISH APPLICATION
      # ----------------------------------------------------

      - name: Publish application

        # Publish only when previous steps succeed
        if: success()

        # Shell command
        run: dotnet publish --configuration $BUILD_CONFIGURATION --output "$PUBLISH_DIRECTORY"


      # ----------------------------------------------------
      # STEP 7: UPLOAD ARTIFACT
      # ----------------------------------------------------

      - name: Upload application artifact

        # Reusable upload action
        uses: actions/upload-artifact@v4

        # Action input configuration
        with:

          # Artifact name
          name: aspnet-application

          # Artifact source directory
          path: ./publish/

          # Retention duration
          retention-days: 7

          # Fail if no files are found
          if-no-files-found: error


  # ========================================================
  # JOB 2: DEPLOY TO EC2
  # ========================================================

  deploy:

    # Level 3: Job display name
    name: Deploy to AWS EC2

    # Level 3: Runner
    runs-on: ubuntu-latest

    # Level 3: Job dependency
    needs: build

    # Level 3: Deploy only on main branch push
    if: github.event_name == 'push' && github.ref == 'refs/heads/main'

    # Level 3: Deployment environment
    environment:

      # Environment name
      name: production

      # Deployment URL
      url: https://example.com

    # Level 3: Job permissions
    permissions:
      contents: read

    # Level 3: Job steps
    steps:


      # ----------------------------------------------------
      # STEP 1: DOWNLOAD ARTIFACT
      # ----------------------------------------------------

      - name: Download application artifact

        # Artifact download action
        uses: actions/download-artifact@v4

        # Action inputs
        with:

          # Artifact name from build job
          name: aspnet-application

          # Download destination
          path: ./publish/


      # ----------------------------------------------------
      # STEP 2: PREPARE SSH KEY
      # ----------------------------------------------------

      - name: Configure SSH key

        # Step-level environment
        env:

          # Private key from GitHub Secrets
          EC2_SSH_KEY: ${{ secrets.EC2_SSH_KEY }}

        # Shell commands
        run: |

          # Create SSH directory
          mkdir -p ~/.ssh

          # Write private key to file
          printf '%s\n' "$EC2_SSH_KEY" > ~/.ssh/ec2_key

          # Restrict private key permissions
          chmod 600 ~/.ssh/ec2_key


      # ----------------------------------------------------
      # STEP 3: COPY APPLICATION TO EC2
      # ----------------------------------------------------

      - name: Copy application files

        # Required environment variables
        env:
          EC2_HOST: ${{ secrets.EC2_HOST }}
          EC2_USER: ${{ secrets.EC2_USER }}

        # Shell commands
        run: |

          # Copy published application files to EC2
          scp -i ~/.ssh/ec2_key \
            -o StrictHostKeyChecking=no \
            -r ./publish/* \
            "$EC2_USER@$EC2_HOST:/var/www/myapi/"


      # ----------------------------------------------------
      # STEP 4: RESTART APPLICATION
      # ----------------------------------------------------

      - name: Restart application

        # Required environment variables
        env:
          EC2_HOST: ${{ secrets.EC2_HOST }}
          EC2_USER: ${{ secrets.EC2_USER }}

        # Shell commands
        run: |

          # Connect to EC2 and restart application service
          ssh -i ~/.ssh/ec2_key \
            -o StrictHostKeyChecking=no \
            "$EC2_USER@$EC2_HOST" \
            "sudo systemctl restart myapi"
```

### ⚠️ Deployment prerequisites

- Configure the EC2 security group to allow SSH from an appropriate trusted source.
- Configure the EC2 instance with the required .NET runtime.
- Create the application directory and configure its ownership and permissions.
- Configure the application as a systemd service before running the restart command.
- Configure SSH host verification securely instead of permanently disabling host key checking in production.
- Protect production deployment environments using GitHub environment rules where appropriate.

### 14.4 What this workflow demonstrates

| Keyword | Used in |
|---|---|
| `name` | Workflow and job names |
| `run-name` | Dynamic workflow execution name |
| `on` | Push, pull request, and manual triggers |
| `branches` | Main branch filtering |
| `permissions` | Workflow and job token permissions |
| `env` | .NET version and build configuration |
| `concurrency` | Matching workflow execution control |
| `group` | Concurrency grouping |
| `cancel-in-progress` | Previous execution cancellation |
| `jobs` | Build and deploy jobs |
| `runs-on` | Ubuntu runner selection |
| `timeout-minutes` | Job execution timeout |
| `needs` | Build-to-deploy dependency |
| `if` | Deployment condition |
| `environment` | Production deployment configuration |
| `steps` | Individual workflow tasks |
| `name` | Step display names |
| `uses` | Checkout, setup, upload, and download actions |
| `with` | Action input configuration |
| `run` | Shell commands |
| `if` | Step execution condition |
| `env` | Step-specific deployment credentials |
| `secrets` | SSH deployment credentials |
| `path` | Artifact and file locations |
| `retention-days` | Artifact retention configuration |

---

# 🔗 15. Important References

### 🎯 What you will learn

This final section provides official GitHub documentation for verifying workflow syntax, discovering action inputs, and exploring advanced automation capabilities.

| Resource | Description |
|---|---|
| [GitHub Actions Documentation](https://docs.github.com/en/actions) | Official documentation covering workflow creation, configuration, execution, and management. |
| [Workflow Syntax](https://docs.github.com/en/actions/reference/workflows-and-actions/workflow-syntax) | Complete reference for workflow YAML keywords and their supported configuration. |
| [Events That Trigger Workflows](https://docs.github.com/en/actions/reference/workflows-and-actions/events-that-trigger-workflows) | Complete reference explaining repository events and their supported activity types. |
| [Contexts Reference](https://docs.github.com/en/actions/reference/workflows-and-actions/contexts) | Explains available contexts and accessing dynamic workflow information through expressions. |
| [Expressions Reference](https://docs.github.com/en/actions/reference/workflows-and-actions/expressions) | Documents expression syntax, operators, functions, and conditional evaluation behavior. |
| [Variables](https://docs.github.com/en/actions/how-tos/write-workflows/choose-what-workflows-do/use-variables) | Explains environment variables and configuration variables in GitHub Actions workflows. |
| [Secrets](https://docs.github.com/en/actions/how-tos/write-workflows/choose-what-workflows-do/use-secrets) | Explains secure storage and usage of sensitive workflow configuration values. |
| [Reusable Workflows](https://docs.github.com/en/actions/how-tos/reuse-automations/reuse-workflows) | Explains how to create and invoke reusable workflow definitions. |
| [Creating Custom Actions](https://docs.github.com/en/actions/sharing-automations/creating-actions) | Explains how to build and configure your own reusable GitHub Actions. |
| [GitHub Marketplace](https://github.com/marketplace?type=actions) | Helps discover published actions for common development and deployment tasks. |
| [GitHub Actions Security](https://docs.github.com/en/actions/security-for-github-actions) | Explains recommended security practices for workflows, actions, tokens, and credentials. |

---

## 🎓 Final Learning Roadmap

| Order | Topic | Learning priority |
|---|---|---|
| 1 | Workflow structure and YAML syntax | Essential |
| 2 | Events and event filters | Essential |
| 3 | Jobs, runners, and dependencies | Essential |
| 4 | Steps, `uses`, `run`, and `with` | Essential |
| 5 | Variables and contexts | Essential |
| 6 | Conditions and expressions | Essential |
| 7 | Outputs and environment files | Important |
| 8 | Permissions and secrets | Essential |
| 9 | Matrix and strategy | Important |
| 10 | Artifacts and caching | Important |
| 11 | Reusable workflows and custom actions | Advanced |
| 12 | CI/CD deployment automation | Practical |

**🚀 Learning Tip:** First become comfortable with `on`, `jobs`, `steps`, `uses`, `run`, `with`, and `needs`. Then practise variables, expressions, outputs, artifacts, matrix strategies, and finally reusable workflows and deployment automation.
