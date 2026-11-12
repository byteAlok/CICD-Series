# 📘 Day 09 – Jenkinsfile & Pipeline as Code

## 🎯 Learning Objectives

By the end of this session, you will understand how to create and execute Jenkinsfiles, configure Pipeline jobs, connect Jenkins with GitHub repositories, manage credentials for private repositories, and troubleshoot common integration issues.

---

## 1. 📖 Introduction to Jenkinsfile

A Jenkinsfile is a text file that contains the definition of a Jenkins Pipeline. It allows us to store pipeline instructions alongside our application source code in a version control repository such as GitHub.

### 🔍 What is Pipeline as Code?

Pipeline as Code means defining CI/CD pipeline instructions in a file instead of configuring the entire pipeline manually through the Jenkins interface.

### ⭐ Benefits of Jenkinsfile

- 📦 Pipeline configuration is stored in Git.
- 🕒 Pipeline changes can be tracked through version control.
- 👥 Multiple developers can collaborate on pipeline changes.
- ♻️ Pipeline configuration can be reused and maintained.
- 🔗 The pipeline definition stays alongside application source code.

### ⚖️ Pipeline Script vs Jenkinsfile

| Pipeline Script | Jenkinsfile |
|---|---|
| Written directly inside the Jenkins interface. | Stored as a file in a repository. |
| Configured through the Pipeline Script editor. | Retrieved from SCM, such as GitHub. |
| Changes are managed through Jenkins configuration. | Changes can be tracked through Git. |
| Suitable for simple pipeline experiments. | Suitable for maintaining pipelines as code. |

### 📝 Is the Jenkinsfile Name Mandatory?

No. `Jenkinsfile` is the default and recommended filename, but it is not mandatory.

You can give your pipeline file any meaningful name, for example:

- `My-CICD-Pipeline`
- `Application-Build`
- `Production-Deployment`

The important thing is that the filename and its location must match the **Script Path** configured in Jenkins.

**📂 Example:**

Suppose your GitHub repository contains:

```text
MyRepository/
│
└── Jenkins/
    │
    └── Pipelines/
        │
        └── My-CICD-Pipeline
```

Then the Script Path in Jenkins must be:

```text
Jenkins/Pipelines/My-CICD-Pipeline
```

If you change the filename or move the file to another directory, update the Script Path accordingly.

**💡 Important:** The Script Path is relative to the root of the Git repository. Do not provide the Jenkins server's local filesystem path.

---

## 2. 🏗️ Declarative Jenkinsfile

Declarative Pipeline provides a structured and readable way to define Jenkins pipelines.

