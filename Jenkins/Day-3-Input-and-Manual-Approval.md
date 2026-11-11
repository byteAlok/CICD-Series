# 🛠️ Day 03 – Input and Manual Approval

This document explains how to implement manual user interaction and approval mechanisms in Jenkins Declarative Pipelines using the `input` directive.

It covers how to pause pipeline execution, request user approval, provide selectable choices, restrict approval permissions, and capture the identity of the user who approves a pipeline.

---

# 🔴 1. Introduction to Pipeline Input

In a normal Jenkins Pipeline, stages execute automatically according to the defined instructions.

However, some operations require manual intervention before proceeding.

For example, before deploying an application to a Production environment, an authorized person may need to review and approve the deployment.

Jenkins provides the `input` directive to pause pipeline execution and wait for user interaction.

### 🎯 Why Do We Need Manual Approval?

Consider a deployment workflow:

1. Developer pushes code to GitHub.
2. Jenkins builds the application.
3. Automated tests are executed.
4. Application is prepared for deployment.
5. An authorized user reviews the deployment.
6. The user approves or rejects the operation.
7. Jenkins proceeds according to the user's decision.

### 📌 Workflow

```text
       Pipeline Starts
              |
              ▼
        Build Application
              |
              ▼
         Run Tests
              |
              ▼
       Approval Required
              |
              ▼
       User Input / Review
              |
        ┌─────┴─────┐
        |           |
        ▼           ▼
      Approve      Reject
        |           |
        ▼           ▼
     Continue    Stop/Fail
     Pipeline    Pipeline
```

**Note:** The actual behaviour after rejection depends on how the pipeline handles the input step and its result.

---

# ⚙️ 2. The `input` Directive

The `input` directive pauses the pipeline and waits for a user to provide input or approve the next operation.

### 📌 Basic Syntax

```groovy
input {
    message 'Do you want to continue?'
}
```

The `input` directive can be configured with different options.

### 🔍 Important Input Options

| Option | Purpose |
|---|---|
| `message` | Displays a message to the user. |
| `id` | Assigns a unique identifier to the input step. |
| `ok` | Customizes the approval button text. |
| `parameters` | Requests parameter values from the user. |
| `submitter` | Restricts who can respond to the input. |
| `submitterParameter` | Stores the username of the person who submitted the input. |

---

# 💬 3. `message`

The `message` option displays a message in the Jenkins approval prompt.

It tells the user what action requires approval.

### 📌 Syntax

```groovy
input message: 'Do you want to approve this deployment?'
```

### 🔍 Explanation

- Jenkins reaches the input step.
- Pipeline execution pauses.
- The configured message is displayed.
- The user can review the request and respond.

### 💻 Example

```groovy
pipeline {

    agent any

    stages {

        stage('Approval') {
            steps {

                input message: 'Do you want to approve the deployment?'

            }
        }
    }
}
```

**Example Message:**

```text
Do you want to approve the deployment?

[Proceed]
```

The exact button text depends on the configured `ok` option and Jenkins interface.

---

# 🟢 4. `ok` – Customize Approval Button

The `ok` option changes the text displayed on the approval button.

By default, Jenkins uses `Proceed`.

### 📌 Syntax

```groovy
input message: 'Approve Production Deployment?',
      ok: 'Approve'
```

### 🔍 Explanation

| Option | Description |
|---|---|
| `message` | Displays the approval request. |
| `ok` | Defines the approval button label. |

### 💻 Example

```groovy
pipeline {

    agent any

    stages {

        stage('Production Approval') {
            steps {

                input message: 'Approve Production Deployment?',
                      ok: 'Deploy Now'

            }
        }
    }
}
```

**Result:** The approval button displays `Deploy Now` instead of the default `Proceed`.

---

# 🆔 5. `id` – Input Step Identifier

The `id` option assigns a unique identifier to an input step.

It helps identify a particular input step, especially when a pipeline contains multiple approval requests.

### 📌 Syntax

