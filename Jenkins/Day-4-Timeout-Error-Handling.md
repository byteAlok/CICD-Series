# ⚙️ Day 04 – Timeout and Error Handling

This document explains how to control Jenkins Pipeline execution using timeout mechanisms, intentional build failures, and interruption handling.

It covers how to limit the waiting time for manual approvals, handle timeout events, intentionally fail a pipeline, and understand the difference between pipeline failure and pipeline abortion.

---

# 🔴 1. Introduction to Pipeline Execution Control

A Jenkins Pipeline does not always complete successfully.

During pipeline execution, different situations can occur:

- A stage may fail because of an error.
- A manual approval may take too long.
- A user may manually abort the pipeline.
- A pipeline may need to stop after a specific time limit.

Jenkins provides different mechanisms to handle these situations.

### 🎯 Why Do We Need Execution Control?

Consider a Production deployment pipeline that requires manual approval.

Suppose the authorized person does not respond to the approval request.

Without a timeout mechanism, the pipeline may remain paused for an extended period.

Similarly, if a critical operation fails, the pipeline should stop instead of continuing with subsequent stages.

### 📌 Main Concepts

| Concept | Purpose |
|---|---|
| `timeout` | Limits the maximum execution or waiting time. |
| `error` | Intentionally marks the pipeline as failed. |
| Abort | Stops a running pipeline manually. |
| Timeout Handling | Controls what happens when the configured time limit expires. |
| Post Conditions | Executes actions according to the final pipeline result. |

---

# ⏳ 2. `timeout` – Pipeline Time Limit

The `timeout` step limits how long a particular block of pipeline execution can run.

If the specified time limit expires before the block completes, Jenkins interrupts the block.

### 📌 Syntax

```groovy
timeout(time: 5, unit: 'MINUTES') {

    // Pipeline instructions

}
```

### 🔍 Explanation

| Parameter | Purpose |
|---|---|
| `time` | Specifies the maximum allowed time. |
| `unit` | Specifies the time unit. |
| `MINUTES` | Time in minutes. |
| `SECONDS` | Time in seconds. |
| `HOURS` | Time in hours. |
| `DAYS` | Time in days. |

### 📌 Supported Time Units

- `NANOSECONDS`
- `MICROSECONDS`
- `MILLISECONDS`
- `SECONDS`
- `MINUTES`
- `HOURS`
- `DAYS`

### 💻 Example

```groovy
pipeline {

    agent any

    stages {

        stage('Build') {

            steps {

                timeout(time: 2, unit: 'MINUTES') {

                    echo 'Building Application'

                    sleep time: 30, unit: 'SECONDS'

                    echo 'Build Completed'

                }
            }
        }
    }
}
```

### 🔍 Code Explanation

1. Jenkins starts the Build stage.
2. The `timeout` block allows a maximum of two minutes.
3. The application build instructions execute.
4. The `sleep` step waits for 30 seconds.
5. The execution completes before the timeout expires.

**Expected Result:**

```text
Building Application
Build Completed

Finished: SUCCESS
```

**Important:** If the block exceeds the configured time limit, Jenkins interrupts its execution.

---

# ⏰ 3. Timeout During Manual Approval

One of the most useful applications of the `timeout` step is controlling manual approval waiting time.

For example, a Production deployment may require approval within five minutes.

If no one responds during that period, Jenkins interrupts the input operation.

### 📌 Basic Syntax

```groovy
timeout(time: 5, unit: 'MINUTES') {

    input message: 'Approve Production Deployment?',
          ok: 'Approve'

}
```

### 🔍 Explanation

- Jenkins reaches the approval step.
- Pipeline execution pauses.
- The approval request is displayed.
- The user must respond within five minutes.
- If the user does not respond, the timeout expires and Jenkins interrupts the operation.

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

        stage('Approval') {

            steps {

                timeout(time: 1, unit: 'MINUTES') {

                    input message: 'Approve Deployment?',
                          ok: 'Deploy'

                }
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

### 🖥️ Expected Behaviour

| Situation | Result |
|---|---|
| User approves within one minute | Pipeline continues to Deploy. |
| No response within one minute | Timeout interrupts the input. |
| User manually aborts | Pipeline is interrupted. |

**Important:** By default, a timeout interruption results in an aborted pipeline unless the interruption is explicitly handled.

---

# ❌ 4. `error` – Intentionally Fail a Pipeline

The `error` step is used to stop pipeline execution and mark the build as failed.

It is useful when a specific condition is not satisfied and the pipeline must not continue.

### 📌 Syntax

```groovy
error 'Application Build Failed'
```

### 🔍 Explanation

When Jenkins executes the `error` step:

1. The configured error message is displayed.
2. The current pipeline execution is interrupted.
3. Subsequent sequential stages are skipped.
4. The pipeline result becomes `FAILURE`.

### 💻 Example

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

                echo 'Running Application Tests'

            }
        }
    }

    post {

        failure {

            echo 'Pipeline Failed'

        }

    }
}
```

### 🖥️ Expected Output

```text
Starting Application Build

