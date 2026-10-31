# ⚡ GitHub Actions – Learning & Practical Projects

Welcome to my **GitHub Actions Learning Journey!** 🚀

This section documents my hands-on learning and practical implementation of **GitHub Actions**, a GitHub-native automation platform used to build CI/CD pipelines and automate software development workflows.

The goal is to understand how workflow automation works, starting with fundamental concepts and gradually moving towards advanced CI/CD implementations and real-world DevOps projects.

## 🎯 Learning Objectives

- ⚙️ Understand GitHub Actions architecture and workflow execution.
- 🔄 Learn Continuous Integration (CI) and Continuous Deployment (CD).
- 📝 Create and configure YAML-based workflows.
- 🚀 Automate application builds, testing, and deployments.
- 🐳 Integrate Docker into CI/CD pipelines.
- ☁️ Automate infrastructure provisioning using Terraform.
- ☸️ Implement Kubernetes deployment automation.
- 🔧 Use Ansible for configuration management.
- 🔗 Build complete end-to-end CI/CD pipelines.

## 📚 Learning Journey – Day-wise Practice

My learning journey is organized day by day. Each topic I learn is documented through its corresponding workflow YAML file inside GitHub's default `.github/workflows/` directory.

This approach helps me maintain a practical record of what I have learned and how GitHub Actions workflows are configured and executed.

**Workflow File Naming Convention:**

```text
.github/
└── workflows/
    |
    ├── day-1-<topic-name>.yaml
    ├── day-2-<topic-name>.yaml
    ├── day-3-<topic-name>.yaml
    ├── ...
    |
    ├── project-1-dev.yaml
    ├── project-1-test.yaml
    ├── project-1-prod.yaml
    |
    ├── project-2-dev.yaml
    ├── project-2-test.yaml
    ├── project-2-prod.yaml
    └── ...
```

**Examples:**
- 📌 `day-1-workflow-basics.yaml`
- 📌 `day-2-events-and-triggers.yaml`
- 📌 `day-3-jobs-and-steps.yaml`
- 📌 `day-4-runners-and-actions.yaml`

*Note: These are illustrative examples. Actual filenames and topics will reflect my learning progress.*

## 🛠️ Practical Projects

After understanding the fundamentals, I will apply the concepts through structured, hands-on projects.

Each project will have its own directory containing application source code, configuration files, automation scripts, tests, and documentation wherever required.

| Project | Implementation |
|---|---|
| 🚀 Project 1 | Basic CI |
| ☁️ Project 2 | Terraform Infrastructure |
| 🐳 Project 3 | Docker CI/CD |
| ☸️ Project 4 | Kubernetes Deployment |
| 🔧 Project 5 | Ansible Automation |
| 🔗 Project 6 | End-to-End CI/CD |

### 🧩 Project Structure

```text
GitHub-Actions-Projects/
│
├── Project-1-Basic-CI/
│   ├── Application/
│   ├── Tests/
│   └── README.md
│
├── Project-2-Terraform-Infrastructure/
│   ├── Application/
│   ├── Terraform/
│   └── README.md
│
├── Project-3-Docker-CI-CD/
│   ├── Application/
│   ├── Docker/
│   ├── Tests/
│   └── README.md
│
├── Project-4-Kubernetes-Deployment/
│   ├── Application/
│   ├── Docker/
│   ├── Kubernetes/
│   └── README.md
│
├── Project-5-Ansible-Automation/
│   ├── Application/
│   ├── Ansible/
│   └── README.md
│
└── Project-6-End-to-End-CI-CD/
    ├── Application/
    ├── Docker/
    ├── Kubernetes/
    ├── Terraform/
    ├── Ansible/
    ├── Tests/
    └── README.md
```

## ⚙️ How GitHub Actions Workflows Are Organized

GitHub Actions automatically discovers workflow files from the repository's default `.github/workflows/` directory.

Unlike the practical project directories, workflow YAML files are maintained at the repository root.

Each workflow can be configured with specific:
- 🔔 Events and triggers (`push`, `pull_request`, `workflow_dispatch`).
- 🌿 Branch filters.
- 📂 Path filters.
- 🖥️ Runners and execution environments.
- 📦 Jobs, steps, and reusable actions.
- 🔐 Secrets and environment variables.

For this repository, workflows will be configured to execute only when relevant files or directories are modified, wherever applicable.

## 🔄 Learning & Implementation Approach

My learning will progress through the following stages:

1. 📖 **Understand:** Learn the concepts, terminology, architecture, and working principles.
2. 🧪 **Experiment:** Create small YAML workflows and test different events, triggers, jobs, and actions.
3. ⚙️ **Automate:** Implement automation for application builds, testing, and deployment.
4. 🛠️ **Build:** Develop independent practical projects integrating different DevOps technologies.
5. 🚀 **Integrate:** Combine multiple tools and technologies to build end-to-end CI/CD pipelines.

## 🧰 Technologies & Tools

- 🐙 GitHub & Git
- ⚡ GitHub Actions
- 📝 YAML
- 🐳 Docker
- ☸️ Kubernetes
- ☁️ Terraform
- 🔧 Ansible
- 🖥️ Linux & GitHub-hosted Runners
- 🔐 GitHub Secrets

## 🌱 Learning Philosophy

This repository is part of my continuous DevOps learning journey. Instead of focusing only on theoretical explanations, I believe in learning by doing.

Every workflow, experiment, configuration, and project will help me understand how automation works in real development and deployment environments.

**Learn → Experiment → Automate → Build → Integrate → Improve.** 🚀

---

👨‍💻 **Maintained by:** Alok Maurya | DevOps & Cloud Enthusiast

🔗 **GitHub:** [@byteAlok](https://github.com/byteAlok)

🌐 **Portfolio:** [aloksde.com](https://aloksde.com)