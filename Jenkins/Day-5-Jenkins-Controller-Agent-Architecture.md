# 🏗️ Day 05 – Jenkins Controller-Agent Architecture

## 📌 1. Introduction to Jenkins Distributed Architecture

Jenkins is an automation server used to automate software development processes such as:

- 💻 Application Build
- 🧪 Automated Testing
- 📦 Package Creation
- 🚀 Application Deployment
- 🔄 Continuous Integration (CI)
- 🔁 Continuous Delivery (CD)

When Jenkins is installed on a single machine, that machine performs both management and build execution tasks.

However, in a real-world environment, multiple developers may trigger multiple pipelines simultaneously. Executing every build on a single machine can increase CPU, RAM, and disk utilization.

To solve this problem, Jenkins provides **Controller-Agent Architecture**, also known as Distributed Build Architecture.

In this architecture:

- 🧠 Controller manages the Jenkins environment.
- ⚙️ Agents execute the tasks assigned by the Controller.
- 🔄 Controller distributes workloads across available execution machines.

### 🎯 Why Do We Need Distributed Architecture?

Suppose an organization has 20 developers working on different applications.

They trigger multiple pipelines simultaneously:

- Java application build
- .NET application build
- React application build
- Python application testing
- Docker image creation
- Application deployment

If all these tasks execute on one Jenkins machine:

- CPU utilization may increase significantly.
- RAM consumption may become high.
- Multiple builds may compete for resources.
- Build execution may become slower.
- A resource-intensive build may affect other builds.

**Solution:** Use multiple Agents and distribute the workloads across them.

---

## 🧠 2. Jenkins Controller

The Controller is the central component of Jenkins architecture.

It manages the complete Jenkins environment and coordinates the execution of jobs across available machines.

### ⚙️ Responsibilities of Controller

| Responsibility | Description |
|---|---|
| 🌐 Web Interface | Provides the Jenkins Dashboard and Web UI. |
| 👤 User Management | Manages users, roles, and permissions. |
| 🔐 Credential Management | Stores credentials used by Jenkins jobs. |
| 🧩 Plugin Management | Installs and manages Jenkins plugins. |
| 📋 Job Management | Manages Freestyle Projects and Pipelines. |
| 📅 Job Scheduling | Determines when and where jobs should execute. |
| 📦 Build Management | Coordinates build execution. |
| 📊 Build History | Maintains build history and execution information. |
| 📝 Logs | Stores and displays build logs. |
| ⚙️ Configuration | Manages Jenkins system and job configurations. |

### 📂 Important Controller Directory

On a Linux-based Jenkins installation, the default Jenkins home directory is:

```bash
/var/lib/jenkins
```

This directory contains important Jenkins data.

| Directory/File | Purpose |
|---|---|
| `config.xml` | Main Jenkins configuration |
| `jobs/` | Job and Pipeline configurations |
| `plugins/` | Installed Jenkins plugins |
| `users/` | User-related information |
| `workspace/` | Build workspace directories |
| `secrets/` | Security-related files |
| `credentials.xml` | Encrypted credential information |
| `nodes/` | Agent/node configuration |

**Important:** The Jenkins home directory contains critical configuration and build data. It should be backed up regularly.

### 💡 Example

Suppose we create a Pipeline from the Jenkins Dashboard.

The Controller:

1. Receives the Pipeline execution request.
2. Reads the Pipeline configuration.
3. Determines where the Pipeline should execute.
4. Allocates an available Executor.
5. Assigns the execution to an eligible machine.
6. Tracks the execution status.
7. Displays the final result.

---

## ⚙️ 3. Jenkins Agent

An Agent is a separate machine connected to the Jenkins Controller.

It provides execution resources to Jenkins and performs the tasks assigned by the Controller.

An Agent can be:

- ☁️ AWS EC2 Instance
- 🖥️ Virtual Machine
- 🐧 Linux Server
- 🪟 Windows Server
- 🍎 macOS Machine
- 🐳 Container-based execution environment

### 🔧 Responsibilities of Agent

| Responsibility | Description |
|---|---|
| 💻 Build Execution | Compiles application source code. |
| 🧪 Testing | Executes automated test cases. |
| 📦 Packaging | Creates application packages and artifacts. |
| 🐳 Docker Operations | Builds and manages Docker images and containers. |
| 🚀 Deployment | Executes application deployment scripts. |
| 📁 Workspace | Maintains files required for build execution. |
| 📤 Execution Results | Reports execution status and logs to Controller. |

### 💡 Example

Suppose we have a Java application.

The Controller assigns the build task to Agent 1.