Build Failed Intentionally

Pipeline Failed

Finished: FAILURE
```

**Important:** The `error` step is deliberately used to generate a pipeline failure. It does not automatically indicate that the actual application compilation failed.

---

# 🛑 5. Pipeline Cancellation – Manual Abort

Jenkins allows users to manually stop a running pipeline.

This is useful when:

- A build is taking too long.
- An incorrect deployment has started.
- A pipeline is executing an unnecessary operation.
- An administrator wants to stop a running job.

### 📌 How to Abort a Pipeline

1. Open the Jenkins Dashboard.
2. Open the running pipeline job.
3. Navigate to the running build.
4. Click the red Stop/Abort button.
5. Jenkins interrupts the running pipeline.

### 🔍 What Happens After Abort?

- Running pipeline execution is interrupted.
- Subsequent sequential stages do not execute.
- The pipeline result becomes `ABORTED`.
- The `aborted` post condition can execute.
- The `always` post condition can also execute.

### 💻 Example

```groovy
pipeline {

    agent any

    stages {

        stage('Long Running Task') {

            steps {

                echo 'Task Started'

                sleep time: 120, unit: 'SECONDS'

                echo 'Task Completed'

            }
        }
    }

    post {

        aborted {

            echo 'Pipeline Was Aborted'

        }

        always {

            echo 'Pipeline Execution Finished'

        }
    }
}
```

**Practical:** Run the pipeline and manually click the Stop button while the `sleep` step is executing.

### 🖥️ Expected Result

```text
Task Started

Pipeline Was Aborted

Pipeline Execution Finished

Finished: ABORTED
```

---

# ⚖️ 6. Difference Between Failure, Timeout and Abort

These three situations are important to understand because they produce different pipeline outcomes.

| Situation | Cause | Build Result |
|---|---|---|
| Failure | Pipeline encounters an error or executes `error`. | FAILURE |
| Timeout | Configured execution time expires. | ABORTED by default |
| Manual Abort | User manually stops the pipeline. | ABORTED |

### 📊 Understanding the Difference

**1. FAILURE**

A pipeline fails because an operation could not complete successfully or an explicit error was raised.

Example:

```groovy
error 'Something went wrong'
```

**2. TIMEOUT**

A pipeline exceeds its configured execution time limit.

Example:

```groovy
timeout(time: 1, unit: 'MINUTES') {

    sleep time: 120, unit: 'SECONDS'

}
```

**3. ABORTED**

A user manually stops a running pipeline.

For example, clicking the Stop button in Jenkins.

### 🎯 Important Observation

Timeout and manual abort are both interruption events.

However, their causes are different:

- Timeout: Jenkins interrupts execution because the configured time limit expires.
- Abort: A user or external interruption stops the execution.

Both normally result in `ABORTED`.

---

# 🔄 7. Handling Different Execution Outcomes Using Post

Jenkins Declarative Pipeline provides the `post` section to handle different pipeline results.

We studied the following conditions in Day 02:

| Condition | Purpose |
|---|---|
| `success` | Executes after successful completion. |
| `failure` | Executes after failed completion. |
| `aborted` | Executes after an aborted pipeline. |
| `always` | Executes regardless of the final result. |

These conditions can be combined with timeout and error handling.

### 💻 Example

```groovy
pipeline {

    agent any

    stages {

        stage('Build') {

            steps {

                echo 'Starting Build'

                error 'Build Failed'

            }
        }
    }

    post {

        success {

            echo 'Build Successful'

        }

        failure {

            echo 'Build Failed'

        }

        aborted {

            echo 'Build Aborted'

        }

        always {

            echo 'Pipeline Execution Finished'

        }
    }
}
```

**Expected Result:**

```text
Starting Build

Build Failed

Pipeline Execution Finished

Finished: FAILURE
```

Only the relevant result condition and `always` execute.

---

# 🧪 8. Practical Implementation

### 🛠️ Creating a Pipeline in Jenkins Dashboard

1. Open Jenkins Dashboard.
2. Click **New Item**.
3. Enter a pipeline job name.
4. Select **Pipeline**.
5. Click **OK**.
6. Scroll down to the Pipeline section.
7. Select **Pipeline script**.
8. Enter the required code.
9. Click **Save**.
10. Click **Build Now**.
11. Observe the pipeline execution.
12. Test timeout, failure, and manual cancellation separately.

### 📌 Practical Scenarios

| Practical | Action | Expected Result |
|---|---|---|
| Timeout Test | Set a short timeout and wait. | ABORTED |
| Error Test | Execute `error`. | FAILURE |
| Manual Abort Test | Click Stop during execution. | ABORTED |
| Successful Execution | Complete all stages within time. | SUCCESS |
| Post Condition Test | Add result-specific post blocks. | Appropriate message executes |

---

# 💻 9. Complete Pipeline Code – Approval with Timeout

This practical combines manual approval, timeout handling, and post-build conditions.

### 📄 Code

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

                echo 'Running Application Tests'

            }
        }

        stage('Manual Approval') {

            steps {

                timeout(time: 1, unit: 'MINUTES') {

                    input(
                        message: 'Approve Production Deployment?',
                        ok: 'Approve'
                    )

                }
            }
        }

        stage('Deploy') {

            steps {

                echo 'Deploying Application to Production'

            }
        }
    }

    post {

        success {

            echo 'SUCCESS: Pipeline Completed'

        }

        failure {

            echo 'FAILURE: Pipeline Failed'

        }

        aborted {

            echo 'ABORTED: Pipeline Interrupted or Timed Out'

        }

        always {

            echo 'Pipeline Execution Finished'

        }
    }
}
```

