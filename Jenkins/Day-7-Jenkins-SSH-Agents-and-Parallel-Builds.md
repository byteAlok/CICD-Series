# 🔐 Day 07 – Jenkins SSH Agents and Parallel Builds

## 📌 1. Introduction

In Day 05, we studied Jenkins Controller-Agent Architecture, and in Day 06, we connected an Agent to the Controller using the Inbound Agent method with WebSocket.

Now, we will configure another Jenkins Agent using the **SSH Launch Method** and execute Pipeline tasks across multiple Agents.

In an SSH-based connection, the Jenkins Controller initiates an SSH connection to the Agent machine and launches the Jenkins Agent process.

### 🎯 What Will We Learn?

- 🔐 SSH Agent Architecture
- 🔑 SSH Authentication using PEM Key
- 🧩 Jenkins SSH Credentials
- 🛡️ Host Key Verification
- 📂 `known_hosts` file
- 🔍 `ssh-keyscan` command
- 🔑 `ssh-keygen -lf` command
- ⚙️ SSH Agent Configuration
- 🔄 Controller-to-Agent SSH Connection
- 🧪 Multiple Agent Execution
- ⚡ Parallel Builds in Jenkins
- 🛠️ Common SSH Troubleshooting

---

## 🌐 2. What is an SSH Agent?

An SSH Agent is a Jenkins Agent that is launched by the Jenkins Controller through an SSH connection.

Unlike an Inbound Agent, where the Agent initiates the connection, an SSH Agent connection is initiated by the Controller.

The Controller uses SSH authentication to connect to the Agent machine and start the Jenkins Agent process.

### 🔄 SSH Agent Architecture

```text
             Jenkins Controller
          +-----------------------+
          |                       |
          |   Jenkins Server      |
          |                       |
          |   SSH Private Key     |
          |                       |
          +-----------------------+
                     |
                     |
                     | SSH Connection
                     | Port 22
                     |
                     v
          +-----------------------+
          |                       |
          |      EC2 Agent        |
          |                       |
          |   SSH Public Key      |
          |                       |
          |   Java Runtime        |
          |                       |
          +-----------------------+
```

### 📌 How Does It Work?

1. The Controller has an SSH private key stored in Jenkins Credentials.
2. The Agent machine has the corresponding public key in its `authorized_keys` file.
3. Jenkins uses the private key to authenticate with the Agent.
4. After successful authentication, the Controller launches the Agent process through SSH.
5. The Agent connects to the Controller and becomes available for executing jobs.

---

## 🔍 3. Inbound Agent vs SSH Agent

We have already configured an Inbound Agent using WebSocket. Now we are configuring an SSH Agent.

| Feature | Inbound Agent | SSH Agent |
|---|---|---|
| Connection Initiator | Agent | Controller |
| Connection Direction | Agent → Controller | Controller → Agent |
| Authentication | Agent Secret | SSH Credentials |
| SSH Server Required | No | Yes |
| Java Required | Yes | Yes |
| Common Connection | WebSocket or inbound TCP | SSH |
| Default SSH Port | Not required for WebSocket | 22 |

### 💡 Important Difference

**Inbound Agent:**

The Agent connects to the Controller.

**SSH Agent:**

The Controller connects to the Agent using SSH and launches the Agent process.

---

## 🏗️ 4. Our AWS EC2 Infrastructure

For this practical, we will use the following infrastructure:

| EC2 Instance | Role | Connection Method |
|---|---|---|
| EC2-Controller | Jenkins Controller | Jenkins Server |
| EC2-Agent-1 | Inbound Agent | WebSocket |
| EC2-Agent-2 | Inbound Agent | WebSocket |
| EC2-Agent-3 | SSH Agent | SSH |

### 📌 Agent Configuration

| Agent | Label | Connection |
|---|---|---|
| Slave1 | `slave1` | WebSocket |
| Slave2 | `slave2` | WebSocket |
| Slave3 | `slave3` | SSH |

Our third Agent will be configured using SSH.

### 🔐 Network Requirements