Agent 1 executes:

```bash
git clone https://github.com/example/java-app.git

mvn clean package
```

The commands execute on Agent 1, not necessarily on the Controller.

After execution, the result is reported back to the Controller.

### 📌 Important Point

An Agent does not need to have the same tools installed as the Controller.

For example:

| Machine | Installed Tools |
|---|---|
| Controller | Jenkins, Java |
| Agent 1 | Java, Maven, Git |
| Agent 2 | Node.js, npm, Git |
| Agent 3 | Docker, Git |

This allows different Agents to handle different workloads.

---

## 🔄 4. Controller vs Agent

| Feature | Controller | Agent |
|---|---|---|
| Main Purpose | Jenkins Management | Task Execution |
| Web Dashboard | Yes | No |
| Job Scheduling | Yes | No |
| Pipeline Management | Yes | Executes assigned Pipeline steps |
| Plugin Management | Yes | Uses required execution tools |
| Credentials Management | Central management | Receives required credentials securely |
| Build Execution | Possible, but not preferred for heavy workloads | Primary purpose |
| Build Workspace | Can have one | Has its own workspace |
| Multiple Machines | Manages multiple Agents | Connects to Controller |

**Remember:**

- 🧠 Controller = Management and Coordination
- ⚙️ Agent = Execution
- 🔢 Executor = Build Execution Slot

---

## 🔢 5. What is an Executor?

An Executor is a slot on a Jenkins node that allows Jenkins to execute a build or Pipeline task.

Every Jenkins node can have one or more Executors.

A node may be the Controller itself or an Agent.

### 📌 Example 1: Single Executor

Suppose Agent 1 has only one Executor.

```text
       Agent 1
   +-------------+
   | Executor 1  |
   |             |
   |   Build A   |
   +-------------+
```

Only one build can use that Executor at a time.

If another build is assigned to the same Agent, it must wait until an Executor becomes available.

### 📌 Example 2: Multiple Executors

Suppose Agent 1 has three Executors.

```text
          Agent 1
   +---------------------+
   |                     |
   |  Executor 1         |
   |  Executor 2         |
   |  Executor 3         |
   |                     |
   +---------------------+
```

The Agent can execute up to three build tasks simultaneously, provided the tasks are eligible for that node.

### 📊 Example 3: Total Executor Calculation

Suppose our Jenkins environment has the following configuration:

| Machine | Executors |
|---|---:|
| Controller | 1 |
| Agent 1 | 2 |
| Agent 2 | 2 |
| Agent 3 | 1 |
| **Total** | **6** |

Total Executors:

```text
1 + 2 + 2 + 1 = 6
```

Therefore, the Jenkins environment has six available execution slots.

**Important:** Six Executors do not guarantee that six complete Pipelines will always run simultaneously. Pipeline stages, agent restrictions, available resources, and scheduling conditions also affect execution.

### ⚠️ Executor Configuration

More Executors do not always mean better performance.

If an Agent has limited CPU and RAM, assigning too many Executors can cause:

- High CPU utilization
- Memory exhaustion
- Slow builds
- Resource contention

Therefore, Executor configuration should depend on the machine's available resources and workload.

---

## 🖥️ 6. What is a Node in Jenkins?

A Node is a machine that is part of the Jenkins execution environment.

It represents a machine where Jenkins can execute tasks.

In Jenkins terminology:

- The Controller is also a Node.
- Connected Agents are also Nodes.

### 📌 Example

Suppose we have:

```text
Jenkins Environment
       |
       +---- Controller
       |
       +---- Agent 1
       |
       +---- Agent 2
       |
       +---- Agent 3
```

Total Nodes = 4.

Here:

- 1 Controller Node
- 3 Agent Nodes

### 🔍 Node vs Agent vs Executor

| Term | Meaning |
|---|---|
| Node | A machine participating in Jenkins execution |
| Controller | Central Jenkins management machine |
| Agent | A machine used to execute assigned work |
| Executor | An execution slot available on a Node |

**Remember:** Node represents the machine, Agent represents its execution role, and Executor represents its build capacity.

---

## 🏷️ 7. What are Labels in Jenkins?

Labels are identifiers assigned to Jenkins Nodes.

They help Jenkins select the appropriate Agent for a particular Pipeline.

Suppose we have three Agents:

| Agent | Label |
|---|---|
| Agent 1 | `java` |
| Agent 2 | `nodejs` |
| Agent 3 | `docker` |

Now we can assign jobs based on their requirements.

### 💡 Example

A Java application should execute on the Java Agent.

