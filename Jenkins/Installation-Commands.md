# 🛠️ Jenkins – Installation Commands

This document provides installation command references for Jenkins and additional tools used in Jenkins-based CI/CD pipelines.

# 🔴 1. Jenkins Installation

This section contains the installation commands and dependencies specifically required to install and run Jenkins.

### ☕ Java (JDK)

**Purpose:** Jenkins requires a compatible Java runtime environment to start and operate.

- 📌 [Java Installation Commands](https://github.com/byteAlok/DevOps-Tools-Installation-Commands/blob/main/Programming%20Languages%20%26%20Development/Java.md)

### 🔴 Jenkins

**Purpose:** Install and configure the Jenkins automation server.

- 📌 [Jenkins Installation Commands](https://github.com/byteAlok/DevOps-Tools-Installation-Commands/blob/main/DevOps%20%26%20Infrastructure/Jenkins.md)
- 📖 [Official Jenkins Documentation](https://www.jenkins.io/doc/book/installing/)

### 🐙 Git

**Purpose:** Used for source code checkout and repository integration with Jenkins pipelines.

- 📌 [Git Installation Commands](https://github.com/byteAlok/DevOps-Tools-Installation-Commands/blob/main/Version%20Control/Git.md)

---

# ⚙️ 2. Additional Tools & Dependencies

This section contains additional tools and technologies that can be integrated with Jenkins according to application requirements and practical CI/CD implementations.

**Note:** These tools are not mandatory for installing or running Jenkins. They are used for application development, building, testing, containerization, infrastructure automation, deployment, and monitoring.

## 💻 Programming Languages & Application Development

| Technology | Purpose | Installation Commands |
|---|---|---|
| 🔷 .NET | Build and publish ASP.NET Core applications. | [.NET](https://github.com/byteAlok/DevOps-Tools-Installation-Commands/blob/main/Programming%20Languages%20%26%20Development/Dotnet.md) |
| ☕ Java | Build and test Java applications. | [Java](https://github.com/byteAlok/DevOps-Tools-Installation-Commands/blob/main/Programming%20Languages%20%26%20Development/Java.md) |
| 🟢 Node.js | Build and test Node.js applications. | [Node.js](https://github.com/byteAlok/DevOps-Tools-Installation-Commands/blob/main/Programming%20Languages%20%26%20Development/NodeJs.md) |
| 🐘 PHP | Build and test PHP applications. | [PHP](https://github.com/byteAlok/DevOps-Tools-Installation-Commands/blob/main/Programming%20Languages%20%26%20Development/Php.md) |
| 🐍 Python | Run Python applications, scripts, and automated tests. | [Python](https://github.com/byteAlok/DevOps-Tools-Installation-Commands/blob/main/Programming%20Languages%20%26%20Development/Python.md) |

## 🐳 Containerization & Orchestration

| Technology | Purpose | Installation Commands |
|---|---|---|
| 🐳 Docker | Build, package, and publish application container images. | [Docker](https://github.com/byteAlok/DevOps-Tools-Installation-Commands/blob/main/DevOps%20%26%20Infrastructure/Docker.md) |
| ☸️ Kubernetes | Automate container deployment, scaling, and orchestration. | [Kubernetes](https://github.com/byteAlok/DevOps-Tools-Installation-Commands/blob/main/DevOps%20%26%20Infrastructure/Kubernates.md) |

## ☁️ Cloud Platforms & CLI Tools

| Platform | Purpose | Installation Commands |
|---|---|---|
| 🟠 AWS | Manage AWS resources and automate cloud deployments. | [AWS](https://github.com/byteAlok/DevOps-Tools-Installation-Commands/blob/main/Cloud%20Platforms%20%26%20CLI%20Tools/AWS.md) |
| 🔵 Azure | Manage Azure resources and automate deployments. | [Azure](https://github.com/byteAlok/DevOps-Tools-Installation-Commands/blob/main/Cloud%20Platforms%20%26%20CLI%20Tools/Azure.md) |
| 🌐 GCP | Manage Google Cloud resources and deployment operations. | [GCP](https://github.com/byteAlok/DevOps-Tools-Installation-Commands/blob/main/Cloud%20Platforms%20%26%20CLI%20Tools/GCP.md) |

## 🏗️ Infrastructure as Code & Configuration Management

| Technology | Purpose | Installation Commands |
|---|---|---|
| 🏗️ Terraform | Automate cloud infrastructure provisioning using IaC. | [Terraform](https://github.com/byteAlok/DevOps-Tools-Installation-Commands/blob/main/DevOps%20%26%20Infrastructure/Terraform.md) |
| 🔧 Ansible | Automate server configuration and application deployment. | [Ansible](https://github.com/byteAlok/DevOps-Tools-Installation-Commands/blob/main/DevOps%20%26%20Infrastructure/Ansible.md) |

## 📊 Monitoring & Observability

| Technology | Purpose | Installation Commands |
|---|---|---|
| 📈 Prometheus, Grafana & PagerDuty | Infrastructure monitoring, visualization, and alerting. | [Monitoring Stack](https://github.com/byteAlok/DevOps-Tools-Installation-Commands/blob/main/Monitoring%20%26%20Observability/Prometheus-Grafana-PagerDuty-Monitoring.md) |
| 🖥️ Prometheus Node Exporter | Collect system-level metrics from Linux servers. | [Node Exporter](https://github.com/byteAlok/DevOps-Tools-Installation-Commands/blob/main/Monitoring%20%26%20Observability/Prometheus-NodeExporter.md) |

---

## 📌 Important Notes

- 🔴 **Jenkins Installation:** Java and the Jenkins installation itself are the primary runtime requirements.
- 🐙 **Git:** Commonly needed for source code integration and checkout.
- ⚙️ **Additional Tools:** Install only when required by the application, pipeline, or practical project.
- 🖥️ **Execution Environment:** Tools may need to be installed on Jenkins Agents or other execution environments rather than the Jenkins Controller.

---

🔗 **Installation Commands Repository:** [DevOps-Tools-Installation-Commands](https://github.com/byteAlok/DevOps-Tools-Installation-Commands)

👨‍💻 **Maintained by:** Alok Maurya | Full Stack Engineer 

🌐 **Portfolio:** [aloksde.com](https://aloksde.com)