The Controller must be able to connect to Agent 3 through SSH.

```text
Controller EC2  ----------->  Agent 3 EC2
                  TCP 22
```

Configure the Agent Security Group to allow SSH traffic from the Controller's private IP or Security Group.

**Security Note:** Avoid allowing SSH access from `0.0.0.0/0`. Restrict access to the required source.

---

## 🔑 5. Understanding SSH Key Authentication

SSH supports key-based authentication using a key pair.

An SSH key pair consists of:

1. 🔐 Private Key
2. 🔓 Public Key

### 🔐 Private Key

The private key is kept securely on the client machine that initiates the SSH connection.

In our Jenkins setup, Jenkins Controller uses the private key to authenticate with the Agent.

Example:

```text
jenkins.pem
```

### 🔓 Public Key

The public key is installed on the Agent machine inside:

```bash
~/.ssh/authorized_keys
```

It allows the corresponding private key holder to authenticate, subject to SSH configuration and permissions.

### 🔄 SSH Authentication Flow

```text
       Jenkins Controller
              |
              |
       Private Key (.pem)
              |
              |
              v
        SSH Authentication
              |
              |
              v
          Agent EC2
              |
              |
       authorized_keys
              |
              |
              v
       Authentication
          Successful
              |
              v
       SSH Connection
       Established
```

### 📌 Important Points

- Private key must remain confidential.
- Public key can be installed on the Agent.
- Both keys belong to the same key pair.
- The private key must correspond to an authorized public key.
- Jenkins should store the private key securely in Credentials.

---

## 🧩 6. Configure SSH Credentials in Jenkins

Jenkins needs the private key to authenticate with the SSH Agent.

We will store the PEM private key in Jenkins Credentials.

### ⚙️ Step 1: Open Credentials

Navigate to:

```text
Jenkins Dashboard
       |
       v
Manage Jenkins
       |
       v
Credentials
       |
       v
System
       |
       v
Global credentials
       |
       v
Add Credentials
```

### ⚙️ Step 2: Configure Credential

Select the following configuration:

| Field | Value |
|---|---|
| Kind | SSH Username with private key |
| Scope | Global |
| ID | `slave3-ssh-key` |
| Username | `ec2-user` |
| Private Key | Enter directly |
| Passphrase | Leave empty if the key has no passphrase |

### 🔍 Understanding Important Fields

**Kind: SSH Username with private key**

This credential type stores an SSH username and its corresponding private key.

**Username:**

The Linux username used to connect to the Agent.

For Amazon Linux 2023, the default EC2 username is generally:

```text
ec2-user
```

**Private Key:**

Select:

```text
Enter directly
```

Paste the contents of your PEM private key.

A PEM key commonly looks like:

```text
-----BEGIN RSA PRIVATE KEY-----
...
-----END RSA PRIVATE KEY-----
```

The actual key format may differ depending on how the key pair was generated.

**ID:**

The ID uniquely identifies this credential inside Jenkins.

Example:

```text
slave3-ssh-key
```

Click **Create** to save the credential.

### 🔐 Why Do We Store PEM in Jenkins Credentials?

Instead of writing a private key directly inside Pipeline code, Jenkins securely manages it through its Credentials system.

This helps:

- Protect private keys.
- Avoid hardcoding secrets in Pipeline files.
- Reuse credentials across Jenkins jobs and Nodes.
- Control access through Jenkins permissions.

**Important:** Never upload your PEM private key to GitHub or expose it in logs.

---

## 🛡️ 7. Understanding known_hosts

When Jenkins connects to an SSH Agent for the first time, it needs to verify the identity of the remote SSH server.

For this purpose, SSH uses a file called:

```text
known_hosts
```

This file stores the public host keys of remote SSH servers that the client has trusted.

### 🔍 Why Is known_hosts Required?

Suppose the Jenkins Controller connects to an Agent EC2.

The Controller needs to verify that the remote server is actually the expected Agent and not an impersonating machine.