```groovy
pipeline {
    agent {
        label 'java'
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building Java Application'
            }
        }
    }
}
```

Jenkins searches for an eligible Node with the `java` label.

If Agent 1 has that label and an available Executor, Jenkins can execute the Pipeline there.

### 🔍 Multiple Labels

A Node can have multiple labels.

For example:

```text
Agent 1:
java maven linux

Agent 2:
nodejs npm linux

Agent 3:
docker linux
```

We can use label expressions:

```groovy
agent {
    label 'linux && docker'
}
```

This selects a Node that has both `linux` and `docker` labels.

Another example:

```groovy
agent {
    label 'java || nodejs'
}
```

This allows Jenkins to select a Node with either the `java` or `nodejs` label.

### 📌 Common Label Operators

| Operator | Meaning |
|---|---|
| `&&` | Both labels must match |
| `||` | Either label can match |
| `!` | Excludes a label |
| `->` | If the left condition matches, the right must also match |
| `<->` | Both conditions must match together |

---

## 🔄 8. How Does Controller-Agent Communication Work?

The Controller and Agents communicate over a network.

The communication method depends on how the Agent is configured.

Common connection methods include:

1. 🌐 Inbound Agent
2. 🔐 SSH Agent

### 🌐 Inbound Agent

In an inbound connection, the Agent initiates the connection to the Controller.

The Agent connects to Jenkins using its agent process.

```text
+---------------------+
| Jenkins Controller |
|                     |
|     Port 8080       |
+---------------------+
          ^
          |
          | Agent Connection
          |
+---------------------+
|     Agent EC2       |
|                     |
|     agent.jar       |
+---------------------+
```

Jenkins inbound Agents can use WebSocket communication.

With WebSocket enabled, the Agent connection can operate over the Jenkins HTTP/HTTPS endpoint, avoiding the need for a separate inbound TCP agent port.

### 🔐 SSH Agent

In SSH-based communication, the Controller initiates an SSH connection to the Agent.

```text
+---------------------+
| Jenkins Controller |
|                     |
|  SSH Private Key    |
+---------------------+
          |
          | SSH Connection
          |
          v
+---------------------+
|     Agent EC2       |
|                     |
|  SSH Public Key     |
+---------------------+
```

The Controller uses SSH credentials to connect to the Agent.

The Agent must have an SSH server running and the appropriate public key authorized.

### 📊 Inbound vs SSH Agent

| Feature | Inbound Agent | SSH Agent |
|---|---|---|
| Connection Initiator | Agent | Controller |
| Connection Direction | Agent → Controller | Controller → Agent |
| Authentication | Agent secret | SSH credentials |
| Java Required | Yes | Yes |
| SSH Server Required | No | Yes |
| Common Use | Inbound/WebSocket connection | SSH-based launch |

**Note:** Detailed practical setup of inbound and SSH Agents will be covered in Day 06 and Day 07.

---

## 🧩 9. What is Agent Workload Distribution?

Workload distribution means assigning different build tasks to different Agents.

Suppose an organization has three Agents:

| Agent | Workload |
|---|---|
| Agent 1 | Java Application Build |
| Agent 2 | React Application Build |
| Agent 3 | Docker Image Build |

The Controller schedules the jobs according to the Pipeline configuration, labels, and available Executors.

### 📌 Example Pipeline

```groovy
pipeline {
    agent none

    stages {
        stage('Java Build') {
            agent {
                label 'java'
            }

            steps {
                echo 'Executing Java Build'
            }
        }

        stage('Node Build') {
            agent {
                label 'nodejs'
            }

            steps {
                echo 'Executing Node.js Build'
            }
        }

        stage('Docker Build') {
            agent {
                label 'docker'
            }

            steps {
                echo 'Executing Docker Build'
            }
        }
    }
}
```

### 🔍 Explanation

- `agent none` means Jenkins will not allocate one global Agent for the entire Pipeline.
- `agent { label 'java' }` selects a Node with the `java` label for that stage.
- `agent { label 'nodejs' }` selects a Node with the `nodejs` label.
- `agent { label 'docker' }` selects a Node with the `docker` label.

Each stage runs on its selected Node.

**Important:** These stages execute sequentially by default. Using different Agents does not automatically make them parallel. Parallel execution requires the `parallel` directive.

---

## 🏢 10. Real-World AWS EC2 Architecture

Suppose we want to create a distributed Jenkins environment using AWS EC2.

We create four EC2 instances:

| EC2 Instance | Role | Purpose |
|---|---|---|
| EC2-Controller | Controller | Jenkins Management |
| EC2-Agent-1 | Agent | Java Application Build |
| EC2-Agent-2 | Agent | Node.js Application Build |
| EC2-Agent-3 | Agent | Docker Operations |

### 🏗️ Architecture

```text
                    Developer
                        |
                        |
                  Trigger Pipeline
                        |
                        v
             +-----------------------+
             |   Jenkins Controller  |
             |                       |
             |  Jenkins Dashboard    |
             |  Job Scheduling       |
             |  Credentials          |
             |  Build Management     |
             +-----------------------+
                        |
           +------------+------------+
           |            |            |
           v            v            v
      +----------+ +----------+ +----------+
      | Agent 1  | | Agent 2  | | Agent 3  |
      |          | |          | |          |
      | Java     | | Node.js  | | Docker   |
      | Maven    | | npm      | | Docker   |
      | Git      | | Git      | | Git      |
      +----------+ +----------+ +----------+
           |            |            |
           +------------+------------+
                        |
                        v
              Execution Results
                        |
                        v
             +-----------------------+
             |   Jenkins Controller  |
             |                       |
             |  Build Status         |
             |  Console Output       |
             |  Build History        |
             +-----------------------+
```

### ⚙️ EC2 Network Requirements

For the architecture to work correctly:

- Controller must be reachable through the required Jenkins access port.
- Inbound Agents must be able to reach the Controller's Jenkins endpoint.
- SSH Agents require SSH connectivity from Controller to Agent.
- Security Groups should allow only the required traffic.
- Use private networking where possible instead of exposing Agents publicly.

### 🔐 Security Best Practices

- Do not expose SSH (port 22) to the entire internet.
- Restrict SSH access to the Controller's private IP or security group.
- Use Jenkins credentials instead of hardcoding passwords or private keys.
- Prefer private IP communication between EC2 instances.
- Keep Agent machines updated.
- Avoid running untrusted builds with excessive privileges.
- Back up Jenkins home directory.

---

## ⚠️ 11. Should We Run Builds on the Controller?

Jenkins can execute builds on the Controller itself.

However, in production environments, it is generally recommended to keep the Controller focused on management and use Agents for build execution.

### 🚨 Problems with Heavy Builds on Controller

- High CPU consumption
- High memory usage
- Reduced Jenkins UI responsiveness
- Resource contention
- Potential impact on Jenkins management operations

### ✅ Recommended Configuration

The Controller can be configured with zero Executors so that Pipeline workloads execute only on Agents.

For example:

```text
Controller:
Executors = 0

Agent 1:
Executors = 2

Agent 2:
Executors = 2

Agent 3:
Executors = 1
```

This keeps build execution separate from the Controller's management responsibilities.

---

## 📚 12. Important Jenkins Terminology

| Term | Explanation |
|---|---|
| Controller | Central Jenkins management component |
| Agent | Machine that executes assigned tasks |
| Node | Machine participating in Jenkins |
| Executor | Build execution slot |
| Label | Identifier used to select Nodes |
| Workspace | Directory where a job executes |
| Queue | Holds builds waiting for execution |
| Build | An execution of a Jenkins job or Pipeline |
| Agent Protocol | Communication mechanism between Controller and Agent |
| Distributed Build | Workload execution across multiple machines |

---

## 📝 13. Quick Revision

| Question | Answer |
|---|---|
| Who manages Jenkins? | Controller |
| Who executes assigned tasks? | Agent |
| What is an Executor? | Build execution slot |
| What is a Node? | Machine participating in Jenkins |
| What are Labels? | Identifiers used to select Nodes |
| Can one Controller manage multiple Agents? | Yes |
| Can an Agent have multiple Executors? | Yes |
| Can Controller execute builds? | Yes |
| Is it recommended to run heavy builds on Controller? | No |
| Which connection methods are covered next? | Inbound and SSH |

---

## 🎯 14. Day 05 Learning Outcome

After completing this topic, we understand:

- 🧠 Jenkins Controller architecture
- ⚙️ Jenkins Agent architecture
- 🔢 Executors and execution capacity
- 🖥️ Nodes and their roles
- 🏷️ Labels and Node selection
- 🔄 Controller-Agent communication
- 🌐 Inbound and SSH connection concepts
- 🧩 Workload distribution
- ☁️ AWS EC2 distributed Jenkins architecture
- 🔐 Basic security practices

---

## 🚀 Next Day

**Day 06 – Jenkins Inbound Agent Setup (WebSocket)**

In the next session, we will practically connect EC2 Agent machines to the Jenkins Controller using the inbound WebSocket method.