### 📝 Basic Structure

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

    post {
        success {
            echo 'Pipeline Completed Successfully'
        }

        failure {
            echo 'Pipeline Failed'
        }

        always {
            echo 'Pipeline Execution Finished'
        }
    }
}
```

### 📚 Important Declarative Pipeline Directives

| Directive | Purpose |
|---|---|
| `pipeline` | Defines the complete pipeline. |
| `agent` | Specifies where the pipeline runs. |
| `stages` | Groups all pipeline stages. |
| `stage` | Defines an individual stage. |
| `steps` | Contains executable pipeline steps. |
| `environment` | Defines environment variables. |
| `parameters` | Accepts user-provided inputs. |
| `options` | Configures pipeline execution options. |
| `triggers` | Configures automatic pipeline triggers. |
| `when` | Controls conditional stage execution. |
| `tools` | Configures supported build tools. |
| `input` | Requests manual approval. |
| `post` | Defines actions after pipeline execution. |

### ⚙️ Common Pipeline Options

| Option | Purpose |
|---|---|
| `timeout()` | Limits pipeline execution time. |
| `timestamps()` | Displays timestamps in console output. |
| `buildDiscarder()` | Manages old build history. |
| `disableConcurrentBuilds()` | Prevents concurrent executions of the same pipeline. |
| `skipDefaultCheckout()` | Controls automatic source checkout. |

### 🧪 Practical Areas

- 🌍 Define environment variables.
- 📝 Accept user inputs through parameters.
- 🔀 Execute stages conditionally using `when`.
- ⏱️ Configure build timeout and retention.
- ✋ Request manual approval.
- 📋 Handle successful and failed executions using `post`.

---

## 3. ⚙️ Scripted Jenkinsfile

Scripted Pipeline uses Groovy-based syntax and provides greater programming flexibility.

### 📝 Basic Structure

```groovy
node('slave1') {

    stage('Build') {
        echo 'Building Application'
        sh 'echo Application Build Successful'
    }

    stage('Test') {
        echo 'Running Tests'
        sh 'echo All Tests Passed'
    }

    stage('Deploy') {
        echo 'Deploying Application'
        sh 'echo Deployment Successful'
    }
}
```

### 📚 Important Concepts

- 🖥️ `node` and `stage`.
- 📦 Variables and environment variables.
- 🔀 Conditional execution using `if-else`.
- 🔁 Loops and collections.
- 🧩 Methods and closures.
- ⚡ Parallel execution.
- 🛡️ `try-catch-finally`.
- 📊 Build result handling.

---

## 4. 🔐 Environment Variables, Parameters & Credentials

### 4.1 🌍 Jenkins Environment Variables

Jenkins provides built-in environment variables that can be accessed through `env`.

| Variable | Description |
|---|---|
| `env.BUILD_NUMBER` | Current build number. |
| `env.BUILD_ID` | Current build ID. |
| `env.JOB_NAME` | Current Jenkins job name. |
| `env.BUILD_URL` | URL of the current build. |
| `env.WORKSPACE` | Workspace directory. |
| `env.NODE_NAME` | Name of the node executing the build. |
| `env.BRANCH_NAME` | Branch name when available, such as in Multibranch Pipelines. |

**Example:**

```groovy
pipeline {
    agent any

    stages {
        stage('Build Information') {
            steps {
                echo "Job Name: ${env.JOB_NAME}"
                echo "Build Number: ${env.BUILD_NUMBER}"
                echo "Workspace: ${env.WORKSPACE}"
            }
        }
    }
}
```

### 4.2 📝 Build Parameters

Parameters allow users to provide values before a pipeline starts.

**Common parameter types:**

- String
- Boolean
- Choice
- Password

**Example:**

```groovy
pipeline {
    agent any

    parameters {
        choice(
            name: 'ENVIRONMENT',
            choices: ['Development', 'Testing', 'Production'],
            description: 'Select deployment environment'
        )
    }

    stages {
        stage('Deployment') {
            steps {
                echo "Selected Environment: ${params.ENVIRONMENT}"
            }
        }
    }
}
```

### 4.3 🔑 Jenkins Credentials

Jenkins Credentials Store allows sensitive information to be stored securely instead of hardcoding it in pipeline files.

**Common credential types:**

| Credential Type | Purpose |
|---|---|
| Username with password | Username and password or token-based authentication. |
| Secret text | API tokens and other secret strings. |
| Secret file | Sensitive files such as configuration files. |
| SSH Username with private key | SSH authentication. |

### 🔒 Using Credentials in a Pipeline

Example using Secret Text:

```groovy
pipeline {
    agent any

    stages {
        stage('Authentication') {
            steps {
                withCredentials([
                    string(
                        credentialsId: 'api-secret',
                        variable: 'API_KEY'
                    )
                ]) {
                    sh 'echo "API key is available securely"'
                }
            }
        }
    }
}
```

**⚠️ Security:** Never hardcode passwords, access tokens, or private keys in a Jenkinsfile.

---

## 5. 🧰 Essential Jenkins Pipeline Steps

These steps are useful when building and managing real applications.

| Step | Purpose |
|---|---|
| `sh` | Executes Linux shell commands. |
| `bat` | Executes Windows commands. |
| `pwd` | Displays the current directory. |
| `dir` | Changes the working directory. |
| `deleteDir` | Deletes workspace contents. |
| `fileExists` | Checks whether a file exists. |
| `writeFile` | Creates or writes a file. |
| `readFile` | Reads a file. |
| `stash` | Saves files for use in another stage or node. |
| `unstash` | Restores previously stashed files. |
| `archiveArtifacts` | Archives build artifacts. |
| `junit` | Publishes JUnit test reports. |
| `retry` | Retries a block of steps. |
| `catchError` | Handles errors while controlling build and stage results. |

### 🏗️ Actual Application Build Commands

Jenkins can execute the build commands of different application technologies.

| Technology | Common Build Commands |
|---|---|
| Java | `mvn clean package`, `gradle build` |
| Node.js | `npm install`, `npm run build` |
| .NET | `dotnet restore`, `dotnet build`, `dotnet test`, `dotnet publish` |
| PHP | `composer install` |
| Python | `pip install -r requirements.txt`, test commands |

**💡 Note:** The required runtime and build tools must be available on the agent executing the pipeline.

---

# 6. 🐙 Jenkinsfile Integration with GitHub

In this section, we will connect Jenkins with GitHub and execute a Jenkinsfile stored in a repository.

## 6.1 📋 Prerequisites

Before configuring the Pipeline job, make sure:

- 🖥️ Jenkins Controller is running.
- 🧰 Git is installed on the Jenkins Controller.
- 🔌 Jenkins Git plugin is installed.
- 🐙 A GitHub repository is available.
- 📄 The repository contains a valid pipeline file.
- 🌐 Jenkins has network access to GitHub.

### 6.1 🧰 Install Git on Amazon Linux 2023

**📍 Location: Jenkins Controller EC2 → SSH Terminal**

**Step 1 – 🔍 Check Git Installation**

```bash
git --version
```

**Step 2 – 📥 Install Git if Missing**

```bash
sudo dnf install git -y
```

**Step 3 – ✅ Verify Installation**

```bash
git --version
```

### ❓ Why is Git Required?

Jenkins uses Git commands to communicate with Git repositories and retrieve source code.

**Example:**

```bash
git ls-remote https://github.com/username/repository.git HEAD
```

This command checks whether the repository is accessible and retrieves its remote reference information.

**⚠️ Important:** Git must be available on the machine performing the Git checkout. For the initial Pipeline script-from-SCM retrieval, this is generally the Jenkins Controller.

---

## 6.2 🐙 GitHub Repository Setup

**📍 Location: GitHub Website**

Follow these steps:

1. 🌐 Open [GitHub](https://github.com/).
2. 📂 Open the repository where your pipeline file is stored.
3. 📁 Navigate to the required directory.
4. ➕ Create a new file or upload an existing pipeline file.
5. 💻 Write the Declarative or Scripted Pipeline code.
6. 📤 Commit and push the changes.

### 📂 Example Repository Structure

```text
MyRepository/
│
├── src/
├── tests/
├── README.md
│
└── Jenkins/
    │
    └── Pipelines/
        │
        └── My-CICD-Pipeline
