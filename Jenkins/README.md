# 🔴 Jenkins

### Open-Source Automation Server for CI/CD

<p align="center">
  <img src="https://img.shields.io/badge/Jenkins-Automation%20Server-D24939?style=for-the-badge&logo=jenkins&logoColor=white" />
  <img src="https://img.shields.io/badge/Category-CI%2FCD-16A5FF?style=for-the-badge" />
  <img src="https://img.shields.io/badge/License-MIT-10B981?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Status-Learning%20in%20Progress-F59E0B?style=for-the-badge" />
</p>

<p align="center">
  <b>Understand · Configure · Automate · Integrate · Deploy</b>
</p>

---

## 📌 1. Introduction to Jenkins

**Jenkins** is a free and open-source automation server written in Java. It is primarily used to automate software development workflows, including Continuous Integration (CI), Continuous Delivery (CD), testing, building, and deployment.

It helps developers and DevOps teams automate repetitive tasks throughout the software development lifecycle.

Jenkins integrates with various development tools, version control systems, build tools, testing frameworks, container platforms, cloud services, and deployment technologies through its extensive plugin ecosystem.

Instead of manually executing every development and deployment task, Jenkins allows teams to define automated workflows that can be triggered by code changes, scheduled events, or manual actions.

### 🎯 What Is Jenkins Used For?

- 🔄 Continuous Integration (CI)
- 🚀 Continuous Delivery and Deployment (CD)
- ⚙️ Build Automation
- 🧪 Automated Testing
- 📦 Artifact Generation and Management
- ☁️ Application Deployment
- 🔗 Integration with Development Tools
- 🤖 Workflow and Task Automation

---

## 📜 2. History of Jenkins

Jenkins was originally developed under the name **Hudson** by **Kohsuke Kawaguchi**, a software engineer working at Sun Microsystems.

The project started in 2004 to solve a practical problem: developers frequently broke software builds when integrating their code changes.

Kohsuke wanted an automated system that could continuously build and test code changes so that errors could be detected earlier.

The project gradually gained popularity among developers and was released as open-source software.

### 🗓️ Jenkins Historical Timeline

| Year | Historical Event |
|---|---|
| 2004 | Kohsuke Kawaguchi started the project under the name Hudson at Sun Microsystems. |
| 2005 | Hudson became an open-source project and started gaining adoption. |
| 2010 | Oracle acquired Sun Microsystems, bringing Hudson under Oracle's ownership. |
| 2011 | Following a governance and naming dispute, the community fork adopted the name Jenkins. |
| 2011 onwards | Jenkins developed into an independent community-driven open-source automation project. |
| 2010s | Jenkins became widely adopted for CI/CD automation across software development organizations. |
| 2020s | Jenkins continued evolving with Pipeline as Code, distributed builds, cloud integrations, and container-based execution. |
| 2026 | Jenkins remains an established CI/CD platform alongside GitHub Actions, GitLab CI, and Azure DevOps. |

### 👨‍💻 Creator of Jenkins

**Kohsuke Kawaguchi**

- Original project name: Hudson
- Project started: 2004
- Original organization: Sun Microsystems
- Jenkins name adopted: 2011
- Technology: Java

### 🔍 Why Was Hudson Renamed to Jenkins?

After Oracle acquired Sun Microsystems, disagreements arose between Oracle and the Hudson community regarding the project's governance and ownership.

In January 2011, the community proposed moving forward with the name Jenkins.

The community subsequently continued developing Jenkins as an independent open-source project.

**Important:** Jenkins and Hudson have a common origin, but Jenkins became the community-led continuation of the project under a new name.

---

## ❓ 3. Why Do We Need Jenkins?

Before understanding Jenkins, consider how software development and deployment activities were performed without automated CI/CD tools.

Suppose a development team is building a web application.

Whenever a developer changes the application code, the following activities may need to be performed:

1. Download or retrieve the latest source code.
2. Compile and build the application.
3. Execute automated tests.
4. Identify compilation errors and test failures.
5. Package the application.
6. Deploy the application to a server.
7. Verify the deployment.

Performing these tasks manually for every code change can consume significant time and introduce human errors.

### Without Jenkins

```text
Developer Pushes Code
        |
        ▼
Manual Code Retrieval
        |
        ▼
Manual Application Build
        |
        ▼
Manual Testing
        |
        ▼
Manual Packaging
        |
        ▼
Manual Deployment
        |
        ▼
Application Running
```

