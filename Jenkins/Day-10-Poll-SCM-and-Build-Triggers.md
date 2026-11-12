# 📘 Day 10 – Poll SCM & Scheduled Build Triggers

## 🎯 Objective

In this practical, we will learn how to configure automatic build triggers in Jenkins using two different methods:

1. 🔄 **Poll SCM** – Periodically checks a configured SCM repository for changes and triggers a build when changes are detected.
2. ⏰ **Build Periodically** – Triggers a build at scheduled intervals, regardless of whether repository changes exist.

We will first create a Pipeline Job and then configure both methods separately inside the same job.

---

## 🧠 Important Concept – Where is the Jenkinsfile Stored?

Before configuring build triggers, understand the difference between these two Pipeline configurations.

### 1. Pipeline Script – Jenkins UI

Navigation:

`Jenkins Dashboard → New Item → Pipeline → Configure → Pipeline`

Select:

```text
Definition: Pipeline script
```

In this configuration, we write the Pipeline code directly inside the Jenkins UI.

**What works?**

- ✅ Build Periodically can trigger this job.
- ⚠️ Poll SCM requires a configured SCM repository to monitor. An inline Pipeline script alone does not provide a GitHub repository for polling.
- ⚠️ GitHub Webhook integration for SCM-based change detection also requires the appropriate SCM configuration and trigger integration.

### 2. Pipeline Script from SCM – GitHub Repository

Navigation:

`Jenkins Dashboard → New Item → Pipeline → Configure → Pipeline`

Select:

```text
Definition: Pipeline script from SCM
SCM: Git
```

In this configuration, the Jenkinsfile is stored in a GitHub repository.

**What works?**

- ✅ Poll SCM can monitor the configured GitHub repository.
- ✅ Build Periodically can trigger the job on a schedule.
- ✅ GitHub Webhook can notify Jenkins about repository push events when the required integration is configured.

### 📊 Comparison

| Feature | Pipeline Script | Pipeline Script from SCM |
|---|---|---|
| Jenkinsfile location | Jenkins UI | GitHub repository |
| Build Periodically | ✅ | ✅ |
| Poll SCM | Requires SCM configuration | ✅ |
| GitHub change detection | Requires SCM configuration | ✅ |
| GitHub Webhook integration | Requires appropriate SCM integration | ✅ |

> **Our practical approach:** We will use Pipeline script from SCM so that Poll SCM can monitor actual GitHub repository changes. This will also prepare us for Day 11 – GitHub Webhooks.

---

## 🛠️ Prerequisites

Before starting, make sure:

- Jenkins is running and accessible.
- Git is installed on the Jenkins Controller.
- A GitHub repository is available.
- A Jenkinsfile exists in the repository.
- The repository contains the required Pipeline code.

---

# 🚀 Step 1 – Create a Pipeline Job

First, we will create one Pipeline Job. We will use the same job to test both trigger methods.

### 1. Open Jenkins Dashboard

1. Open your Jenkins Dashboard.
2. Click **New Item** from the left sidebar.
3. Enter a job name:

```text
Day-10-Poll-SCM
```

4. Select **Pipeline**.
5. Click **OK**.

### 2. Configure the Pipeline

After clicking OK, Jenkins will open the job configuration page.

Scroll down to the **Pipeline** section.

Configure these settings:

| Setting | Value |
|---|---|
| Definition | Pipeline script from SCM |
| SCM | Git |
| Repository URL | Your GitHub repository URL |
| Credentials | `- none -` for a public repository |
| Branch Specifier | `*/main` |
| Script Path | Path to your Jenkinsfile |

Example:

```text
Repository URL:
https://github.com/byteAlok/CICD-Series.git

Branch Specifier:
*/main

Script Path:
Jenkins/Day-9-Jenkinsfile-and-Pipeline-as-Code/Jenkinsfile
```

Click **Save**.

### 3. Test the Pipeline

1. Open the newly created Pipeline Job.
2. Click **Build Now**.
3. Open the latest build.
4. Click **Console Output**.
5. Verify that the Pipeline executes successfully.

Now our Pipeline Job is ready.

---

# 🔄 Method 1 – Poll SCM

## 1. What is Poll SCM?

Poll SCM is a Jenkins build trigger that periodically checks the configured Source Code Management (SCM) repository, such as GitHub, for changes.

