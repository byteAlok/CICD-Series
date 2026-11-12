# 🔔 Day 11 – GitHub Webhooks & Automatic Build Triggers

## 🎓 Completing the Jenkins Learning Journey – The Final Chapter!

## 🎯 Objective

The objective of this practical is to integrate GitHub Webhooks with Jenkins and configure automatic Pipeline execution whenever a push event occurs in the GitHub repository.

Instead of Jenkins periodically checking the repository for changes, GitHub will notify Jenkins whenever a configured event occurs.

In this practical, we will learn:

1. 🔔 Introduction to GitHub Webhooks.
2. ⚙️ How GitHub Webhooks work.
3. 🔄 Poll SCM vs GitHub Webhooks.
4. 🔌 Jenkins GitHub Plugin verification.
5. 🌐 Jenkins Webhook Endpoint.
6. 🔗 GitHub Repository Webhook configuration.
7. 🛠️ Jenkins Pipeline Job trigger configuration.
8. 🚀 Automatic build execution after GitHub push.
9. 📊 Webhook delivery verification.
10. 🔐 Webhook security considerations.
11. 🧰 Troubleshooting common issues.

---

# 1. 🧠 What is a Webhook?

A Webhook is a mechanism that allows one application to notify another application automatically when a particular event occurs.

In our case:

- GitHub is the event source.
- Jenkins is the event receiver.
- A GitHub push event triggers an HTTP POST request.
- Jenkins receives the notification and triggers the configured Pipeline Job.

### 🔍 Simple Example

Suppose we have a GitHub repository:

```text
https://github.com/byteAlok/CICD-Series
```

And Jenkins has a Pipeline Job connected to this repository.

Whenever we push new code to GitHub, GitHub sends a notification to Jenkins.

Jenkins receives that notification and automatically starts the Pipeline.

**We do not need to manually click Build Now.**

---

# 2. ⚙️ How GitHub Webhooks Work

Let's understand the complete workflow.

```text
Developer
    |
    | 1. Modify Code
    | 2. Git Commit
    | 3. Git Push
    |
    v
GitHub Repository
    |
    | 4. Push Event Generated
    |
    v
GitHub Webhook
    |
    | 5. HTTP POST Request
    |
    v
Jenkins Controller
    |
    | 6. Webhook Event Received
    | 7. Trigger SCM Polling
    |
    v
GitHub Repository
    |
    | 8. Check Configured SCM Changes
    |
    v
Jenkins Pipeline
    |
    | 9. Checkout Latest Changes
    | 10. Execute Jenkinsfile
    |
    v
Build Successful
```

### 📌 Step-by-Step Explanation

| Step | Description |
|---|---|
| 1 | Developer modifies application code. |
| 2 | Changes are committed locally. |
| 3 | Developer pushes changes to GitHub. |
| 4 | GitHub generates a push event. |
| 5 | GitHub sends an HTTP POST request to Jenkins. |
| 6 | Jenkins receives the webhook notification. |
| 7 | The GitHub integration triggers SCM polling for the configured job. |
| 8 | Jenkins checks the configured SCM repository and branch for changes. |
| 9 | Jenkins checks out the latest relevant revision. |
| 10 | Jenkins executes the Pipeline. |

**Important:** A webhook notification does not itself contain the complete application source code. Jenkins retrieves the required source code from the configured SCM repository.

---

# 3. 🔄 Poll SCM vs GitHub Webhooks

We studied Poll SCM in Day 10. Now let's understand how Webhooks are different.

### 🔄 Poll SCM

In Poll SCM, Jenkins periodically checks the configured GitHub repository to detect changes.

Example:

```cron
H/5 * * * *
```

This configures Jenkins to check for changes approximately every five minutes.

Workflow:

```text
Jenkins
   |
   | Periodically checks
   |
   v
GitHub Repository
   |
   v
Changes Detected?
   |
   +----------+
   |          |
  YES         NO
   |          |
   v          v
Build       No Build
```

### 🔔 GitHub Webhook

In Webhook integration, GitHub sends a notification to Jenkins when a configured event occurs.

Workflow:

```text
GitHub Repository
       |
       | Push Event
       |
       v
GitHub Webhook
       |
       | HTTP POST
       |
       v
Jenkins
       |
       v
Automatic Build
```

### 📊 Comparison

| Feature | Poll SCM | GitHub Webhook |
|---|---|---|
| Notification initiated by | Jenkins | GitHub |
| Repository checking | Periodically | Triggered by an event |
| Build trigger | SCM changes detected | Webhook event followed by SCM change detection |
| Waiting | Depends on polling interval | Usually near real-time |
| Cron schedule | Required | Not required |
| GitHub push event | Not required as a notification mechanism | Used to notify Jenkins |
| Network requirement | Jenkins must access GitHub | GitHub must reach Jenkins |
| Main purpose | Periodic change detection | Event-driven integration |

### 💡 Important Understanding

Poll SCM and Webhooks can both be used for SCM-based automatic builds.

However, when Webhooks are configured correctly, continuous periodic polling is generally unnecessary.

Webhook integration is more efficient for event-driven build triggering because Jenkins does not need to repeatedly check the repository on a fixed schedule.

---

# 4. 🔌 Verify GitHub Plugin in Jenkins

Before configuring Webhooks, we need to verify that the required GitHub integration plugin is installed.

### 📍 Navigation

```text
Jenkins Dashboard
       |
       v
Manage Jenkins
       |
       v
Plugins
       |
       v
Installed Plugins
```

1. Open Jenkins Dashboard.
2. Click **Manage Jenkins**.
3. Open **Plugins**.
4. Select **Installed Plugins**.
5. Search for `GitHub`.

### ✅ Plugins Verified

The following plugins were already installed and enabled in our Jenkins environment:

| Plugin | Status |
|---|---|
| GitHub API Plugin | Enabled |
| GitHub Branch Source Plugin | Enabled |
| GitHub Plugin | Enabled |
| Pipeline: GitHub Groovy Libraries | Enabled |

**No additional plugin installation was required for this practical.**

---

# 5. 🌐 Understand the Jenkins Webhook Endpoint

GitHub needs a URL where it can send webhook notifications.

The standard endpoint provided by the Jenkins GitHub plugin is:

```text
http://YOUR-JENKINS-PUBLIC-IP:8080/github-webhook/
```

### 📌 Example

```text
http://YOUR-PUBLIC-IP:8080/github-webhook/
```

### 🔍 Endpoint Breakdown

| Component | Meaning |
|---|---|
| `http://` | Communication protocol |
| `YOUR-PUBLIC-IP` | Public IP address of Jenkins EC2 |
| `8080` | Jenkins default port in our setup |
| `/github-webhook/` | GitHub plugin webhook endpoint |

### 🌍 Our Jenkins Environment

Jenkins was running on an AWS EC2 instance and was accessible through its public IP.

The Jenkins installation was accessible using:

```text
http://YOUR-JENKINS-PUBLIC-IP:8080/
```

Therefore, our webhook endpoint followed this format:

```text
http://YOUR-JENKINS-PUBLIC-IP:8080/github-webhook/
```

### ⚠️ Network Requirements

For GitHub to deliver webhook notifications:

- Jenkins must be running.
- Jenkins must be accessible over the network.
- The webhook endpoint must be reachable from GitHub.
- The AWS Security Group must allow the required inbound connection.
- The operating system firewall and network configuration must not block the request.

**Our learning environment:** The EC2 Security Group had an inbound All Traffic rule with source `0.0.0.0/0`, so the required Jenkins port was reachable.

> ⚠️ This broad inbound rule is only a temporary lab configuration. It should not be used in production.

---

# 6. 🔗 Create a Webhook in GitHub

Now we will configure GitHub to send push-event notifications to Jenkins.

### 📍 Navigation

```text
GitHub
   |
   v
Open Repository
   |
   v
Settings
   |
   v
Webhooks
   |
   v
Add webhook
```

### Step 1 – Open Repository

Open the GitHub repository:

```text
https://github.com/byteAlok/CICD-Series
```

### Step 2 – Open Webhooks

1. Click **Settings** in the repository navigation.
2. Find **Webhooks** in the left sidebar.
3. Click **Webhooks**.
4. Click **Add webhook**.