### With Jenkins

```text
Developer Pushes Code
        |
        ▼
GitHub Repository
        |
        ▼
Jenkins Trigger
        |
        ▼
Automated Code Checkout
        |
        ▼
Automated Build
        |
        ▼
Automated Testing
        |
        ▼
Application Packaging
        |
        ▼
Automated Deployment
        |
        ▼
Application Running
```

### 🎯 Main Purpose

Jenkins reduces repetitive manual work by automating software development and delivery activities.

It helps teams achieve:

- Faster software delivery.
- Early detection of errors.
- Consistent build and deployment processes.
- Reduced manual intervention.
- Better collaboration between development and operations teams.

---

## ⚙️ 4. How Does Jenkins Work?

Jenkins works by executing predefined automation instructions.

It can monitor source code repositories, receive webhook notifications, execute scheduled jobs, and start pipelines based on configured triggers.

For example:

A developer pushes updated code to a GitHub repository. GitHub sends a webhook notification to Jenkins. Jenkins starts the configured pipeline and executes the required build, testing, and deployment tasks.

### Basic Working Process

| Step | Activity | Description |
|---|---|---|
| 01 | Trigger | Jenkins receives an event or manual execution request. |
| 02 | Checkout | Retrieves source code from the repository. |
| 03 | Build | Compiles or builds the application. |
| 04 | Test | Executes automated tests. |
| 05 | Package | Generates application packages or artifacts. |
| 06 | Deploy | Deploys the application to the target environment. |
| 07 | Report | Displays execution status and build logs. |

### 🧠 Important Concepts

| Term | Meaning |
|---|---|
| Job | A configured unit of work in Jenkins. |
| Build | An execution of a configured job. |
| Pipeline | A sequence of automated tasks. |
| Jenkinsfile | A file containing Jenkins Pipeline definitions. |
| Plugin | An extension that adds functionality or integrations. |
| Controller | The central Jenkins management component. |
| Agent | A machine or environment that executes assigned tasks. |
| Executor | An execution slot available on a Jenkins node. |

*Detailed architecture, Controller-Agent communication, Master-Slave terminology, Jenkinsfiles, and Pipeline implementation will be covered in separate learning sections.*

---

## ⚖️ 5. Advantages of Jenkins

### ✅ Benefits

**1. Open Source**

Jenkins is freely available under the MIT License. Organizations can use and customize it without purchasing a commercial Jenkins license.

**2. Extensive Plugin Ecosystem**

Jenkins supports a large number of plugins for integrating with GitHub, GitLab, Docker, Kubernetes, Maven, cloud platforms, and other tools.

**3. Platform Independence**

Jenkins can run on supported Windows, Linux, and macOS environments. It can also run using Docker and Kubernetes-based environments.

**4. Pipeline as Code**

Jenkins allows automation workflows to be defined in Jenkinsfiles, making pipeline configurations version-controllable and reusable.

**5. Distributed Build Execution**

Jenkins supports multiple agents, allowing workloads to be distributed across different machines.

**6. Flexible Automation**

It can automate different types of tasks, including application builds, testing, packaging, deployment, and infrastructure-related workflows.

**7. Integration Capabilities**

Jenkins can integrate with different source control platforms, cloud services, testing tools, and container technologies.

**8. Community Support**

Jenkins has a large open-source community that contributes plugins, documentation, improvements, and support.

---

## ⚠️ 6. Disadvantages of Jenkins

### ❌ Limitations

**1. Maintenance Overhead**

Jenkins requires regular maintenance, upgrades, plugin management, and system administration.

**2. Plugin Compatibility Issues**

Some plugins may have compatibility issues with particular Jenkins versions or other installed plugins.

**3. Complex Configuration**

Configuring advanced pipelines, distributed agents, credentials, and integrations can be challenging for beginners.

**4. Infrastructure Management**

Unlike fully managed CI/CD services, self-hosted Jenkins requires organizations to manage its infrastructure, storage, availability, and security.

**5. Scaling Complexity**

Large Jenkins environments require proper controller and agent management, resource allocation, and workload distribution.

**6. Security Responsibility**

Administrators must manage access permissions, credentials, plugin security, network restrictions, and updates.

**7. User Interface**