SSH uses host key verification to help prevent Man-in-the-Middle (MITM) attacks.

### 📂 Default Location

For the Jenkins Linux user, the file is generally located at:

```bash
/var/lib/jenkins/.ssh/known_hosts
```

### 📌 Important Difference

Do not confuse these two files:

| File | Purpose |
|---|---|
| `authorized_keys` | Stores public keys authorized to log in to a user account |
| `known_hosts` | Stores trusted SSH server host keys |

**Remember:**

- `authorized_keys` verifies client authentication.
- `known_hosts` verifies the remote server's identity.

---

## 🔍 8. What is ssh-keyscan?

`ssh-keyscan` is a Linux command used to retrieve SSH host public keys from a remote server.

It is commonly used to collect the host key that can be added to the `known_hosts` file.

### 📌 Command

Run this command on the Jenkins Controller:

```bash
sudo ssh-keyscan -H AGENT_PRIVATE_IP
```

Replace `AGENT_PRIVATE_IP` with the actual private IP address of Agent 3.

### 🔍 Understanding the Command

| Part | Meaning |
|---|---|
| `ssh-keyscan` | Retrieves SSH host keys |
| `-H` | Hashes hostnames and IP addresses in the output |
| `AGENT_PRIVATE_IP` | Address of the SSH Agent |

### 📌 Save the Host Key

To save the retrieved key directly into the Jenkins user's `known_hosts` file:

```bash
sudo -u jenkins mkdir -p /var/lib/jenkins/.ssh
```

```bash
sudo ssh-keyscan -H AGENT_PRIVATE_IP >> /tmp/agent-host-key
```

Append it to the Jenkins user's file:

```bash
sudo sh -c 'cat /tmp/agent-host-key >> /var/lib/jenkins/.ssh/known_hosts'
```

Set ownership and permissions:

```bash
sudo chown -R jenkins:jenkins /var/lib/jenkins/.ssh
```

```bash
sudo chmod 700 /var/lib/jenkins/.ssh
```

```bash
sudo chmod 600 /var/lib/jenkins/.ssh/known_hosts
```

### ⚠️ Security Note

`ssh-keyscan` retrieves the server's presented host key, but it does not independently prove that the key belongs to the intended server.

For secure production configuration, verify the fingerprint through a trusted channel before trusting and adding the key to `known_hosts`.

---

## 🔑 9. What is ssh-keygen -lf?

`ssh-keygen` is an SSH key management utility.

The `-lf` option is used to display the fingerprint of a public key file.

### 📌 Command

```bash
ssh-keygen -lf /var/lib/jenkins/.ssh/known_hosts
```

### 🔍 Understanding the Command

| Option | Meaning |
|---|---|
| `-l` | Displays the fingerprint |
| `-f` | Specifies the file containing the key |

The command displays the fingerprint information of the host key entries in the specified file.

### 💡 Why Do We Use It?

We use fingerprints to compare the host key obtained from the SSH server against a fingerprint obtained through a trusted source.

This helps verify the identity of the SSH server.

**Important:** Merely generating a fingerprint does not verify the server. The fingerprint must be compared with a trusted fingerprint.

---

## 🔐 10. SSH Host Key Verification in Jenkins

When Jenkins connects to the Agent using SSH, it must verify the remote server's host key.

Jenkins SSH Agent configuration supports different Host Key Verification Strategies.

Common strategies include:

| Strategy | Purpose |
|---|---|
| Known hosts file | Verifies the server using trusted entries in `known_hosts` |
| Manually provided keys | Uses administrator-provided trusted host keys |
| Accept first connection | Trusts the key on first connection and remembers it |
| Non verifying verification strategy | Skips host key verification |

### 🛡️ Recommended Approach

For production environments, use a trusted host key verification strategy.

The `known_hosts` file approach is useful when the Controller's SSH client environment is correctly configured.

Avoid disabling host key verification in production because it weakens SSH security.

---

## 🖥️ 11. Create SSH Agent Node in Jenkins

Now we will create the third Agent Node.

Navigate to:

```text
Jenkins Dashboard
       |
       v
Manage Jenkins
       |
       v
Nodes
       |
       v
New Node
```

Enter:

```text
Node Name: Slave3
Node Type: Permanent Agent
```

Click **Create**.

### ⚙️ Configure Agent

| Field | Example Value |
|---|---|
| Name | Slave3 |
| Description | SSH Agent |
| Number of Executors | 1 |
| Remote Root Directory | `/home/ec2-user/jenkins-agent` |
| Labels | `slave3` |
| Usage | Only build jobs with label expressions matching this node |
| Launch Method | Launch agents via SSH |

### 🔍 SSH Launch Configuration

After selecting **Launch agents via SSH**, configure:

| Field | Value |
|---|---|
| Host | Agent 3 Private IP |
| Credentials | `slave3-ssh-key` |
| Host Key Verification Strategy | Known hosts file Verification Strategy |

The SSH Host Key Verification Strategy must match the trusted host key configuration we prepared earlier.

Save the configuration.

### 📌 What Happens Now?

1. Jenkins tries to connect to Agent 3.
2. It verifies the Agent's host key.
3. It authenticates using the configured SSH credentials.
4. It checks the remote environment and Java availability.
5. It launches the Jenkins Agent process.
6. Agent 3 becomes Online if the launch succeeds.

---

## ☕ 12. Java Requirement on SSH Agent

The SSH Agent also needs a compatible Java runtime because Jenkins launches the Agent process using Java.

Check Java on Agent 3:

```bash
java -version
```

If Java is missing, install the compatible runtime.

For Amazon Linux 2023 with Amazon Corretto 25:

```bash
sudo dnf install java-25-amazon-corretto-headless -y
```

Verify:

```bash
java -version
```

### 📌 Why Is Java Required?

The Controller uses SSH to connect to the Agent and launch the Java-based Agent process.

Without a compatible Java runtime, the Agent process cannot start.

---

## 🔄 13. Verify SSH Agent Connection

After saving the Node configuration, Jenkins attempts to launch the Agent over SSH.

Open:

```text
Manage Jenkins
       |
       v
Nodes
       |
       v
Slave3
```

If the setup is successful, the Node should show:

```text
Slave3: Online
```

### 🔍 Verify Using Logs

Open the Agent Node page and check its log.

The logs can help identify:

- SSH authentication success or failure.
- Host key verification errors.
- Java availability.
- Agent launch errors.
- Connection status.

### 📌 Important

Unlike the Inbound Agent setup, we do not need to manually run `agent.jar` using `nohup` for this SSH launch configuration.

Jenkins handles the SSH connection and Agent process launch.

---

## ⚡ 14. What are Parallel Builds?

Parallel execution allows multiple Pipeline branches to execute at the same time.

Instead of executing every task sequentially, Jenkins can execute independent tasks simultaneously.

### 🔍 Sequential Execution

Suppose we have three tasks:

- Build Application
- Run Tests
- Deploy Application

In sequential execution:

```text
Build
  |
  v
Test
  |
  v
Deploy
```

Each stage starts after the previous stage finishes.

### ⚡ Parallel Execution

Suppose we have three independent tasks:

- Build Java Application
- Build Node.js Application
- Run Docker Checks

These tasks can execute simultaneously on different Agents.

```text
             Pipeline
                 |
          +------+------+------+
          |      |      |      |
          v      v      v
        Agent1 Agent2 Agent3
          |      |      |
        Java   Node   Docker
        Build  Build  Checks
          |      |      |
          +------+------+------+
                 |
                 v
             Completion
```

### 📌 Benefits of Parallel Builds

- ⚡ Faster Pipeline execution.
- 🧪 Simultaneous testing.
- 💻 Better utilization of multiple Agents.
- 🔄 Independent task execution.
- 📈 Improved CI/CD efficiency.

**Important:** Parallel execution is suitable when tasks are independent or their dependencies are handled correctly.

---

## 🧪 15. Parallel Pipeline Using Multiple Agents