### Step 3 – Configure Webhook

GitHub will display the Add webhook form.

Configure the following fields:

| Field | Value |
|---|---|
| Payload URL | `http://YOUR-JENKINS-PUBLIC-IP:8080/github-webhook/` |
| Content type | `application/json` |
| Secret | Blank for our initial learning test |
| SSL verification | Disabled for our temporary HTTP setup |
| Which events? | Just the push event |
| Active | Checked |

### 🔍 Understanding Each Field

**1. Payload URL**

This is the Jenkins endpoint where GitHub sends webhook notifications.

```text
http://YOUR-JENKINS-PUBLIC-IP:8080/github-webhook/
```

**2. Content Type**

We selected:

```text
application/json
```

This tells GitHub to send the webhook payload in JSON format.

**3. Secret**

The secret is used to authenticate webhook payloads through a signature.

For our initial practical, we left this field blank.

For a secured setup, configure a strong secret in GitHub and configure the corresponding secret verification on the Jenkins receiving side. Do not assume that simply entering a secret in GitHub automatically enables verification in Jenkins.

**4. SSL Verification**

Our Jenkins endpoint was using HTTP instead of HTTPS.

Therefore, we selected:

```text
Disable (not recommended)
```

SSL certificate verification applies to HTTPS connections, not plain HTTP.

**5. Which Events?**

We selected:

```text
Just the push event.
```

This means GitHub sends webhook notifications when a push event occurs.

**6. Active**

Keep the Active checkbox enabled.

### Step 4 – Create Webhook

After entering all the settings:

1. Verify the Payload URL.
2. Verify Content Type.
3. Verify the Push event selection.
4. Verify Active is checked.
5. Click **Add webhook**.

### ✅ Expected Result

GitHub displays a success message:

```text
Okay, that hook was successfully created.
We sent a ping payload to test it out!
```

This confirms that GitHub successfully created the webhook and attempted its initial ping delivery.

---

# 7. 🛠️ Configure Jenkins Pipeline Job

Creating a webhook in GitHub is not sufficient by itself.

We also need to enable the appropriate trigger in the Jenkins Pipeline Job.

### 📍 Navigation

```text
Jenkins Dashboard
       |
       v
Open Pipeline Job
       |
       v
Configure
       |
       v
Build Triggers
```

### Step 1 – Open Pipeline Job

We reused the Pipeline Job created during Day 10.

```text
Job Name:
Day-10-Poll-SCM
```

### Step 2 – Open Configuration

1. Open Jenkins Dashboard.
2. Click the Pipeline Job.
3. Click **Configure**.
4. Scroll down to **Build Triggers**.

### Step 3 – Enable GitHub Trigger

Find and enable:

```text
GitHub hook trigger for GITScm polling
```

This option allows the Jenkins GitHub integration to trigger SCM polling when the corresponding webhook event is received.

5. Click **Save**.

### ⚠️ Important Difference

Do not confuse these two options:

| Jenkins Trigger | Purpose |
|---|---|
| Poll SCM | Periodically checks the repository using a configured schedule. |
| GitHub hook trigger for GITScm polling | Triggers SCM polling when a GitHub webhook event is received. |

For our Day 11 practical, we enabled:

```text
GitHub hook trigger for GITScm polling
```

We did not need to keep the Poll SCM schedule enabled for this webhook test.

---

# 8. 🚀 Test Automatic Jenkins Build

Now we will test the complete integration.

### Step 1 – Modify Repository Code

Open the local clone of:

```text
CICD-Series
```

Modify any tracked file or add a new file.

For example:

```text
README.md
```

### Step 2 – Commit and Push

Execute:

```bash
git add .
git commit -m "Test GitHub Webhook"
git push origin main
```

### Step 3 – GitHub Generates Push Event

After the push succeeds:

1. GitHub receives the new commit.
2. GitHub generates a push event.
3. The configured webhook sends an HTTP POST request to Jenkins.

### Step 4 – Jenkins Receives Notification

Jenkins receives the webhook request through:

```text
/github-webhook/
```

The GitHub integration triggers SCM polling for the configured Pipeline Job.

