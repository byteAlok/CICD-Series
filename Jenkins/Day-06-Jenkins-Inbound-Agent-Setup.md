# 🌐 Day 06 – Jenkins Inbound Agent Setup (WebSocket)

## 📌 1. Introduction

In Day 05, we studied Jenkins Controller-Agent Architecture and understood how Jenkins distributes workloads across multiple machines.

Now, we will practically connect an EC2 instance to our Jenkins Controller using the **Inbound Agent method with WebSocket**.

In this method, the Agent machine initiates a connection to the Jenkins Controller.

The Agent uses a Java-based process called `agent.jar` to establish and maintain the connection.

### 🎯 What Will We Learn?

- 🌐 What is an Inbound Agent?
- 🔄 How Inbound Agent communication works
- 🖥️ Creating an Agent Node in Jenkins
- ☕ Installing Java on the Agent EC2
- 📦 Downloading `agent.jar`
- 🔐 Understanding Agent Secret
- ⚙️ Connecting Agent using WebSocket
- 🚀 Running Agent in the background
- 📝 Managing Agent logs
- 🔍 Verifying Agent connection
- 🛠️ Troubleshooting common issues

---

## 🌐 2. What is an Inbound Agent?

An Inbound Agent is a Jenkins Agent that initiates the connection to the Jenkins Controller.

In this connection method:

1. The Controller creates and configures an Agent Node.
2. The Agent machine downloads `agent.jar` from the Controller.
3. The Agent uses its name and secret to authenticate.
4. The Agent initiates a connection to the Controller.
5. After successful authentication, the Agent becomes available for executing Jenkins jobs.

### 🔄 Connection Architecture

```text
          Jenkins Controller
        +----------------------+
        |                      |
        |   Jenkins Dashboard  |
        |                      |
        |   Agent Configuration|
        |                      |
        |   Agent Secret       |
        +----------------------+
                   ^
                   |
                   | WebSocket Connection
                   |
                   |
        +----------------------+
        |                      |
        |     EC2 Agent        |
        |                      |
        |     Java Runtime     |
        |                      |
        |     agent.jar        |
        |                      |
        +----------------------+
```

### 📌 Important Point

In an Inbound Agent connection, the Agent initiates the connection instead of the Controller.

This is useful when the Agent is behind a firewall or NAT and the Controller cannot directly initiate a connection to it.

---

## 🔌 3. What is WebSocket?

WebSocket is a communication protocol that provides a persistent, two-way communication channel between a client and a server.

In Jenkins, WebSocket can be used for communication between the Controller and an Inbound Agent.

### 🔍 How WebSocket Works

1. The Agent initiates an HTTP request to the Jenkins Controller.
2. The request establishes a WebSocket connection through an HTTP Upgrade handshake.
3. After successful connection, both sides can exchange data over the established connection.
4. The connection remains open while the Agent is connected.

### 📊 WebSocket Communication

```text
+-----------------------+
|   Jenkins Controller  |
|                       |
|     HTTP / HTTPS      |
+-----------------------+
            ^
            |
            | WebSocket
            | Persistent Connection
            |
+-----------------------+
|       EC2 Agent       |
|                       |
|       agent.jar       |
+-----------------------+
```

### 🔢 Which Port Is Required?

By default, Jenkins Web UI commonly uses:

| Port | Purpose |
|---|---|
| 8080 | Jenkins HTTP access |
| 443 | HTTPS access when configured |
| 50000 | Traditional inbound TCP agent port |

**Important:**

When we use WebSocket mode, the Agent communicates through the Jenkins HTTP/HTTPS endpoint.

Therefore, a separate inbound TCP agent port such as `50000` is not required for this WebSocket connection.

For our practical setup, we will use port `8080`.

---

## 🏗️ 4. Our AWS EC2 Infrastructure

We will use two EC2 instances.

| EC2 Instance | Role | Purpose |
|---|---|---|
| EC2-Controller | Jenkins Controller | Manages Jenkins |
| EC2-Agent-1 | Inbound Agent | Executes assigned tasks |

### 📌 Infrastructure Details

**Controller EC2:**
- Jenkins installed
- Jenkins service running
- Port 8080 accessible
- Jenkins Dashboard available