Now we will execute three Pipeline branches simultaneously across three Agents.

### 📌 Complete Pipeline Code

```groovy
pipeline {
    agent none

    stages {
        stage('Parallel Execution') {
            parallel {

                stage('Agent 1 - Java') {
                    agent {
                        label 'slave1'
                    }

                    steps {
                        echo 'Running on Agent 1'
                        sh 'hostname'
                        sh 'java -version'
                    }
                }

                stage('Agent 2 - WebSocket') {
                    agent {
                        label 'slave2'
                    }

                    steps {
                        echo 'Running on Agent 2'
                        sh 'hostname'
                        sh 'java -version'
                    }
                }

                stage('Agent 3 - SSH') {
                    agent {
                        label 'slave3'
                    }

                    steps {
                        echo 'Running on Agent 3'
                        sh 'hostname'
                        sh 'java -version'
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'All Parallel Tasks Completed Successfully'
        }

        failure {
            echo 'One or More Parallel Tasks Failed'
        }

        always {
            echo 'Pipeline Execution Finished'
        }
    }
}
```

### 🔍 Code Explanation

**1. `agent none`**

Jenkins does not allocate one global Agent for the entire Pipeline.

Each parallel branch selects its own Agent.

**2. `parallel`**

The `parallel` directive allows multiple stages to execute simultaneously.

**3. `agent { label 'slave1' }`**

Selects the Agent having the `slave1` label.

**4. `agent { label 'slave2' }`**

Selects the Agent having the `slave2` label.

**5. `agent { label 'slave3' }`**

Selects the Agent having the `slave3` label.

**6. `hostname`**

Displays the hostname of the machine executing the Pipeline branch.

**7. `java -version`**

Displays the Java runtime installed on the selected Agent.

**8. `post`**

Displays the final Pipeline execution result.

### 📊 Expected Execution

| Parallel Branch | Selected Agent | Task |
|---|---|---|
| Branch 1 | Slave1 | Java information |
| Branch 2 | Slave2 | Java information |
| Branch 3 | Slave3 | Java information |

All three branches can execute simultaneously if all Agents are online, eligible, and have available Executors.

### 📌 Important Point

If one Agent is offline or has no available Executor, its branch may wait in the queue.

Parallel execution does not guarantee that every branch starts at exactly the same time.

---

## 🧠 16. How Jenkins Executes Parallel Branches

Let's understand the complete execution flow.

1. Developer triggers the Pipeline.
2. Controller reads the Pipeline configuration.
3. Jenkins enters the Parallel Execution stage.
4. Jenkins evaluates the Agent label for each branch.
5. Jenkins identifies eligible Nodes.
6. Each branch requests an available Executor.
7. Branches execute on their selected Agents.
8. Each Agent reports its execution status and logs.
9. Jenkins collects the branch results.
10. The Pipeline completes according to the execution results.

### 🔄 Execution Flow

```text
              Pipeline Started
                     |
                     v
             Parallel Stage
                     |
          +----------+----------+
          |          |          |
          v          v          v
       Slave1     Slave2     Slave3
       WebSocket  WebSocket  SSH
          |          |          |
          v          v          v
        Build      Test      Deploy
          |          |          |
          +----------+----------+
                     |
                     v
              Collect Results
                     |
                     v
              Pipeline Finished
```

---

## 🛠️ 17. Common SSH Agent Problems

| Problem | Possible Reason | Solution |
|---|---|---|
| Permission denied (publickey) | Incorrect private key or username | Verify PEM key and SSH username |
| Host key verification failed | Missing or incorrect `known_hosts` entry | Verify and add the correct host key |
| Connection timed out | Security Group or network issue | Check TCP port 22 connectivity |
| Connection refused | SSH service is unavailable | Check SSH service on Agent |
| Java not found | Java is not installed or not in PATH | Install compatible Java |
| Agent launch failed | Remote directory or permission issue | Check remote root directory |
| Incorrect credentials | Wrong Jenkins credential selected | Verify credential ID and contents |
| Agent remains offline | SSH launch or Java error | Inspect the Agent log |