```

Here, `My-CICD-Pipeline` is an example filename. You can use your own filename and directory structure.

---

## 6.3 🆕 Create a Pipeline Job in Jenkins

**📍 Location: Jenkins Dashboard**

Follow these steps:

1. 🏠 Open Jenkins Dashboard.
2. ➕ Click **New Item** in the left sidebar.
3. ✏️ Enter a job name, for example:

   `GitHub-Jenkinsfile-Pipeline`

4. 🔧 Select **Pipeline** as the job type.
5. ✅ Click **OK**.

The Pipeline job will be created.

---

## 6.4 ⚙️ Configure Pipeline from SCM

**📍 Location: Jenkins Dashboard → GitHub-Jenkinsfile-Pipeline → Configure**

Scroll down to the **Pipeline** section.

### 🔧 Configure the Following Options

| Field | Configuration |
|---|---|
| Definition | Pipeline script from SCM |
| SCM | Git |
| Repository URL | GitHub repository URL |
| Credentials | Select credentials if the repository is private |
| Branch Specifier | `*/main` |
| Script Path | Repository-relative path of your pipeline file |

### 📂 Understanding Script Path

Script Path tells Jenkins where the pipeline file is located inside the GitHub repository.

**Example:**

If the repository contains:

```text
MyRepository/
│
└── Jenkins/
    │
    └── Pipelines/
        │
        └── My-CICD-Pipeline
```

Enter this in Script Path:

```text
Jenkins/Pipelines/My-CICD-Pipeline
```

If your file is located directly in the repository root, enter only its filename.

### 💡 Important Points

- 📝 The filename is not mandatory; `Jenkinsfile` is simply the default convention.
- ✏️ The file can have any meaningful name.
- 📍 The Script Path must match the exact filename and directory structure in GitHub.
- 🚫 Do not include the repository name at the beginning of Script Path.
- ➡️ Use forward slashes `/` between directories.

After configuration, click **Save**.

---

## 6.5 🌐 Public GitHub Repository Configuration

A public repository can be accessed without authentication for Git checkout.

**📍 Location: Jenkins Dashboard → Pipeline Job → Configure → Pipeline**

### 🔧 Configure the Following Fields

| Field | Value |
|---|---|
| Definition | Pipeline script from SCM |
| SCM | Git |
| Repository URL | `https://github.com/username/repository.git` |
| Credentials | `- none -` |
| Branch Specifier | `*/main` |
| Script Path | Exact repository-relative file path |

### 🪜 Configuration Steps

1. 🔽 Select **Pipeline script from SCM**.
2. 🔧 Select **Git** under SCM.
3. 🔗 Enter the public GitHub repository URL.
4. 🔓 Leave Credentials as `- none -`.
5. 🌿 Enter the required branch.
6. 📄 Enter the exact Script Path.
7. 💾 Click **Save**.
8. ▶️ Click **Build Now**.