If Jenkins detects new changes in the monitored repository and branch, it automatically triggers a build.

If no changes are detected, Jenkins does not trigger a new build.

### 🔍 How Does Poll SCM Work?

```text
GitHub Repository
       |
       | New Commit / Push
       |
       v
Jenkins Polling Schedule
       |
       v
Check Repository Changes
       |
       v
Changes Detected?
       |
   +---+---+
   |       |
  YES      NO
   |       |
   v       v
 Trigger   No New
  Build     Build
```

## 2. Configure Poll SCM

Follow this exact navigation:

1. Open **Jenkins Dashboard**.
2. Click your Pipeline Job (`Day-10-Poll-SCM`).
3. Click **Configure** from the left sidebar.
4. Scroll down to the **Build Triggers** section.
5. Find and enable the **Poll SCM** checkbox.

You will see a Schedule field.

Enter:

```cron
H/5 * * * *
```

This configures Jenkins to check the repository at approximately 5-minute intervals.

6. Click **Save**.

## 3. Test Poll SCM

Now we will verify whether Jenkins automatically detects GitHub changes.

1. Open your GitHub repository.
2. Update an existing tracked file or add a new file.
3. Commit and push the changes to the configured branch.

Example:

```bash
git add .
git commit -m "Test Poll SCM trigger"
git push origin main
```

4. Wait for the next polling cycle.
5. Open your Jenkins Dashboard.
6. Check the Pipeline Job's **Build History**.

### ✅ Expected Result

- Jenkins periodically checks the configured GitHub repository.
- Jenkins detects the newly pushed changes.
- Jenkins automatically triggers a new build.
- A new build number appears in Build History.

**Important:** You do not need to click Build Now after pushing changes. Jenkins will trigger the build automatically when it detects changes.

### 💡 Important Understanding

Poll SCM is useful when Jenkins needs to monitor an external SCM repository.

If you write the Pipeline code directly inside Jenkins UI and do not configure an SCM repository for the job, there is no GitHub repository for Poll SCM to monitor.

That is why we are using **Pipeline script from SCM** for this practical.

---

# ⏰ Method 2 – Build Periodically

## 1. What is Build Periodically?

Build Periodically is a Jenkins trigger that executes a job according to a predefined schedule.

Unlike Poll SCM, it does not depend on detecting repository changes.

It triggers the Pipeline at scheduled intervals, even if no new changes have been pushed to GitHub.

### 🔍 How Does Build Periodically Work?

```text
Jenkins Scheduled Time
          |
          v
     Schedule Reached?
          |
          v
    Trigger Pipeline
          |
          v
      Build Starts
```

## 2. Configure Build Periodically

We will use the same Pipeline Job.

Follow this navigation:

1. Open **Jenkins Dashboard**.
2. Click your Pipeline Job (`Day-10-Poll-SCM`).
3. Click **Configure**.
4. Scroll down to the **Build Triggers** section.
5. Find and enable **Build periodically**.

You will see a Schedule field.

Enter:

```cron
H/5 * * * *
```

This schedules the job to run at approximately 5-minute intervals.

6. Click **Save**.

## 3. Test Build Periodically

1. Keep the Pipeline Job configured with **Pipeline script from SCM**.
2. Do not make any changes to your GitHub repository.
3. Wait for the next scheduled execution.
4. Open your Jenkins Dashboard.
5. Check the Pipeline Job's **Build History**.
6. Open the latest build and inspect **Console Output**.

### ✅ Expected Result

- Jenkins follows the configured schedule.
- The Pipeline executes automatically.
- A new build appears even if no changes were made to GitHub.

**Important:** You do not need to push new code to GitHub for Build Periodically to execute.

### 💡 Important Understanding

Build Periodically does not depend on GitHub changes.

It can run a Pipeline whose code is written directly inside Jenkins UI or one whose Jenkinsfile is stored in GitHub.

For example, you can use it to run:

- Daily reports.
- Scheduled cleanup tasks.
- Periodic health checks.
- Regular maintenance jobs.
- Scheduled application builds.

---

# 🕒 Jenkins Cron Syntax

Jenkins uses five fields in its cron expressions.