Compared with some modern CI/CD platforms, Jenkins' default user interface may feel less modern.

---

## 📈 7. Jenkins Market Adoption & Industry Demand

Jenkins has been an important CI/CD tool for many years. Its adoption grew significantly during the expansion of Agile development, Continuous Integration, and DevOps practices.

However, the CI/CD ecosystem has evolved.

Modern platforms such as GitHub Actions, GitLab CI/CD, and Azure DevOps offer integrated automation capabilities, increasing competition in this space.

### 📊 Historical Industry Position

| Period | Jenkins Industry Position |
|---|---|
| 2004–2010 | Early adoption of automated build and continuous integration practices. |
| 2011–2015 | Growing adoption as Jenkins became an independent community project. |
| 2016–2020 | Widely used across enterprise CI/CD environments. |
| 2021–2024 | Continued enterprise usage alongside growing cloud-native and integrated CI/CD platforms. |
| 2025–2026 | Still relevant, but faces stronger competition from GitHub Actions and GitLab CI. |

### 📊 CI/CD Tool Adoption: 2025 vs 2026

According to the JetBrains State of CI/CD Tools Survey:

| CI/CD Tool | 2025 | 2026 |
|---|---:|---:|
| GitHub Actions | 41% | 47% |
| GitLab CI | 34% | 32% |
| Jenkins | 31% | 28% |
| Azure DevOps Server | 14% | 15% |
| TeamCity | 7% | 10% |

<sub>Source: JetBrains State of CI/CD Tools Survey 2025/2026. Respondents could select multiple tools, so these figures are not exclusive market shares.</sub>

### 📉 What Does This Mean for Jenkins?

- Jenkins adoption decreased from 31% to 28% in this survey.
- GitHub Actions gained adoption and became the leading option in the survey.
- GitLab CI also remains a significant competitor.
- Jenkins continues to be used in organizations with established and customized CI/CD infrastructure.
- Self-hosted CI/CD platforms remain relevant for organizations requiring control over infrastructure, networks, security, and execution environments.

### 💼 Is Jenkins Still in Demand in the Job Market?

**Yes. Jenkins is still a relevant skill in DevOps and Cloud Engineering roles.**

Recent Indian job descriptions continue to mention Jenkins alongside other technologies.

For example:

- DevOps roles at Bangalore-based companies mention Jenkins, Docker, Kubernetes, AWS, and Linux.
- Enterprise DevOps roles mention Jenkins and GitHub Actions for pipeline development.
- Cloud and DevOps positions combine Jenkins with Terraform, Ansible, monitoring, and scripting.

However, knowing Jenkins alone is not sufficient for most modern DevOps roles.

### 🎯 Skills Commonly Combined with Jenkins

| Category | Technologies |
|---|---|
| CI/CD | Jenkins, GitHub Actions, GitLab CI |
| Version Control | Git, GitHub, GitLab |
| Operating Systems | Linux, Ubuntu, Amazon Linux |
| Containers | Docker |
| Orchestration | Kubernetes |
| Cloud | AWS, Azure, GCP |
| Infrastructure as Code | Terraform |
| Configuration Management | Ansible |
| Scripting | Bash, Python |
| Monitoring | Prometheus, Grafana |

**My takeaway:** Learn Jenkins properly, but also understand the underlying CI/CD concepts. Those concepts are transferable across Jenkins, GitHub Actions, GitLab CI/CD, and Azure DevOps.

---

## 💻 8. Jenkins System Requirements

Jenkins is a Java-based application, so a compatible Java runtime environment is required.

### 🧰 Software Requirements

| Requirement | Details |
|---|---|
| Java | Java 21 or Java 25 (supported Jenkins runtime versions) |
| Operating System | Supported Linux, Windows, or macOS environment |
| Web Browser | Modern compatible web browser |
| Network | Network connectivity for repositories, plugins, and integrations |
| Disk Space | Depends on jobs, plugins, logs, and build artifacts |

### 🖥️ Hardware Requirements

According to the official Jenkins installation documentation:

| Resource | Minimum Requirement | Recommended for a Small Team |
|---|---:|---:|
| RAM | 256 MB | 4 GB or more |
| Storage | 1 GB | 50 GB or more |
| CPU | Workload-dependent | Depends on concurrent builds |

**Important:** The minimum values are basic installation requirements, not recommended specifications for running practical CI/CD workloads.