**Agent EC2:**
- Amazon Linux 2023
- Java 25 installed
- `agent.jar` downloaded
- Inbound connection configured

### 🔐 Network Requirements

The Agent must be able to reach the Controller.

For our example:

```text
Agent EC2  ------------->  Controller EC2
             TCP 8080
```

Configure the Controller Security Group to allow TCP `8080` from the Agent's IP address or Security Group, as appropriate.

If HTTPS is configured, use the HTTPS endpoint and allow the corresponding port, commonly `443`.

**Security Note:** Restrict access to the required source instead of opening Jenkins to the entire internet.

---

## 🖥️ 5. Create an Agent Node in Jenkins

First, open the Jenkins Dashboard.

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

### ⚙️ Step 1: Create Node

Enter the following details:

| Field | Value |
|---|---|
| Node Name | Slave1 |
| Node Type | Permanent Agent |

Click **Create**.

### ⚙️ Step 2: Configure Agent

Configure the following fields:

| Configuration | Example Value | Purpose |
|---|---|---|
| Name | Slave1 | Unique Agent name |
| Description | First Inbound Agent | Agent description |
| Number of Executors | 1 | One execution slot |
| Remote Root Directory | `/home/ec2-user/jenkins-agent` | Agent working directory |
| Labels | `slave1` | Agent identification |
| Usage | Only build jobs with label expressions matching this node | Restricts scheduling |
| Launch Method | Launch agent by connecting it to the controller | Inbound connection |

### 🔍 Understanding Important Fields

**1. Number of Executors**

Defines how many build tasks can execute simultaneously on this Agent.

Example:

```text
Executors = 1
```

The Agent can execute one build task at a time.

**2. Remote Root Directory**

This is the directory on the Agent machine where Jenkins stores Agent-related data and workspaces.

Example:

```bash
/home/ec2-user/jenkins-agent
```

**3. Labels**

Labels help Jenkins identify and select the Agent.

Example:

```text
slave1
```

We can use this label in a Declarative Pipeline:

```groovy
pipeline {
    agent {
        label 'slave1'
    }

    stages {
        stage('Build') {
            steps {
                echo 'Running on Slave1'
            }
        }
    }
}
```

**4. Usage**

The usage setting determines whether Jenkins can use this Node for any job or only jobs matching its label expressions.

**5. Launch Method**

Select:

```text
Launch agent by connecting it to the controller
```

This configures the Node for an inbound connection.

Save the configuration.

---

## ☕ 6. Install Java on Agent EC2

Jenkins Agent requires a compatible Java runtime to execute `agent.jar`.

Our Agent EC2 uses Java 25.

Connect to the Agent EC2 and check Java:

```bash
java -version
```

If Java is not installed, install the required Java runtime.

For Amazon Linux 2023 with Amazon Corretto 25:

```bash
sudo dnf install java-25-amazon-corretto-headless -y
```

Verify the installation:

```bash
java -version
```

Example output:

```text
openjdk version "25"
OpenJDK Runtime Environment
OpenJDK 64-Bit Server VM
```

### 📌 Why Is Java Required?

`agent.jar` is a Java application.

Therefore, the Agent machine needs a compatible Java runtime to execute it.

**Important:** The Java version must be supported by the Jenkins version and Agent implementation being used. Our practical environment uses Java 25.

---

## 📦 7. Download agent.jar (Optional)

`agent.jar` is a Java-based executable file provided by Jenkins to run the Jenkins Agent process.

It is responsible for establishing communication between the Agent machine and the Jenkins Controller.

When we configure an Inbound Agent, the Agent machine needs `agent.jar` to initiate the connection with the Controller. It uses the Controller URL, Agent name, and authentication secret to establish the connection.

### 💡 Why Is Manual Download Optional?

Jenkins provides `agent.jar` through the Controller. We can download it manually using `wget` or `curl`, or use the Agent launch instructions provided by Jenkins.

In our practical, we followed the launch instructions provided by Jenkins, so we did not perform a separate manual download step.

**Important:** `agent.jar` is required to run the Agent process, but manually downloading it using `wget` or `curl` is optional.

### 🔍 Download Command

First, move to the Agent working directory:

```bash
mkdir -p ~/jenkins-agent

cd ~/jenkins-agent
```

