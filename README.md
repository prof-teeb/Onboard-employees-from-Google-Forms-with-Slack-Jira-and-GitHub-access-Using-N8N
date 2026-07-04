# 🚀 Automated Employee Onboarding Pipeline (Slack, Jira & GitHub)

An enterprise-grade **n8n orchestration workflow** designed to fully automate the digital onboarding lifecycle for new hires. The system triggers automatically upon a new **Google Form** submission, filters duplicate entries, provisions communication channels via **Slack**, handles engineering repository access via **GitHub**, creates structured tracking tasks in **Jira**, and updates database records in **Google Sheets**.

---

## 🛠️ System Architecture & Logic Blocks

<img width="1314" height="465" alt="Onboard employees from Google Forms with Slack, Jira, and GitHub access" src="https://github.com/user-attachments/assets/fb059e59-ec77-4363-aa13-a2abbd479928" />


The pipeline is split into two foundational operational zones to maintain high data integrity and flawless execution:

### 1. 🗂️ Sorting Office (Data Processing & Validation)
* **Real-time Trigger:** Automatically polls the Google Sheets backend linked to your Google Form every minute looking for new employee entries.
* **Dynamic Structural Mapping:** A specialized JavaScript execution block dynamically maps departments (`Software`, `Marketing`, `Finance`, `Project`) and functional teams (`FrontEnd`, `BackEnd`, `Mobile`, `UI/UX`, etc.) to distinct channel IDs, components, and target repositories.
* **Idempotency Check:** Evaluates if the current employee has already been onboarded by validating their tracking state (`Status == Completed`). If they have, it bypasses the system and alerts the Slack Administrator immediately to avoid double billing or credential duplication.

### 2. ⚡ Digital Onboarding (User Provisioning & Access)
* **Communications Integration:** Automatically adds the new hire to their respective departmental **Slack** channel based on the dynamic configuration.
* **Conditional Engineering Provisioning:** Conditional branches inspect the incoming payload. If the department is flagged as `Software`, an API call routes straight to **GitHub** to add the user to their designated repository with `push` permissions.
* **Project Management Setup:** Sends an invitation link to the employee's corporate email via the **Jira Cloud API** and seamlessly creates a personalized onboarding epic/task mapped to specific team components.
* **Database Finalization:** Writes back to the master **Google Sheet**, marking the user's status explicitly as `Completed`.

---

## ⚙️ Configuration Setup & Variables Mapping

To operationalize the blueprint within your infrastructure, update the environment blocks inside the workflow nodes:

### 1. Script Map Configuration (`Map department and team configuration`)
Modify the `config` object keys inside the JavaScript script node to match your team's real operational IDs:
* **Slack:** Replace `YOUR_SOFTWARE_SLACK_CHANNEL_ID`, etc., with actual target channel strings.
* **Jira Project Keys:** Map your exact functional boards (e.g., `YOUR_SOFTWARE_JIRA_PROJECT_KEY`).
* **GitHub Repositories:** Define specific code repositories inside the `teams` mapping array.

### 2. Global Node Placeholders
* **Google Sheets Trigger & Node:** Update `YOUR_GOOGLE_SHEET_ID_HERE` with your primary responses sheet URL identifier.
* **GitHub HTTP Request Node:** Update the endpoint path string `YOUR_ORG_NAME` to match your GitHub Organization profile name.
* **Jira HTTP Request Node:** Alter the invitation endpoint URL parameter `YOUR_SITE.atlassian.net` to point to your enterprise cloud domain.
* **Slack Admin Notice Node:** Set `YOUR_SLACK_ADMIN_USER_ID_HERE` to your designated IT workspace moderator.

---

## 🚀 Deployment Steps

1. **Import to Workspace:** Download the workflow JSON blueprint, open your n8n workspace canvas, choose **Import from File**, and upload this file.
2. **Configure App Credentials:** Connect and authenticate your respective credentials tokens for **Google Sheets**, **Slack**, **GitHub API**, and **Jira Software Cloud API**.
3. **Set Active:** Flip the workflow activation switch in the top-right corner to **Active** to start automating your onboarding operations.