Jenkins will retrieve the pipeline file from the public repository and execute it.

**💡 Note:** Public repositories do not require GitHub credentials for normal read-only checkout. However, private submodules or other protected resources may require separate authentication.

---

## 6.6 🔐 Private GitHub Repository Configuration

A private repository requires authentication because its source code cannot be accessed anonymously.

For this practical, we will use a GitHub Personal Access Token (PAT).

### 6.6.1 🔑 Generate a GitHub Personal Access Token

**📍 Location: GitHub Website**

**Navigation:**

`GitHub Profile → Settings → Developer settings → Personal access tokens → Fine-grained tokens`

### 🪜 Token Creation Steps

1. ⚙️ Open **Fine-grained tokens**.
2. ➕ Click **Generate new token**.
3. ✏️ Enter a token name, for example `Jenkins-Access`.
4. ⏳ Select the token expiration.
5. 🔒 Under Repository access, select **Only select repositories**.
6. 📂 Select the required private repository.
7. 🛡️ Under Repository permissions, set:

   `Contents → Read-only`

8. 🚀 Generate the token.
9. 📋 Copy and securely store the token.

The Contents read permission is sufficient for reading repository files through Git checkout.

### 6.6.2 🔐 Add GitHub Credentials in Jenkins

**📍 Location: Jenkins Dashboard**

**Navigation:**

`Manage Jenkins → Credentials → System → Global credentials (unrestricted)`

### 🪜 Credential Creation Steps

1. 📂 Open **Global credentials (unrestricted)**.
2. ➕ Click **Add Credentials**.
3. 🔽 Select the following credential type:

   `Username with password`

4. ✏️ Configure the fields:

| Field | Example Value |
|---|---|
| Scope | Global |
| Username | `github-jenkins-token` |
| Password | Paste your GitHub PAT |
| ID | `github-pat` |
| Description | `GitHub Private Repository Access` |

5. 💾 Click **Create**.

### 💡 Understanding Credential Fields

- 👤 **Username:** Can be a custom value; use your GitHub username or a descriptive identifier.
- 🔑 **Password:** Must contain the actual GitHub Personal Access Token.
- 🆔 **ID:** A unique identifier used by Jenkins to reference the credential.
- 📝 **Description:** Explains the purpose of the credential.

The ID can be customized, but the same ID must be used wherever the credential is referenced.

### 6.6.3 ⚙️ Configure Private Repository in Jenkins

**📍 Location: Jenkins Dashboard → Pipeline Job → Configure → Pipeline**

### 🔧 Configure the Following Fields

| Field | Value |
|---|---|
| Definition | Pipeline script from SCM |
| SCM | Git |
| Repository URL | Private GitHub repository URL |
| Credentials | Select `github-pat` |
| Branch Specifier | `*/main` |
| Script Path | Exact repository-relative file path |

💾 Click **Save**.

### 6.6.4 ▶️ Execute Pipeline

1. 🏠 Open the Pipeline job.
2. ▶️ Click **Build Now**.
3. 🔢 Open the generated build number.
4. 📋 Select **Console Output**.
5. ✅ Verify that Jenkins successfully checks out the repository and executes the pipeline.

### ⚖️ Public vs Private Repository

| Feature | Public Repository | Private Repository |
|---|---|---|
| Repository access | Anonymous read access | Authentication required |
| Jenkins credentials | Not required for normal checkout | Required |
| PAT | Not required | One authentication option |
| Git checkout | Without authentication | Authenticated checkout |
| Source visibility | Public | Restricted |

### 🛡️ Security Best Practices

- 🚫 Never hardcode PATs in Jenkinsfiles.
- 🚫 Never commit tokens to GitHub.
- 🔐 Store secrets in Jenkins Credentials.
- 🎯 Give tokens only the permissions they require.
- ⏳ Use an appropriate expiration period.
- 🤫 Never print credentials in console output.

For production environments, GitHub App authentication and SSH deploy keys are also useful alternatives.

---

## 6.7 ✅ Verify Jenkinsfile Execution

**📍 Location: Jenkins Dashboard → Your Pipeline Job**

After saving the configuration, follow these steps:

1. ▶️ Click **Build Now**.
2. 📜 Check the Build History.
3. 🔢 Open the latest build.
4. 📋 Click **Console Output**.
5. 🔍 Verify Git checkout and pipeline execution.
6. 📊 Check the stage results in the Pipeline Stage View.