Download `agent.jar`:

```bash
wget http://CONTROLLER_PUBLIC_IP:8080/jnlpJars/agent.jar
```

Replace `CONTROLLER_PUBLIC_IP` with the actual public IP or reachable hostname of the Controller.

### 📌 Understanding the URL

```text
http://CONTROLLER_PUBLIC_IP:8080/jnlpJars/agent.jar
```

| Part | Meaning |
|---|---|
| `http://` | HTTP protocol |
| `CONTROLLER_PUBLIC_IP` | Jenkins Controller address |
| `8080` | Jenkins port |
| `/jnlpJars/` | Jenkins Agent JAR download path |
| `agent.jar` | Agent Java application |

Verify the downloaded file:

```bash
ls -lh agent.jar
```

### 💡 Alternative: Using curl

```bash
curl -O http://CONTROLLER_PUBLIC_IP:8080/jnlpJars/agent.jar
```

Both `wget` and `curl` can download the Agent JAR.

**Security Note:** For production, prefer HTTPS and verify that the downloaded file comes from your trusted Jenkins Controller.

---

## 🔐 8. Understanding Agent Secret

When we create an inbound Agent Node, Jenkins generates a unique secret for that Agent.

This secret is used to authenticate the Agent when it connects to the Controller.

### 📌 Where Can We Find It?

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
Slave1
       |
       v
Agent Launch Instructions
```

Jenkins provides the Agent launch command and its associated secret.

The exact displayed command depends on the Jenkins version and Agent configuration.

### 🔍 Why Is Secret Required?

Suppose an unknown machine tries to connect to our Jenkins Controller.

Jenkins needs to verify whether that machine is authorized to connect as the configured Agent.

The Agent secret helps authenticate the connection.

### ⚠️ Important Security Points

- Every Agent has its own secret.
- Do not share the secret publicly.
- Do not commit the secret to GitHub.
- Do not expose the secret in screenshots or logs.
- Use the command provided by Jenkins for the configured Node.

---

## 🚀 9. Connect Agent Using WebSocket

Now we will connect our EC2 Agent to the Jenkins Controller.

Jenkins provides the required launch command in the Agent configuration page.

The general command structure is:

```bash
java -jar agent.jar \
  -url http://CONTROLLER_PUBLIC_IP:8080/ \
  -secret 'AGENT_SECRET' \
  -name 'Slave1' \
  -webSocket \
  -workDir '/home/ec2-user/jenkins-agent'
```

Replace:

- `CONTROLLER_PUBLIC_IP` with the Controller's reachable IP or hostname.
- `AGENT_SECRET` with the actual secret generated by Jenkins.
- `Slave1` with the exact Agent Node name.

### 🔍 Understanding Every Argument

| Argument | Meaning |
|---|---|
| `java -jar agent.jar` | Executes the Agent JAR using Java |
| `-url` | Specifies the Jenkins Controller URL |
| `-secret` | Provides the Agent authentication secret |
| `-name` | Specifies the Agent Node name |
| `-webSocket` | Uses WebSocket communication |
| `-workDir` | Specifies the Agent working directory |

### 📌 Why Do We Use `-webSocket`?

This option tells the Agent to use WebSocket communication through the Jenkins HTTP/HTTPS endpoint.

Without this option, the Agent may use the traditional inbound TCP connection method, depending on its launch configuration.

### ⚠️ Important

The command above is a general example. Prefer the exact command displayed by Jenkins for your Agent Node.

Do not replace the secret with a guessed value.

---

## 🖥️ 10. Run Agent in Foreground

We can initially start the Agent in the foreground.

Example:

```bash
java -jar agent.jar \
  -url http://CONTROLLER_PUBLIC_IP:8080/ \
  -secret 'AGENT_SECRET' \
  -name 'Slave1' \
  -webSocket \
  -workDir '/home/ec2-user/jenkins-agent'