```groovy
input id: 'production-approval',
      message: 'Approve Production Deployment?'
```

### 🔍 Explanation

- `id` identifies the input step.
- It should be unique within the relevant pipeline.
- It can be useful when interacting with Jenkins input endpoints or identifying input steps.

### 💻 Example

```groovy
pipeline {

    agent any

    stages {

        stage('Approval') {
            steps {

                input id: 'deploy-approval',
                      message: 'Approve Deployment?',
                      ok: 'Approve'

            }
        }
    }
}
```

**Important:** The `id` value is an identifier; it is not an approval password or user permission.

---

# 👥 6. `submitter` – Restrict Approval Permissions

The `submitter` option restricts which users are allowed to respond to an input step.

This is useful when sensitive operations require approval from specific users.

For example, a Production deployment may require approval from an administrator.

### 📌 Syntax

```groovy
input message: 'Approve Production Deployment?',
      submitter: 'admin'
```

### 🔍 Explanation

- Jenkins displays the approval prompt.
- Only users permitted by the configured submitter restriction can respond.
- Other users cannot approve or reject that input request.

Multiple permitted users can be specified as a comma-separated list.

### 💻 Example

```groovy
pipeline {

    agent any

    stages {

        stage('Production Approval') {
            steps {

                input message: 'Approve Production Deployment?',
                      submitter: 'admin,alok'

            }
        }
    }
}
```

**Important:**

- The specified users must exist in Jenkins.
- Jenkins security and authorization must be configured appropriately.
- The `submitter` option controls who may respond to the input; it does not independently create user accounts or grant general Jenkins permissions.

---

# 👤 7. `submitterParameter` – Capture Approver Identity

The `submitterParameter` option captures the username of the person who responds to the input step.

This is useful for recording who approved a deployment.

### 📌 Syntax

```groovy
input message: 'Approve Deployment?',
      submitterParameter: 'APPROVED_BY'
```

The returned username can be stored in a variable and used in subsequent pipeline steps.

### 💻 Example

```groovy
pipeline {

    agent any

    stages {

        stage('Approval') {
            steps {

                script {

                    def approver = input(
                        message: 'Approve Deployment?',
                        submitterParameter: 'APPROVED_BY'
                    )

                    echo "Approved By: ${approver}"

                }
            }
        }
    }
}
```

### 🔍 Explanation

1. Jenkins displays the approval request.
2. A user responds to the input.
3. Jenkins captures the responding user's identity.
4. The value is returned from the `input` step.
5. The pipeline stores it in the `approver` variable.
6. The username is printed using `echo`.

**Example Output:**

```text
Approved By: alok
```

**Important:** When `submitterParameter` is used, the `input` step returns a map containing the configured submitter parameter. The example above captures that returned value as a map. To access the username, use:

```groovy
def response = input(
    message: 'Approve Deployment?',
    submitterParameter: 'APPROVED_BY'
)

echo "Approved By: ${response.APPROVED_BY}"
```

---

# 🔘 8. `parameters` – Request User Input

The `parameters` option allows Jenkins to request values from a user during pipeline execution.

For example, a user may be asked to select an environment before deployment.

### 📌 Basic Syntax

```groovy
input message: 'Select Deployment Environment?',
      parameters: [
          choice(
              name: 'ENVIRONMENT',
              choices: ['DEV', 'QA', 'PRODUCTION'],
              description: 'Choose deployment environment'
          )
      ]
```

### 🔍 Explanation

| Option | Purpose |
|---|---|
| `parameters` | Defines the input parameters. |
| `choice` | Creates a dropdown selection. |
| `name` | Defines the parameter name. |
| `choices` | Defines the available options. |
| `description` | Displays information about the parameter. |

---

# 📋 9. `choice` – Select an Option

The `choice` parameter allows a user to select one option from multiple available choices.

It is commonly used for selecting deployment environments.

### 📌 Syntax

```groovy
choice(
    name: 'ENVIRONMENT',
    choices: ['DEV', 'QA', 'PRODUCTION'],
    description: 'Select Environment'
)
```

