# ⚙️ Day 02 – Declarative Pipeline and Post Actions

This document explains the fundamentals of Jenkins Declarative Pipeline, its structure, commonly used pipeline directives, and post-build actions.

It covers how to create a pipeline, organize execution into stages, execute steps, and handle different build outcomes using the `post` section.

---

# 🔴 1. Introduction to Jenkins Pipeline

A **Jenkins Pipeline** is a set of automated instructions that defines how an application should be built, tested, packaged, and deployed.

Instead of manually creating and configuring every build operation, Jenkins allows us to define the complete automation workflow using code.

A Jenkins Pipeline can be written inside the Jenkins dashboard or stored in a file named `Jenkinsfile` inside a source code repository.

### 🎯 Why Do We Need Jenkins Pipeline?

Consider an application that requires multiple operations:

1. Retrieve source code.
2. Build the application.
3. Execute tests.
4. Package the application.
5. Deploy the application.

A pipeline organizes these operations into a structured workflow.

### 📌 Basic Pipeline Workflow

```text
       Jenkins Pipeline
              |
              ▼
        Stage 1: Build
              |
              ▼
        Stage 2: Test
              |
              ▼
        Stage 3: Deploy
              |
              ▼
       Pipeline Completed
              |
              ▼
         Post Actions
```

### ⚙️ Key Features

- Pipeline as Code.
- Multiple stages and steps.
- Automated execution.
- Build status handling.
- Integration with source code repositories.
- Reusable and maintainable automation workflows.

---

# 🏗️ 2. Types of Jenkins Pipeline

Jenkins primarily supports two pipeline syntax styles.

| Pipeline Type | Description |
|---|---|
| Declarative Pipeline | Structured and predefined syntax for writing pipelines. |
| Scripted Pipeline | Flexible, Groovy-based syntax that provides greater programming control. |

### 🔴 Declarative Pipeline

Declarative Pipeline uses a predefined structure with mandatory sections such as `pipeline`, `agent`, `stages`, and `steps`.

Example:

```groovy
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building Application'
            }
        }
    }
}
```

### 🔵 Scripted Pipeline

Scripted Pipeline uses Groovy programming syntax and provides more flexibility for complex automation logic.

Example:

```groovy
node {
    stage('Build') {
        echo 'Building Application'
    }
}
```

### ⚖️ Declarative vs Scripted Pipeline

| Feature | Declarative | Scripted |
|---|---|---|
| Syntax | Structured | Groovy-based |
| Learning Curve | Easier for beginners | Requires more programming knowledge |
| Structure | Predefined blocks | Flexible programming structure |
| Flexibility | Controlled structure | Greater flexibility |
| Error Handling | Declarative directives | Groovy exception handling |
| Usage | Common CI/CD workflows | Complex custom workflows |

**Note:** Our practical implementation focuses on Declarative Pipeline.

---

# 🧱 3. Declarative Pipeline Structure

A Declarative Pipeline follows a structured syntax.

### 📌 Basic Structure

```groovy
pipeline {

    agent any

    stages {

        stage('Stage Name') {

            steps {

                echo 'Hello Jenkins'

            }
        }
    }
}
```

### 🔍 Explanation of Each Component

| Component | Purpose |
|---|---|
| `pipeline` | Defines the complete Declarative Pipeline. |
| `agent` | Specifies where the pipeline or its stages execute. |
| `stages` | Contains one or more stages. |
| `stage` | Defines an individual stage of the pipeline. |
| `steps` | Contains the instructions executed inside a stage. |
| `echo` | Prints a message in the Jenkins build console output. |

### 1. `pipeline`

The `pipeline` block is the root element of a Declarative Pipeline.

All other pipeline sections are defined inside it.

```groovy
pipeline {

    // Pipeline configuration

}
```

### 2. `agent`

The `agent` directive specifies where Jenkins should execute the pipeline.

Example:

```groovy
agent any
```

**Meaning:** Jenkins can select any available eligible agent to execute the pipeline.

Some commonly used agent declarations:

| Agent | Purpose |
|---|---|
| `agent any` | Run on any available eligible agent. |
| `agent none` | Do not allocate a global agent; stages must define their own agent when execution is required. |
| `agent { label 'slave1' }` | Run on an agent matching the `slave1` label. |

**Important:** Agent selection and distributed execution will be covered in the Controller-Agent section.

### 3. `stages`

The `stages` block contains the different stages of the pipeline.

```groovy
stages {

    stage('Build') {
        steps {
            echo 'Building Application'
        }
    }

    stage('Test') {
        steps {
            echo 'Testing Application'
        }
    }
}
```