```text
MINUTE HOUR DAY-OF-MONTH MONTH DAY-OF-WEEK
```

| Field | Allowed Values |
|---|---|
| Minute | 0–59 |
| Hour | 0–23 |
| Day of Month | 1–31 |
| Month | 1–12 |
| Day of Week | 0–7 (Sunday can be 0 or 7) |

### Common Examples

| Cron Expression | Meaning |
|---|---|
| `* * * * *` | Every minute |
| `H/5 * * * *` | Approximately every 5 minutes |
| `H/10 * * * *` | Approximately every 10 minutes |
| `H * * * *` | Once every hour |
| `H 9 * * *` | Once daily around 9 AM |
| `H 9 * * 1-5` | Once daily around 9 AM, Monday to Friday |

### What is H in Jenkins?

`H` stands for **Hash**.

Jenkins uses a hash of the job name to choose a consistent time within the specified range.

It helps distribute scheduled jobs instead of running every job at exactly the same time.

For example:

```cron
H/5 * * * *
```

It means Jenkins schedules checks or executions at approximately 5-minute intervals, but the exact minute can vary depending on the job's hash.

---

# ⚖️ Poll SCM vs Build Periodically

| Feature | Poll SCM | Build Periodically |
|---|---|---|
| Checks configured SCM repository | Yes | No |
| Detects repository changes | Yes | No |
| Triggers build when changes are detected | Yes | Not required |
| Runs without repository changes | No | Yes |
| Uses cron schedule | Yes | Yes |
| Main purpose | Change-based builds | Time-based execution |

### Example Scenario

Suppose both methods are configured with:

```cron
H/5 * * * *
```

**🔄 Poll SCM:**

- Jenkins checks the configured GitHub repository.
- If changes are detected, a build starts.
- If no changes are detected, no new build starts.

**⏰ Build Periodically:**

- Jenkins follows its schedule.
- A build starts regardless of whether GitHub has changed.

---

# ⚠️ Important Practical Notes

1. Both triggers are configured from the same Pipeline Job's **Build Triggers** section.
2. Poll SCM and Build Periodically are independent triggers.
3. For learning, test one method at a time. Disable the first trigger before testing the second.
4. Poll SCM needs a configured SCM repository to monitor.
5. Build Periodically does not require GitHub changes.
6. A valid Pipeline configuration is required for both methods to execute the Jenkinsfile.
7. Jenkins scheduling follows the Jenkins Controller's system time and timezone.
8. If both triggers are enabled, both can independently schedule builds. This can create extra or overlapping builds.

---

# 🧰 Troubleshooting

| Problem | What to Check |
|---|---|
| Poll SCM does not trigger a build | SCM configuration, repository URL, branch, polling schedule and new commits |
| Build Periodically does not execute | Cron expression, enabled checkbox and Jenkins system time |
| GitHub changes are not detected | Correct branch, commit and repository configuration |
| Pipeline fails after triggering | Open Console Output and inspect the error |
| Git checkout fails | Verify Git installation and repository credentials |

---

# ✅ Day 10 Completion Checklist

- [ ] Created a Pipeline Job.
- [ ] Configured GitHub repository in Pipeline.
- [ ] Executed the Pipeline manually once.
- [ ] Configured Poll SCM.
- [ ] Tested automatic build after pushing GitHub changes.
- [ ] Configured Build Periodically.
- [ ] Tested scheduled build without GitHub changes.
- [ ] Practised Jenkins cron expressions.
- [ ] Understood the difference between Poll SCM and Build Periodically.
- [ ] Understood why SCM configuration is needed for repository-based polling.

---

## 📌 Next – Day 11 : Jenkins Learning Finale

**🔔 GitHub Webhooks & Automatic Build Triggers**

In Day 11, we will configure GitHub Webhooks so that GitHub can notify Jenkins when a push event occurs.

We will learn:

- What Webhooks are.
- How GitHub sends push-event notifications to Jenkins.
- Jenkins GitHub integration.
- Webhook Payload URL and Content Type.
- Push event configuration.
- Webhook delivery verification.
- Automatic Jenkins build triggering.
- Poll SCM vs GitHub Webhook.

**Our goal:** Understand the difference between Jenkins periodically checking GitHub (Poll SCM) and GitHub notifying Jenkins when changes occur (Webhook).