### Step 5 – Automatic Build Execution

If Jenkins detects relevant changes in the configured SCM branch, it automatically starts a new build.

We do not need to click:

```text
Build Now
```

### ✅ Expected Result

A new build should appear automatically in Jenkins Build History.

---

# 9. 📊 Verify Webhook Delivery in GitHub

We can verify whether GitHub successfully delivered the webhook notification.

### 📍 Navigation

```text
GitHub Repository
       |
       v
Settings
       |
       v
Webhooks
       |
       v
Select Configured Webhook
       |
       v
Recent Deliveries
```

### What is Recent Deliveries?

Recent Deliveries displays the webhook requests sent by GitHub.

It provides information such as:

- Event type.
- Delivery ID.
- Request details.
- Response status.
- Response headers.
- Response body.
- Delivery time.

### Important Event Types

**1. Ping Event**

GitHub sends a ping event when a webhook is created to test the endpoint.

**2. Push Event**

GitHub sends a push event when a push occurs in the repository.

### Expected Result

A successfully delivered event should show a green success indicator.

Open the latest `push` event to inspect its delivery details and HTTP response.

> A successful HTTP delivery confirms that GitHub reached the webhook endpoint and received a response. It does not by itself prove that Jenkins executed the intended Pipeline successfully. Always verify Jenkins Build History too.

---

# 10. 🧪 Our Actual Practical Result

We successfully configured GitHub Webhooks with Jenkins and verified automatic Pipeline execution.

### 🔍 Jenkins Build Verification

Our Jenkins Pipeline generated:

| Field | Actual Result |
|---|---|
| Build Number | `#17` |
| Trigger | Started by GitHub push by byteAlok |
| Repository | `https://github.com/byteAlok/CICD-Series.git` |
| Branch | `refs/remotes/origin/main` |
| Commit | `d5da6ec` |
| Build Status | SUCCESS |

### Important Console/Build Information

Jenkins displayed:

```text
Started by GitHub push by byteAlok
```

This confirmed that the build was started through the GitHub push-trigger integration rather than manually using Build Now.

### 🔍 GitHub Recent Deliveries Verification

We also verified the GitHub Webhook's Recent Deliveries page.

| Event | Delivery Status |
|---|---|
| Ping | Successfully delivered |
| Push | Successfully delivered |

Both events showed successful delivery indicators.

### 🎉 Final Result

```text
GitHub Push
     |
     v
Webhook Notification
     |
     v
Jenkins Trigger
     |
     v
Pipeline Build #17
     |
     v
SUCCESS
```

**GitHub Webhook integration was successfully tested.**

---

# 11. 🌿 Which Branch Changes Trigger the Build?

An important point is that GitHub Webhooks are configured at the repository level, while Jenkins SCM configuration determines which branch the Pipeline Job checks out.

Our Pipeline Job was configured with:

```text
Branch Specifier:
*/main
```

Therefore:

- A push to `main` can trigger the configured Pipeline when relevant SCM changes are detected.
- A push to another branch may still generate a GitHub push webhook event.
- However, the Jenkins Job is configured to build from `main`, not automatically from every other branch.

### 📌 Example

| Repository Change | Webhook Event | Main-branch Job |
|---|---|---|
| Update README on main | Yes | Can trigger build |
| Add a file on main | Yes | Can trigger build |
| Delete a file on main | Yes | Can trigger build |
| Push changes to another branch | Yes | Does not normally trigger this main-only job |

The exact result also depends on SCM branch configuration, change detection and plugin behavior.

**Important:** A webhook event is not the same thing as a guaranteed Pipeline execution. Jenkins must detect relevant changes for the configured job.

---

# 12. 🧰 Troubleshooting

| Problem | Possible Cause | Solution |
|---|---|---|
| GitHub webhook delivery fails | Jenkins endpoint unreachable | Check public IP, port and network access |
| Connection refused | Jenkins is stopped or port is not accessible | Verify Jenkins service and security rules |
| Webhook returns an error | Incorrect endpoint URL | Verify `/github-webhook/` path |
| GitHub delivery succeeds but no build starts | Jenkins trigger is not enabled | Enable GitHub hook trigger for GITScm polling |
| Build does not start for another branch | Job monitors only `main` | Verify Branch Specifier |
| Build starts but fails | Pipeline or application error | Inspect Console Output |
| Git checkout fails | Incorrect repository configuration or credentials | Verify SCM URL and credentials |
| Webhook works only sometimes | Network or Jenkins availability issue | Inspect Recent Deliveries and Jenkins logs |