For actual builds, testing, Docker images, and multiple agents, additional CPU, RAM, and storage may be required.

### ☕ Java Compatibility

As per the current Jenkins Java support policy:

- Java 21 is supported.
- Java 25 is supported.
- Java 17 and older versions are no longer supported by current Jenkins releases.

The Java version used to run Jenkins does not necessarily have to be the same version used by an application being built.

For example, Jenkins can run on Java 21 while a configured build environment uses a different compatible JDK.

---

## 🌐 9. Supported Operating Systems & Workspaces

Jenkins is not restricted to a single operating system.

It can run in different environments depending on the installation method, Java compatibility, operating system support, and system configuration.

### 🐧 Linux Distributions

| Operating System | Installation Method | Environment |
|---|---|---|
| Ubuntu | APT Package Manager | Local VM / AWS / Server |
| Debian | APT Package Manager | Local VM / Server |
| Amazon Linux 2 | RPM Package Manager | AWS EC2 |
| Amazon Linux 2023 | DNF Package Manager | AWS EC2 |
| RHEL | YUM / DNF | Enterprise Server |
| Rocky Linux | YUM / DNF | Server / VM |
| Fedora | DNF | Local VM / Server |

**Amazon Linux Note:** Jenkins provides an official AWS installation tutorial using Amazon Linux 2. For Amazon Linux 2023, use DNF-based package management. Amazon Linux 2 reached the end of standard support on June 30, 2026, so Amazon Linux 2023 is the preferred choice for new AWS practice environments.

### 🪟 Windows

Jenkins supports Windows installations through its official Windows MSI installer.

- Windows 64-bit environment.
- Java runtime compatibility.
- Jenkins Windows Service.
- Local development and testing.
- Browser-based administration.

### 🍎 macOS

Jenkins can also run on macOS.

The Jenkins macOS installation documentation provides guidance, although its macOS installer is maintained outside the Jenkins project.

### 🐳 Container-Based Environments

Jenkins can also run inside containerized environments.

| Environment | Purpose |
|---|---|
| Docker | Run Jenkins as a container |
| Kubernetes | Run Jenkins in a container orchestration environment |
| Virtual Machine | Isolated Jenkins installation |
| Cloud Server | Host Jenkins on AWS, Azure, or other cloud infrastructure |

### 🏗️ Jenkins Practice Environments

The following environments can be used for learning and implementation:

```text
                 JENKINS
                    |
       ┌────────────┼────────────┐
       |            |            |
       ▼            ▼            ▼
    LOCAL PC     VIRTUAL VM     CLOUD
       |            |            |
       ▼            ▼            ▼
    Windows       Ubuntu       AWS EC2
    macOS         Linux        Amazon Linux
                  Debian       Ubuntu
                  RHEL
       |
       ▼
    Docker
```

### 🎯 Recommended Learning Environments

| Environment | Recommended Use |
|---|---|
| Windows | Initial installation and UI exploration |
| Ubuntu VM | Linux-based Jenkins administration |
| Amazon Linux 2023 | AWS EC2-based Jenkins practice |
| Docker | Container-based Jenkins deployment |
| Kubernetes | Advanced Jenkins deployment and integration |

**Important:** The Jenkins Controller and its Agents do not have to use the same operating system.

For example, a Jenkins Controller can run on Ubuntu while its Agents execute jobs on Windows and Amazon Linux machines.

---

## 🔌 10. Jenkins Installation Methods

Jenkins supports different installation and deployment methods.

| Method | Description |
|---|---|
| Linux Package Installation | Install Jenkins using APT, DNF, or RPM packages. |
| Windows MSI Installer | Install Jenkins as a Windows service. |
| WAR File | Run Jenkins using a compatible Java runtime. |
| Docker | Run Jenkins inside a Docker container. |
| Kubernetes | Deploy Jenkins in a Kubernetes environment. |
| Cloud VM | Install Jenkins on a cloud-based virtual machine. |

### 🌍 Default Web Access

After successful installation, Jenkins can generally be accessed through:

```text
http://localhost:8080
```

For a remote server:

```text
http://SERVER_PUBLIC_IP:8080
```

The default Jenkins web interface port is `8080`, unless configured differently.

⚠️ For cloud deployments, restrict access through security groups or firewalls. Do not expose Jenkins administration publicly without appropriate network restrictions and security configuration.