```

### 🔍 What Happens?

1. Java starts `agent.jar`.
2. Agent reads the Controller URL.
3. Agent authenticates using its secret.
4. Agent establishes a WebSocket connection.
5. Jenkins Controller recognizes the Agent.
6. The Agent becomes online.

### ⚠️ Foreground Execution

When the command runs in the foreground:

- The terminal remains occupied.
- Logs appear directly in the terminal.
- Pressing `Ctrl + C` stops the Agent process.

Foreground execution is useful for initial testing and troubleshooting.

---

## 🚀 11. Run Agent in Background Using nohup

We generally want the Agent to keep running even after closing the terminal session.

For this, we can use `nohup` and background execution.

### 📌 Command

```bash
nohup java -jar agent.jar \
  -url http://CONTROLLER_PUBLIC_IP:8080/ \
  -secret 'AGENT_SECRET' \
  -name 'Slave1' \
  -webSocket \
  -workDir '/home/ec2-user/jenkins-agent' \
  > agent.log 2>&1 &
```

### 🔍 Understanding the Command

| Part | Meaning |
|---|---|
| `nohup` | Allows the process to continue after terminal logout |
| `java -jar agent.jar` | Starts the Jenkins Agent |
| `> agent.log` | Redirects standard output to `agent.log` |
| `2>&1` | Redirects standard error to the same log file |
| `&` | Runs the process in the background |

### 💡 Why Do We Need nohup?

Without `nohup`, a process started from an SSH session may receive a hangup signal when the session closes.

`nohup` helps the process continue running after logout.

### 💡 Why Do We Need &?

The `&` symbol runs the command in the background.

The terminal becomes available for executing other commands.

**Remember:**

- `nohup` = Continue after terminal logout.
- `&` = Run in background.
- `> agent.log` = Save standard output.
- `2>&1` = Save standard error in the same log file.

---

## 📝 12. Check Agent Logs

We redirected Agent output into:

```text
agent.log
```

To view the complete log:

```bash
cat agent.log
```

To view the latest logs:

```bash
tail -n 50 agent.log
```

To continuously monitor logs:

```bash
tail -f agent.log
```

Press:

```text
Ctrl + C
```

to stop viewing the logs.

**Important:** `Ctrl + C` in the `tail -f` terminal stops log viewing, not the background Agent process.

---

## 🔍 13. Verify Agent Process

We can verify whether the Agent process is running.

### Check Process

```bash
ps -ef | grep agent.jar
```

This displays processes containing `agent.jar` in their command line.

### Check Java Processes

```bash
ps -ef | grep java
```

### Check Working Directory

```bash
pwd
```

### Check Downloaded Files

```bash
ls -lh
```

If the Agent process is running, it should appear in the process list.

**Note:** The `grep` command can also display its own process. Look for the actual Java Agent process.

---

## ✅ 14. Verify Agent Status in Jenkins Dashboard

After starting the Agent, return to the Jenkins Dashboard.

Navigate to:

```text
Manage Jenkins
       |
       v
Nodes
       |
       v