In this example, Jenkins executes the Build stage first and then the Test stage.

### 4. `stage`

A `stage` represents an individual phase of the pipeline.

For example:

- Build
- Test
- Package
- Deploy

```groovy
stage('Build') {

    steps {
        echo 'Build Stage Executed'
    }
}
```

### 5. `steps`

The `steps` block contains the actual instructions that Jenkins executes inside a stage.

```groovy
steps {

    echo 'Hello Jenkins'

}
```

A stage can contain multiple steps.

### 6. `echo`

The `echo` command prints a message in the Jenkins Console Output.

```groovy
echo 'Hello Jenkins'
```

**Output:**

```text
Hello Jenkins
```

It is commonly used to display pipeline execution messages and debugging information.

---

# 🔄 4. Sequential Stage Execution

By default, stages in a normal Declarative Pipeline execute sequentially.

This means Jenkins completes one stage before moving to the next stage.

### 📌 Example Workflow

```text
Pipeline Starts
      |
      ▼
  Build Stage
      |
      ▼
  Test Stage
      |
      ▼
 Package Stage
      |
      ▼
  Deploy Stage
      |
      ▼
 Pipeline Ends
```

### 💻 Example

```groovy
pipeline {

    agent any

    stages {

        stage('Build') {
            steps {
                echo 'Building Application'
            }
        }

        stage('Test') {
            steps {
                echo 'Testing Application'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying Application'
            }
        }
    }
}
```

### 🖥️ Expected Console Output

```text
[Pipeline] Start of Pipeline

[Pipeline] stage
[Pipeline] { (Build)
Building Application
[Pipeline] }

[Pipeline] stage
[Pipeline] { (Test)
Testing Application
[Pipeline] }

[Pipeline] stage
[Pipeline] { (Deploy)
Deploying Application
[Pipeline] }

[Pipeline] End of Pipeline
Finished: SUCCESS
```

**Note:** Actual console output may contain additional Jenkins log messages.

---

# 📬 5. Post-Build Actions

The `post` section defines actions that execute after the pipeline or its stages have completed.

It is commonly used for:

- Displaying completion messages.
- Reporting successful builds.
- Reporting failed builds.
- Handling aborted builds.
- Executing cleanup or notification operations.

### 📌 Basic Syntax

```groovy
pipeline {

    agent any

    stages {

        stage('Build') {
            steps {
                echo 'Building Application'
            }
        }
    }

    post {

        always {
            echo 'Pipeline Execution Completed'
        }

    }
}
```

### 🔍 Important Post Conditions

Jenkins Declarative Pipeline provides different post conditions.

| Condition | Execution |
|---|---|
| `always` | Executes regardless of the pipeline result. |
| `success` | Executes when the pipeline completes successfully. |
| `failure` | Executes when the pipeline fails. |
| `aborted` | Executes when the pipeline is aborted. |
| `unstable` | Executes when the pipeline result is unstable. |
| `changed` | Executes when the current pipeline result differs from its previous result. |
| `fixed` | Executes when the previous result was not successful and the current result is successful. |
| `regression` | Executes when the previous result was successful and the current result is not successful. |
| `unsuccessful` | Executes when the pipeline result is not successful. |
| `cleanup` | Executes after the other post conditions have been evaluated. |

**Our practical focus:** `always`, `success`, `failure`, and `aborted`.

---

# ✅ 6. `success` Condition

The `success` condition executes only when the pipeline completes successfully.

### 📌 Syntax

```groovy
post {

    success {
        echo 'Pipeline Executed Successfully'
    }

}
```

### 🔍 Explanation

- Jenkins executes the pipeline stages.
- If all required stages complete successfully, the pipeline result becomes `SUCCESS`.
- The `success` block executes.
- If the pipeline fails or is aborted, this block does not execute.

### 💻 Example

```groovy
pipeline {

    agent any

    stages {

        stage('Build') {
            steps {
                echo 'Application Build Successful'
            }
        }
    }

    post {

        success {
            echo 'Build Completed Successfully'
        }

    }
}
```

**Expected Result:**

```text
Application Build Successful
Build Completed Successfully

Finished: SUCCESS
```

---

# ❌ 7. `failure` Condition

The `failure` condition executes when the pipeline result is `FAILURE`.

It is useful for displaying failure messages or initiating failure-related notifications.

### 📌 Syntax

```groovy
post {

    failure {
        echo 'Pipeline Execution Failed'
    }

}
```

### 🔍 Explanation

- Jenkins starts executing the pipeline.
- If a stage fails, the pipeline normally enters the failure state.
- The `failure` block executes after pipeline execution finishes.
- The `success` block does not execute.