### 🔍 Execution Explanation

1. Jenkins executes the Build stage.
2. Jenkins executes the Test stage.
3. The Manual Approval stage starts.
4. Jenkins waits for the user's response.
5. The approval request has a maximum waiting time of one minute.
6. If approved in time, Jenkins executes Deploy.
7. If the timeout expires, Jenkins interrupts the input.
8. The appropriate post conditions execute.

### 🖥️ Expected Results

| User Action | Result |
|---|---|
| Approves within one minute | Deployment executes; SUCCESS |
| Does not respond | Deployment skipped; ABORTED |
| Manually aborts | Deployment skipped; ABORTED |

---

# 💻 10. Complete Pipeline Code – Error and Execution Control

This practical demonstrates a controlled pipeline failure.

### 📄 Code

```groovy
pipeline {

    agent any

    stages {

        stage('Build') {

            steps {

                echo 'Starting Application Build'

                sleep time: 5, unit: 'SECONDS'

                echo 'Build Completed'

            }
        }

        stage('Validation') {

            steps {

                echo 'Validating Application'

                script {

                    def validationPassed = false

                    if (!validationPassed) {

                        error 'Validation Failed'

                    }

                    echo 'Validation Successful'

                }
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

            echo 'SUCCESS: Application Ready'

        }

        failure {

            echo 'FAILURE: Validation Failed'

        }

        aborted {

            echo 'ABORTED: Execution Stopped'

        }

        always {

            echo 'Pipeline Execution Finished'

        }
    }
}
```

### 🔍 Execution Explanation

**Case 1: `validationPassed = false`**

1. Jenkins starts the Build stage.
2. The Build stage executes successfully.
3. Jenkins moves to the Validation stage.
4. The condition `!validationPassed` becomes `true`.
5. The `error` step executes and marks the pipeline as failed.
6. The Deploy stage is skipped.
7. The `failure` condition executes.
8. The `always` condition executes.

### 🖥️ Expected Output

**Case 1: `validationPassed = false`**

```text
Starting Application Build
Build Completed

Validating Application

FAILURE: Validation Failed
Pipeline Execution Finished

Finished: FAILURE
```

**Case 2: `validationPassed = true`**

1. Jenkins starts the Build stage.
2. The Build stage executes successfully.
3. Jenkins moves to the Validation stage.
4. The condition `!validationPassed` becomes `false`.
5. The `error` step is skipped.
6. The Validation Successful message is printed.
7. The Deploy stage executes.
8. The `success` and `always` conditions execute.

### 🖥️ Expected Output

**Case 2: `validationPassed = true`**

```text
Starting Application Build
Build Completed

Validating Application
Validation Successful

Deploying Application

SUCCESS: Application Ready
Pipeline Execution Finished

Finished: SUCCESS
```

---

# 📌 11. Important Points to Remember

- `timeout` limits how long a pipeline block can execute.
- `time` specifies the duration.
- `unit` specifies the time measurement.
- Timeout can be applied to an entire pipeline or an individual operation.
- Timeout around an input step is useful for approval deadlines.
- A timeout interruption normally results in `ABORTED`.
- `error` intentionally marks a pipeline as `FAILURE`.
- Manual cancellation results in `ABORTED`.
- Failure and abortion are different pipeline results.
- `post` can handle success, failure, and aborted outcomes.
- `always` executes regardless of the final pipeline result.

---

# 📚 12. Official Documentation & References

### 🔴 Jenkins Documentation

- [Pipeline Syntax](https://www.jenkins.io/doc/book/pipeline/syntax/)
- [Pipeline Basic Steps](https://www.jenkins.io/doc/pipeline/steps/workflow-basic-steps/)
- [Jenkins Pipeline](https://www.jenkins.io/doc/book/pipeline/)
- [Pipeline Steps Reference](https://www.jenkins.io/doc/pipeline/steps/)

---

## 👨‍💻 About This Learning Section

This document is part of my **CI/CD Series – Jenkins Learning Journey**.

It covers pipeline execution limits, manual cancellation, intentional failures, timeout handling, and build result management through practical implementations.

---

<p align="center">
  <b>⏳ Control Execution. ❌ Handle Failures. 🛑 Manage Interruptions.</b>
</p>

<p align="center">
  <i>Learn · Practice · Implement · Improve</i>
</p>