---

## 🏢 11. Where Is Jenkins Used?

Jenkins is used in different types of organizations and software development environments.

### Common Use Cases

- Web Application Development
- Enterprise Software Development
- Microservices-Based Applications
- Cloud Application Deployment
- Docker Image Automation
- Kubernetes Deployment Workflows
- Automated Testing
- Build and Release Management
- Infrastructure Automation

### Example: ASP.NET Core Application

A development team stores its ASP.NET Core Web API in GitHub.

Jenkins can automate:

1. Source code checkout.
2. .NET application build.
3. Automated testing.
4. Application publishing.
5. Artifact packaging.
6. Deployment to AWS EC2 or another configured server.

This makes Jenkins useful even when the application itself is developed using technologies other than Java.

---

## 📚 12. Learning Roadmap

The following topics will be explored progressively.

| # | Topic | Learning Focus |
|---|---|---|
| 01 | Introduction to Jenkins | History, purpose, features |
| 02 | Jenkins Architecture | Controller, Agent, Executor |
| 03 | Installation | Linux, Windows, Cloud |
| 04 | Configuration | Dashboard and system settings |
| 05 | Freestyle Projects | Basic job creation |
| 06 | Pipeline Projects | Pipeline fundamentals |
| 07 | Jenkinsfile | Pipeline as Code |
| 08 | Declarative Pipeline | Structured pipeline syntax |
| 09 | Scripted Pipeline | Groovy-based pipeline scripting |
| 10 | Master-Slave Architecture | Controller and Agent configuration |
| 11 | Distributed Builds | Workload distribution |
| 12 | Plugins | Extending Jenkins functionality |
| 13 | Credentials | Secure secret management |
| 14 | Webhooks | Automated pipeline triggers |
| 15 | GitHub Integration | Source control integration |
| 16 | Advanced Pipelines | Parallel execution and conditions |
| 17 | Security | Access control and security practices |
| 18 | Practical Projects | End-to-end implementations |

---

## 📖 13. References & Further Reading

### 🔴 Official Jenkins Resources

- [Jenkins Official Website](https://www.jenkins.io/)
- [Jenkins Documentation](https://www.jenkins.io/doc/)
- [Jenkins Download](https://www.jenkins.io/download/)
- [Jenkins Installation Guide](https://www.jenkins.io/doc/book/installing/)
- [Jenkins Architecture](https://www.jenkins.io/doc/book/using/using-agents/)
- [Jenkins Pipeline Documentation](https://www.jenkins.io/doc/book/pipeline/)
- [Jenkins Java Support Policy](https://www.jenkins.io/doc/book/platform-information/support-policy-java/)

### ☁️ Operating System Installation Guides

- [Ubuntu / Linux Installation](https://www.jenkins.io/doc/book/installing/linux/)
- [Amazon Linux – AWS Installation Tutorial](https://www.jenkins.io/doc/tutorials/tutorial-for-installing-jenkins-on-AWS/)
- [Windows Installation](https://www.jenkins.io/doc/book/installing/windows/)
- [macOS Installation](https://www.jenkins.io/doc/book/installing/macos/)
- [Docker Installation](https://www.jenkins.io/doc/book/installing/docker/)

### 📊 History & Industry Research

- [Jenkins History – Official Community](https://community.jenkins.io/t/lets-thank-kohsuke-the-creator-of-jenkins/168)
- [Hudson to Jenkins – Creator's Announcement](https://kohsuke.org/2011/01/11/bye-bye-hudson-hello-jenkins/)
- [Jenkins – Wikipedia](https://en.wikipedia.org/wiki/Jenkins_(software))
- [JetBrains State of CI/CD 2026](https://www.jetbrains.com/pages/state-of-cicd2026/)

---

## 👨‍💻 About This Learning Section

This directory is part of my **CI/CD Series** repository.

It contains Jenkins fundamentals, installation procedures, configuration files, practical exercises, automation scripts, troubleshooting documentation, and hands-on projects.

The objective is to understand Jenkins from its history and fundamentals to real-world CI/CD implementation.

---

<p align="center">
  <b>🔴 Understand Jenkins. ⚙️ Automate Workflows. 🚀 Deliver Software.</b>
</p>

<p align="center">
  <i>Learn · Practice · Implement · Improve</i>
</p>