Slave1
```

If the connection is successful:

- Agent status becomes Online.
- Jenkins displays the Agent's connection information.
- Its Executor becomes available for eligible jobs.

### 📊 Expected Result

```text
+-------------------------------+
|       Jenkins Nodes           |
+-------------------------------+
|                               |
|  Built-In Node    Online      |
|                               |
|  Slave1           Online      |
|                               |
+-------------------------------+
```

<badge color="success">Expected Status: Online</badge>

---

## 🧪 15. Test the Agent Using a Jenkins Pipeline

Now we will verify that the Pipeline actually executes on the Agent.

Create a new Pipeline and use the following code:

```groovy
pipeline {
    agent {
        label 'slave1'
    }

    stages {
        stage('Check Agent') {
            steps {
                echo 'Checking Jenkins Agent'

                sh 'hostname'

                sh 'whoami'

                sh 'java -version'
            }
        }

        stage('Build') {
            steps {
                echo 'Build Executed Successfully on Agent'
            }
        }
    }

    post {
        success {
            echo 'Agent Pipeline Completed Successfully'
        }

        failure {
            echo 'Agent Pipeline Failed'
        }

        always {
            echo 'Pipeline Execution Finished'
        }
    }
}
```

### 🔍 Code Explanation

**1. `agent { label 'slave1' }`**

Jenkins selects a Node with the `slave1` label.

**2. `sh 'hostname'`**

Displays the hostname of the machine executing the Pipeline.

**3. `sh 'whoami'`**

Displays the Linux user executing the commands.

**4. `sh 'java -version'`**

Displays the Java version installed on the execution machine.

**5. `post`**

Displays the final Pipeline execution status.

### 📌 Expected Result

The hostname should match the Agent EC2 hostname, not the Controller hostname.

This confirms that the Pipeline executed on the Agent.

---

## 🛠️ 16. Common Inbound Agent Problems

| Problem | Possible Reason | Solution |
|---|---|---|
| Java command not found | Java is not installed | Install compatible Java |
| Connection refused | Controller port is unreachable | Check Jenkins service and Security Group |
| Connection timeout | Network connectivity issue | Verify routing and firewall rules |
| Authentication failed | Incorrect Agent secret | Copy the correct secret from Jenkins |
| Agent not found | Incorrect Agent name | Use the exact configured Node name |
| WebSocket connection failed | Incorrect URL or WebSocket configuration | Verify Controller URL and WebSocket option |
| agent.jar not found | Incorrect working directory | Download or specify the correct JAR path |
| Permission denied | Working directory permission issue | Check ownership and permissions |
| Agent goes offline | Process stopped or network disconnected | Check process and Agent logs |

### 🔍 Useful Troubleshooting Commands

Check Jenkins service on Controller:

```bash
sudo systemctl status jenkins
```

Check Java on Agent:

```bash
java -version
```

Check network connectivity:

```bash
curl -I http://CONTROLLER_PUBLIC_IP:8080/
```

Check Agent process:

```bash
ps -ef | grep agent.jar
```

Check Agent logs:

```bash
tail -f agent.log
```

---

## 🛑 17. Stop the Background Agent

If we want to stop the Agent, first identify its process:

```bash
ps -ef | grep agent.jar
```

Then stop the correct Agent process using its PID:

```bash
kill PID
```

Replace `PID` with the actual process ID.

Verify:

```bash
ps -ef | grep agent.jar
```

After stopping the process, the Agent will eventually show as Offline in Jenkins.

**Important:** Do not blindly use `pkill -f java`, because it can stop other Java applications running on the same machine.

---

## 🔐 18. Security Best Practices

- 🔒 Keep the Agent secret confidential.
- 🔑 Never commit Agent secrets to GitHub.
- 🌐 Prefer HTTPS for Controller communication.
- 🛡️ Restrict Security Group access.
- 🔐 Use private networking where possible.
- ☕ Keep Java updated and compatible.
- 📁 Use a dedicated Agent working directory.
- 👤 Run the Agent with an appropriately restricted Linux user.
- 📝 Monitor Agent logs for connection errors.
- 🔄 Configure a reliable process manager for production instead of relying only on a manually started `nohup` process.

---

## 📚 19. Important Commands – Quick Revision

| Command | Purpose |
|---|---|
| `java -version` | Check Java version |
| `mkdir -p ~/jenkins-agent` | Create Agent directory |
| `cd ~/jenkins-agent` | Enter Agent directory |
| `wget URL` | Download agent.jar |
| `ls -lh agent.jar` | Check downloaded JAR |
| `java -jar agent.jar ...` | Start Agent in foreground |
| `nohup ... > agent.log 2>&1 &` | Start Agent in background |
| `cat agent.log` | Display complete logs |
| `tail -f agent.log` | Monitor live logs |
| `ps -ef \| grep agent.jar` | Check Agent process |
| `kill PID` | Stop Agent process |

---

## 🎯 20. Day 06 Learning Outcome

After completing this practical, we understand:

- 🌐 Inbound Agent architecture
- 🔄 WebSocket communication
- 🖥️ Agent Node creation
- ⚙️ Node configuration
- ☕ Java installation on Agent
- 📦 Downloading `agent.jar`
- 🔐 Agent authentication using Secret
- 🚀 Foreground and background execution
- 📝 `nohup`, `&`, and log redirection
- 🔍 Agent process and connection verification
- 🧪 Executing Pipeline on a specific Agent
- 🛠️ Troubleshooting common connection problems

---

## 🚀 Next Day

**Day 07 – Jenkins SSH Agents and Parallel Builds**

In the next session, we will configure an Agent using SSH, understand SSH credentials and host key verification, and execute Pipeline stages across multiple Agents using parallel execution.