### 🔍 Useful Troubleshooting Commands

Check SSH service on Agent:

```bash
sudo systemctl status sshd
```

Check Java:

```bash
java -version
```

Check SSH connectivity from Controller:

```bash
ssh -i /path/to/key.pem ec2-user@AGENT_PRIVATE_IP
```

Check host key entries:

```bash
sudo cat /var/lib/jenkins/.ssh/known_hosts
```

Display host key fingerprint:

```bash
ssh-keygen -lf /var/lib/jenkins/.ssh/known_hosts
```

Check Jenkins service:

```bash
sudo systemctl status jenkins
```

---

## 🔐 18. SSH Security Best Practices

- 🔒 Never expose private keys publicly.
- 🔑 Store PEM keys in Jenkins Credentials.
- 🛡️ Verify SSH host keys using a trusted source.
- 🌐 Restrict SSH access through Security Groups.
- 🖥️ Prefer private IP communication between EC2 instances.
- 👤 Use appropriate Linux users with limited permissions.
- 🔄 Keep SSH and Java updated.
- 📂 Protect the `known_hosts` file.
- 🚫 Avoid disabling host key verification in production.
- 📝 Regularly monitor SSH and Jenkins Agent logs.

---

## 📚 19. Important Commands – Quick Revision

| Command | Purpose |
|---|---|
| `ssh -i key.pem user@IP` | Connect to Agent using SSH |
| `ssh-keyscan -H IP` | Retrieve SSH host keys |
| `ssh-keygen -lf FILE` | Display key fingerprint |
| `sudo systemctl status sshd` | Check SSH service |
| `java -version` | Check Java version |
| `ps -ef \| grep java` | Check Java processes |
| `sudo cat /path/to/known_hosts` | View trusted host keys |
| `sudo chown` | Change file ownership |
| `sudo chmod` | Change file permissions |

---

## 📝 20. Quick Revision – Day 07

| Question | Answer |
|---|---|
| Who initiates an SSH Agent connection? | Controller |
| Which port is commonly used for SSH? | 22 |
| What is a PEM file? | A private key file format commonly used for EC2 SSH access |
| Where is the private key stored in Jenkins? | Jenkins Credentials |
| What is `authorized_keys`? | File containing authorized client public keys |
| What is `known_hosts`? | File containing trusted SSH server host keys |
| What does `ssh-keyscan` do? | Retrieves SSH host keys |
| What does `ssh-keygen -lf` do? | Displays key fingerprints |
| What is Parallel Execution? | Executing independent Pipeline branches simultaneously |
| Which Pipeline directive enables parallel branches? | `parallel` |
| Which directive avoids a global Agent allocation? | `agent none` |

---

## 🎯 21. Jenkins Learning Series – Completion

After completing Day 07, we have covered the following Jenkins topics:

| Day | Topic | Status |
|---|---|---|
| Day 01 | Installation and Configuration | ✅ |
| Day 02 | Declarative Pipeline and Post Actions | ✅ |
| Day 03 | Input and Approval | ✅ |
| Day 04 | Timeout and Error Handling | ✅ |
| Day 05 | Controller-Agent Architecture | ✅ |
| Day 06 | Inbound Agent Setup (WebSocket) | ✅ |
| Day 07 | SSH Agents and Parallel Builds | ✅ |

### 🏆 Final Learning Outcomes

We now understand:

- 🏗️ Jenkins Installation and Configuration
- 📜 Declarative Pipeline
- 🔄 Pipeline Execution Flow
- 📢 Post Actions
- 👤 Manual Approval
- ⏱️ Timeout and Error Handling
- 🧠 Controller-Agent Architecture
- 🌐 Inbound Agent Setup
- 🔐 SSH Agent Configuration
- 🔑 PEM Key Authentication
- 🛡️ Host Key Verification
- 📂 `known_hosts`
- ⚡ Parallel Pipeline Execution
- ☁️ Jenkins Distributed Architecture on AWS EC2