### 🔍 Useful Jenkins Verification

Open the Pipeline Job and check:

```text
Build History
    |
    v
Latest Build
    |
    v
Console Output
```

Also inspect:

```text
GitHub Repository
    |
    v
Settings
    |
    v
Webhooks
    |
    v
Recent Deliveries
```

These two locations help identify whether the problem occurred during webhook delivery or Pipeline execution.

---

# 13. 🔐 Security Best Practices

Our practical used HTTP and a broadly open AWS Security Group for learning purposes. This is not a recommended production configuration.

For a production environment:

1. **Use HTTPS:** Protect webhook communication with TLS.
2. **Enable Signature Verification:** Configure a strong webhook secret and verify the GitHub signature on the receiving side.
3. **Restrict Network Access:** Avoid `All traffic` from `0.0.0.0/0`.
4. **Use a Secure Ingress:** Place Jenkins behind a properly configured reverse proxy, load balancer or other secure ingress.
5. **Protect Jenkins:** Use authentication, authorization and appropriate security settings.
6. **Protect Credentials:** Never commit PATs, passwords or webhook secrets into GitHub repositories.
7. **Monitor Deliveries:** Regularly inspect webhook delivery failures and Jenkins logs.

### ⚠️ Our Lab Cleanup

After completing the practical:

- Replace the broad EC2 Security Group rule with only the required inbound access.
- Do not leave Jenkins publicly exposed over plain HTTP.
- Configure HTTPS before using the setup for real projects.
- Keep webhook secrets and credentials protected.

---

# 14. 🏁 Jenkins Learning Finale – Day 01 to Day 11

With Day 11 completed, we have successfully finished the planned Jenkins learning series, covering Jenkins fundamentals, Pipeline development, distributed builds, GitHub integration and automated build triggers.

### 📚 Learning Summary

| Day | Topic | Status |
|---|---|---|
| Day 01 | Jenkins Installation and Configuration | ✅ Completed |
| Day 02 | Declarative Pipeline and Post Actions | ✅ Completed |
| Day 03 | Input and Manual Approval | ✅ Completed |
| Day 04 | Timeout and Error Handling | ✅ Completed |
| Day 05 | Jenkins Controller-Agent Architecture | ✅ Completed |
| Day 06 | Jenkins Inbound Agent Setup | ✅ Completed |
| Day 07 | Jenkins SSH Agents and Parallel Builds | ✅ Completed |
| Day 08 | Scripted Pipeline and Groovy | ✅ Completed |
| Day 09 | Jenkinsfile and Pipeline as Code | ✅ Completed |
| Day 10 | Poll SCM and Build Triggers | ✅ Completed |
| Day 11 | GitHub Webhooks and Automatic Build Triggers | ✅ Completed |

### 🎯 Learning Achievements

Throughout these 11 days, we covered:

- ⚙️ Jenkins installation, configuration and service management.
- 🔄 Declarative and Scripted Pipelines.
- 📝 Jenkinsfile and Pipeline as Code.
- 🖥️ Controller-Agent architecture and distributed builds.
- 🔐 Jenkins credentials and GitHub integration.
- 🚀 Parallel execution and multi-agent builds.
- ⏰ Build triggers, cron scheduling and Poll SCM.
- 🔔 GitHub Webhooks and automatic build execution.

All the topics were supported by practical exercises and hands-on configurations.

---

# 🚀 What's Next? – Day 12: Jenkins Practical Projects

The learning phase is now complete. It's time to apply the concepts we have learned by building practical Jenkins projects.

In the next phase, we will focus on hands-on implementation, real-world CI/CD workflows and practical automation.

**Learning Phase: Day 01–11 – Completed ✅**

**Next Phase: Day 12 – Jenkins Practical Projects 🚀**