### 🔍 Explanation

- `name`: Name of the parameter.
- `choices`: Available selection options.
- `description`: Explains what the user needs to select.

### 💻 Example

```groovy
pipeline {

    agent any

    stages {

        stage('Environment Selection') {

            steps {

                script {

                    def selectedEnvironment = input(
                        message: 'Select Deployment Environment',
                        ok: 'Continue',
                        parameters: [
                            choice(
                                name: 'ENVIRONMENT',
                                choices: ['DEV', 'QA', 'PRODUCTION'],
                                description: 'Choose an environment'
                            )
                        ]
                    )

                    echo "Selected Environment: ${selectedEnvironment}"

                }
            }
        }
    }
}
```

**Example Selection:**

```text
Select Deployment Environment

ENVIRONMENT:

[DEV ▼]

Choices:
DEV
QA
PRODUCTION

[Continue]
```

**Example Output:**

```text
Selected Environment: QA
```

The selected choice is returned by the `input` step.

---

# 🔗 10. Combining Input Options

Multiple input options can be used together to create a more controlled approval process.

For example:

- Display a custom approval message.
- Customize the approval button.
- Restrict approval to specific users.
- Capture the approver's username.
- Ask the approver to select a deployment environment.
- Assign an identifier to the input step.

### 📌 Combined Syntax

```groovy
def response = input(
    id: 'deployment-approval',
    message: 'Review and approve deployment',
    ok: 'Approve Deployment',
    submitter: 'admin,alok',
    submitterParameter: 'APPROVED_BY',
    parameters: [
        choice(
            name: 'ENVIRONMENT',
            choices: ['DEV', 'QA', 'PRODUCTION'],
            description: 'Select deployment environment'
        )
    ]
)
```

### 🔍 Returned Values

The response contains the selected parameter and the username of the person who submitted the input.

For example:

```text
APPROVED_BY = alok
ENVIRONMENT = PRODUCTION
```

**Note:** When the input step returns multiple values, access them through the returned map using their parameter names.

---

# 🧪 11. Practical Implementation

### 🛠️ Creating a Pipeline in Jenkins Dashboard

1. Open the Jenkins Dashboard.
2. Click **New Item**.
3. Enter a pipeline job name.
4. Select **Pipeline**.
5. Click **OK**.
6. Scroll down to the Pipeline section.
7. Select **Pipeline script**.
8. Enter the Declarative Pipeline code.
9. Click **Save**.
10. Click **Build Now**.
11. Wait for Jenkins to reach the input step.
12. Open the pipeline and respond to the approval request.

### 📌 Practical Scenarios

| Scenario | Expected Behaviour |
|---|---|
| Approve | Pipeline continues to the next stage. |
| Deny / Reject | Input is rejected; pipeline does not continue normally. |
| Choice | User selects one available option. |
| Submitter | Only permitted users can respond. |
| Submitter Parameter | Responding user's identity is captured. |

**Important:** The standard `input` step presents an approval action rather than a separate Deny button. Rejecting or cancelling an input request can result in an aborted pipeline. If you want rejection to be treated as a normal failed build, additional pipeline logic is required.

---

# 💻 12. Complete Pipeline Code – Manual Approval

This example demonstrates manual approval before application deployment.

### 📄 Code

```groovy
pipeline {

    agent any

    stages {

        stage('Build') {

            steps {

                echo 'Building Application'
                echo 'Build Completed Successfully'

            }
        }

        stage('Test') {

            steps {

                echo 'Executing Application Tests'
                echo 'All Tests Passed'

            }
        }

        stage('Manual Approval') {

            steps {

                input(
                    id: 'production-approval',
                    message: 'Do you approve Production Deployment?',
                    ok: 'Approve Deployment'
                )

            }
        }

        stage('Deploy') {

            steps {

                echo 'Deploying Application to Production'
                echo 'Deployment Completed Successfully'

            }
        }
    }
}
```