### 💻 Example Using `error`

```groovy
pipeline {

    agent any

    stages {

        stage('Build') {
            steps {
                echo 'Starting Build'

                error 'Build Failed Intentionally'
            }
        }
    }

    post {

        failure {
            echo 'Build Failure Detected'
        }

    }
}
```

**Expected Result:**

```text
Starting Build

Build Failure Detected

Finished: FAILURE
```

**Important:** The `error` step deliberately marks the build as failed. It is used here only to demonstrate the `failure` condition.

---

# ⛔ 8. `aborted` Condition

The `aborted` condition executes when a pipeline is manually or externally aborted.

For example, a user clicks the Abort button while a pipeline is running.

### 📌 Syntax

```groovy
post {

    aborted {
        echo 'Pipeline Was Aborted'
    }

}
```

### 🔍 Explanation

1. Jenkins starts the pipeline.
2. A stage begins executing.
3. The user manually aborts the running build.
4. Jenkins stops the pipeline execution.
5. The `aborted` condition executes.

### 💻 Example

```groovy
pipeline {

    agent any

    stages {

        stage('Build') {
            steps {
                echo 'Build Started'

                sleep time: 60, unit: 'SECONDS'

                echo 'Build Completed'
            }
        }
    }

    post {

        aborted {
            echo 'Build Was Aborted By User'
        }

    }
}
```

**Practical:** Start the pipeline and click the red Stop/Abort button while the `sleep` step is running.

**Expected Result:**

```text
Build Started

Build Was Aborted By User

Finished: ABORTED
```

---

# 🔁 9. `always` Condition

The `always` condition executes regardless of whether the pipeline succeeds, fails, or is aborted.

It is useful for operations that should be performed after every pipeline execution.

### 📌 Syntax

```groovy
post {

    always {
        echo 'Pipeline Execution Finished'
    }

}
```

### 🔍 Explanation

| Pipeline Result | `always` Executes? |
|---|---|
| SUCCESS | ✅ Yes |
| FAILURE | ✅ Yes |
| ABORTED | ✅ Yes |
| UNSTABLE | ✅ Yes |

### 💻 Example

```groovy
pipeline {

    agent any

    stages {

        stage('Build') {
            steps {
                echo 'Application Build Started'
            }
        }
    }

    post {

        always {
            echo 'This Message Always Executes'
        }

    }
}
```

**Expected Result:**

Even if a pipeline fails or is aborted, the `always` block is still evaluated and executed.

---

# 🔀 10. Multiple Post Conditions Together

A pipeline can contain multiple post conditions.

Jenkins evaluates the conditions according to the final pipeline result.

### 📌 Example

```groovy
post {

    success {
        echo 'Pipeline Successful'
    }

    failure {
        echo 'Pipeline Failed'
    }

    aborted {
        echo 'Pipeline Aborted'
    }

    always {
        echo 'Pipeline Execution Finished'
    }

}
```

### 📊 Execution Behaviour

| Build Result | Executed Post Conditions |
|---|---|
| SUCCESS | `success` + `always` |
| FAILURE | `failure` + `always` |
| ABORTED | `aborted` + `always` |

**Important:** `always` is not an alternative to `success`, `failure`, or `aborted`. It is an additional post condition.

---

# 🧪 11. Practical Implementation

Now let's combine the concepts into practical Jenkins pipelines.

### 🛠️ Creating a Pipeline in Jenkins Dashboard

1. Open the Jenkins Dashboard.
2. Click **New Item**.
3. Enter a pipeline job name.
4. Select **Pipeline**.
5. Click **OK**.
6. Scroll down to the Pipeline section.
7. Select **Pipeline script**.
8. Write the Declarative Pipeline code.
9. Click **Save**.
10. Click **Build Now**.
11. Open **Console Output** to inspect execution logs.

### 📌 Understanding the Build Results

| Status | Meaning |
|---|---|
| SUCCESS | Pipeline completed successfully. |
| FAILURE | Pipeline execution failed. |
| ABORTED | Pipeline execution was stopped. |
| UNSTABLE | Pipeline completed with an unstable result. |

---

# 💻 12. Complete Pipeline Code – Successful Execution

This example demonstrates a complete Declarative Pipeline containing multiple stages and post-build conditions.

### 📄 Code