### 🔄 Expected Execution Flow

```text
Started by user
       |
       v
Checking out Git repository
       |
       v
Reading Pipeline file
       |
       v
Build Stage
       |
       v
Test Stage
       |
       v
Deploy Stage
       |
       v
Pipeline Completed Successfully
```

---

## 6.8 🔄 Test Jenkinsfile Updates

To verify that Jenkins retrieves the latest pipeline definition:

1. 🌐 Open the pipeline file in GitHub.
2. ✏️ Edit one of the stage messages.
3. 📤 Commit and push the changes.
4. 🏠 Open the Jenkins Pipeline job.
5. ▶️ Click **Build Now**.
6. 📋 Check Console Output.
7. ✅ Verify that the updated message appears.

This confirms that Jenkins retrieves the updated pipeline file from the configured repository and branch.

**💡 Note:** This practical uses manual `Build Now` execution. Automatic triggering through GitHub Webhooks will be covered separately in the GitHub Webhook Integration project.

---

## 7. 🐛 Troubleshooting & Debugging

| Error / Problem | Possible Cause | Solution |
|---|---|---|
| `git: command not found` | Git is not installed | Install Git on the machine performing checkout. |
| `Failed to connect to repository` | Git, network, DNS, or repository access issue | Verify Git installation and repository connectivity. |
| Authentication failed | Invalid or expired PAT | Generate a valid token and update Jenkins Credentials. |
| Repository not found | Incorrect URL or insufficient permissions | Verify repository URL and token permissions. |
| Jenkinsfile not found | Incorrect Script Path | Verify the file location and filename in GitHub. |
| Branch not found | Incorrect branch configuration | Verify the branch name and Branch Specifier. |
| No such DSL method | Invalid step or missing plugin | Check Pipeline syntax and required plugins. |
| Agent is offline | Agent connectivity issue | Verify agent status and configuration. |
| Build command not found | Required tool missing on agent | Install and configure the required build tool. |

### 🔎 Useful Jenkins Resources

- 🧩 Pipeline Syntax – Snippet Generator.
- 🏗️ Declarative Directive Generator.
- 📋 Console Output.
- 📊 Pipeline Stage View.
- 🔁 Replay feature, where available and permitted.

---

## 8. 📝 Declarative vs Scripted Jenkinsfile

| Feature | Declarative | Scripted |
|---|---|---|
| Syntax | Structured | Groovy-based |
| Learning curve | Easier | Requires Groovy knowledge |
| Main structure | `pipeline {}` | `node {}` |
| Conditions | `when` | `if-else` |
| Error handling | `post`, `catchError`, etc. | `try-catch-finally` |
| Flexibility | More structured | Greater programming flexibility |
| Common usage | Standard CI/CD pipelines | Custom pipeline logic |

---

## 9. ✅ Day 09 Completion Checklist

- [ ] 📖 Understand Jenkinsfile and Pipeline as Code.
- [ ] 🏗️ Understand Declarative Jenkinsfile structure.
- [ ] ⚙️ Understand Scripted Jenkinsfile structure.
- [ ] 🌍 Understand environment variables and parameters.
- [ ] 🔐 Configure Jenkins Credentials.
- [ ] 🧰 Learn essential Jenkins Pipeline steps.
- [ ] 🏗️ Understand application build commands.
- [ ] 🧰 Install and verify Git on Jenkins Controller.
- [ ] 🆕 Create a Pipeline job in Jenkins.
- [ ] ⚙️ Configure Pipeline script from SCM.
- [ ] 🌐 Connect Jenkins to a public GitHub repository.
- [ ] 🔑 Connect Jenkins to a private GitHub repository using PAT.
- [ ] 📍 Configure branch and Script Path.
- [ ] ▶️ Execute and verify the Jenkinsfile.
- [ ] 🐛 Understand common errors and troubleshooting.

---

## 🎯 Final Learning Outcome

After completing Day 09, you should be able to create Jenkinsfiles, configure Declarative and Scripted Pipelines, manage Jenkins credentials, connect Jenkins with public and private GitHub repositories, and execute pipeline definitions directly from source control.

**Day 09 completes the Jenkins learning phase. The next step is to apply these concepts through practical Jenkins projects.**

---

## 🚀 Next Learning Step

**Jenkins Learning Completed – Moving to Practical Projects**

In the next phase, we will build practical Jenkins projects covering Freestyle jobs, Jenkinsfile pipelines, GitHub integration, GitHub Webhooks, and end-to-end CI/CD.