### 🔍 Code Explanation

| Code | Explanation |
|---|---|
| `agent any` | Selects an available eligible agent. |
| `stage('Build')` | Executes the build phase. |
| `stage('Test')` | Executes the testing phase. |
| `stage('Manual Approval')` | Creates a manual approval stage. |
| `input` | Pauses execution and waits for user input. |
| `id` | Identifies the input step. |
| `message` | Displays the approval request. |
| `ok` | Changes the approval button label. |
| `stage('Deploy')` | Executes only after approval is provided. |

### 🖥️ Expected Execution

```text
Build
  |
  ▼
Test
  |
  ▼
Manual Approval
  |
  ▼
Waiting for User
  |
  ▼
User Approves
  |
  ▼
Deploy
  |
  ▼
Finished: SUCCESS
```

If the user rejects or aborts the input, the Deploy stage does not execute.

---

# 💻 13. Complete Pipeline Code – Environment Selection and Approver Tracking

This example combines manual approval, user restrictions, environment selection, and approver identity tracking.

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

        stage('Approval') {

            steps {

                script {

                    def response = input(
                        id: 'deployment-approval',
                        message: 'Approve Application Deployment?',
                        ok: 'Approve',
                        submitter: 'admin,alok',
                        submitterParameter: 'APPROVED_BY',
                        parameters: [
                            choice(
                                name: 'ENVIRONMENT',
                                choices: ['DEV', 'QA', 'PRODUCTION'],
                                description: 'Select Deployment Environment'
                            )
                        ]
                    )

                    echo "Approved By: ${response.APPROVED_BY}"

                    echo "Selected Environment: ${response.ENVIRONMENT}"

                }
            }
        }

        stage('Deploy') {

            steps {

                echo 'Starting Application Deployment'

            }
        }
    }
}
```

### 🔍 Execution Explanation

1. Jenkins executes the Build stage.
2. Jenkins executes the Test stage.
3. The Approval stage displays the input form.
4. Only the configured submitters can respond.
5. The authorized user selects an environment.
6. The user approves the request.
7. Jenkins captures the approver's username.
8. The pipeline prints the selected environment and approver.
9. The Deploy stage executes.

### 🖥️ Example Output

```text
Building Application

Running Application Tests

Approved By: alok

Selected Environment: PRODUCTION

Starting Application Deployment

Finished: SUCCESS
```

**Note:** In this demonstration, the Deploy stage prints a message. It does not perform an actual application deployment.

---

# 📌 14. Important Points to Remember

- `input` pauses pipeline execution until a response is received.
- `message` displays the approval request.
- `ok` customizes the approval button.
- `id` identifies an input step.
- `parameters` defines values requested from the user.
- `choice` provides selectable options.
- `submitter` restricts who can respond.
- `submitterParameter` captures the responding user's identity.
- `input` can be used for deployment approval and other manual decisions.
- A rejected or aborted input does not allow the pipeline to proceed normally.
- Input and approval are useful for controlled Production deployments.

---

# 📚 15. Official Documentation & References

### 🔴 Jenkins Documentation

- [Pipeline Syntax](https://www.jenkins.io/doc/book/pipeline/syntax/)
- [Pipeline Input Step](https://www.jenkins.io/doc/pipeline/steps/pipeline-input-step/)
- [Jenkins Pipeline](https://www.jenkins.io/doc/book/pipeline/)
- [Pipeline Steps Reference](https://www.jenkins.io/doc/pipeline/steps/)

---

## 👨‍💻 About This Learning Section

This document is part of my **CI/CD Series – Jenkins Learning Journey**.

It covers manual pipeline interaction, approval workflows, user input, choice parameters, submitter restrictions, and approver identity tracking through practical implementations.

---

<p align="center">
  <b>👤 Request Input. ✅ Approve Changes. 🚀 Control Deployments.</b>
</p>

<p align="center">
  <i>Learn · Practice · Implement · Improve</i>
</p>