```groovy
pipeline {

    agent any

    stages {

        stage('Build') {
            steps {
                echo 'Starting Application Build'
                echo 'Application Build Completed'
            }
        }

        stage('Test') {
            steps {
                echo 'Executing Application Tests'
                echo 'All Tests Passed'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying Application'
                echo 'Application Deployed Successfully'
            }
        }
    }

    post {

        success {
            echo 'SUCCESS: Pipeline Executed Successfully'
        }

        failure {
            echo 'FAILURE: Pipeline Execution Failed'
        }

        aborted {
            echo 'ABORTED: Pipeline Was Stopped'
        }

        always {
            echo 'Pipeline Execution Finished'
        }

    }
}
```

### 🔍 Code Explanation

| Code | Explanation |
|---|---|
| `pipeline` | Defines the Declarative Pipeline. |
| `agent any` | Allows Jenkins to select an available eligible agent. |
| `stages` | Defines the pipeline stages. |
| `stage('Build')` | Represents the application build phase. |
| `stage('Test')` | Represents the testing phase. |
| `stage('Deploy')` | Represents the deployment phase. |
| `steps` | Contains the executable instructions. |
| `echo` | Prints messages in Console Output. |
| `post` | Defines actions after pipeline completion. |
| `success` | Executes when the pipeline succeeds. |
| `failure` | Executes when the pipeline fails. |
| `aborted` | Executes when the pipeline is aborted. |
| `always` | Executes regardless of the final result. |

### 🖥️ Expected Output

```text
Starting Application Build
Application Build Completed

Executing Application Tests
All Tests Passed

Deploying Application
Application Deployed Successfully

SUCCESS: Pipeline Executed Successfully
Pipeline Execution Finished

Finished: SUCCESS
```

---

# 💻 13. Complete Pipeline Code – Failure Handling

This example intentionally fails the Build stage to demonstrate failure handling.

### 📄 Code

```groovy
pipeline {

    agent any

    stages {

        stage('Build') {
            steps {
                echo 'Starting Application Build'

                error 'Build Failed Intentionally'
            }
        }

        stage('Test') {
            steps {
                echo 'Executing Application Tests'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying Application'
            }
        }
    }

    post {

        success {
            echo 'SUCCESS: Pipeline Executed Successfully'
        }

        failure {
            echo 'FAILURE: Build Failed'
        }

        aborted {
            echo 'ABORTED: Pipeline Was Stopped'
        }

        always {
            echo 'Pipeline Execution Finished'
        }

    }
}
```

### 🔍 Execution Explanation

1. Jenkins starts the pipeline.
2. The Build stage begins.
3. The `echo` message is printed.
4. The `error` step marks the build as failed.
5. The Test and Deploy stages are skipped.
6. The `failure` post condition executes.
7. The `always` post condition executes.

### 🖥️ Expected Output

```text
Starting Application Build

FAILURE: Build Failed
Pipeline Execution Finished

Finished: FAILURE
```

**Important:** The `error` step is intentionally used to generate a failure. It is not an actual application compilation error.

---

# 📌 14. Important Points to Remember

- A Jenkins Declarative Pipeline starts with the `pipeline` block.
- `agent` specifies the execution environment.
- `stages` contains multiple pipeline stages.
- `stage` represents an individual phase.
- `steps` contains the executable instructions.
- `echo` prints messages in the Console Output.
- Normal sequential stages execute one after another.
- `post` defines actions after pipeline execution.
- `success` handles successful execution.
- `failure` handles failed execution.
- `aborted` handles aborted execution.
- `always` executes regardless of the final pipeline result.
- A failed stage normally prevents subsequent sequential stages from executing.

---

# 📚 15. Official Documentation & References

### 🔴 Jenkins Documentation

- [Jenkins Pipeline](https://www.jenkins.io/doc/book/pipeline/)
- [Declarative Pipeline Syntax](https://www.jenkins.io/doc/book/pipeline/syntax/)
- [Pipeline Steps Reference](https://www.jenkins.io/doc/pipeline/steps/)
- [Jenkinsfile](https://www.jenkins.io/doc/book/pipeline/jenkinsfile/)

### 📖 Further Reading

- [Pipeline Syntax Generator](https://www.jenkins.io/doc/book/pipeline/getting-started/#snippet-generator)
- [Jenkins Pipeline Examples](https://www.jenkins.io/doc/pipeline/examples/)

---

## 👨‍💻 About This Learning Section

This document is part of my **CI/CD Series – Jenkins Learning Journey**.

It covers Declarative Pipeline fundamentals, pipeline structure, sequential stage execution, and post-build actions through practical implementations.

---

<p align="center">
  <b>⚙️ Define Pipelines. 🔄 Automate Execution. 🚀 Handle Build Results.</b>
</p>

<p align="center">
  <i>Learn · Practice · Implement · Improve</i>
</p>
