# elastic-siem-alert-automation
A cybersecurity automation project using Elastic SIEM and Tines for alert generation and response.

# Elastic SIEM Alert Automation

This project showcases a cybersecurity automation pipeline using *Elastic SIEM* and *Tines*. It demonstrates how device logs are ingested, correlated using detection rules, and how alerts are automated for analyst review and escalation.

> 📺 *Based on Tutorial:* "Try this Cybersecurity A.I Project!!" by Andrew Jones

---

## 📌 Project Objective

To build a Security Information and Event Management (SIEM) solution that:
- Collects logs from devices in a network
- Uses Elastic SIEM to detect suspicious activity
- Sends alerts to analysts via Tines
- Enables auto-escalation or rule refinement using automation

### 🖥 Step 1: Created Windows Test Server on AWS EC2

To simulate a real enterprise environment and generate security logs, I launched a *Windows Server EC2 instance* on AWS.

- Region: us-west-2 (Oregon)
- Instance Type: t3.micro (Free Tier)
- OS: Windows Server
- Purpose: This endpoint will act as a log source for Elastic SIEM

📸 Screenshot:

![Screenshot of Windows EC2 instance](docs/step1_aws_ec2_windows.jpg)

### 🖥 Step 2: Connected to Windows Server via Remote Desktop (RDP)

After launching the Windows EC2 instance, I established a *Remote Desktop Connection* to configure it as a log source.

🔐 This step allows access to:
- Install and configure the *Elastic Agent*
- Monitor local services and security logs
- Simulate endpoint behavior (for Elastic SIEM)

*Connection Info:*
- RDP IP: Public IPv4 address
- Username: Administrator
- Password: Decrypted using .pem key from AWS console
- Instance Type: t3.micro
- Availability Zone: us-west-2b

📸 Screenshot:

![Step 2 - RDP Windows Server](docs/step2_rdp_windows_server.jpg)

---

### ⚙ Step 3: Set Up Elastic Cloud Security & Installed Elastic Agent

After connecting to the Windows EC2 instance, I signed up for a *free trial of Elastic Cloud* with built-in Security features, which include:

#### 🛡 Elastic Security Capabilities Enabled:
- *Logs* – Collect, search, and analyze security logs from endpoints.
- *SIEM* – Detect, investigate, and respond to evolving threats.
- *Endpoint Security* – Protect hosts against malware, ransomware, and exploits via Elastic Agent.
- *Cloud Protection* – Assess cloud posture and defend workloads.

---

### ✅ Installed Elastic Agent on Windows Server

I installed the *Elastic Agent* on the Windows EC2 instance using PowerShell and enrolled it into my Elastic Cloud instance using the secure enrollment token.

📸 Screenshot:

![Step 3 - Elastic Installation Windows Server](docs/Step3_elastic_install_windows.jpg)

#### 📝 Installation Commands:
```powershell
$ProgressPreference = 'SilentlyContinue'
Invoke-WebRequest -Uri https://artifacts.elastic.co/downloads/beats/elastic-agent/elastic-agent-9.0.3-windows-x86_64.zip -OutFile elastic-agent.zip
Expand-Archive .\elastic-agent.zip -DestinationPath .
.\elastic-agent.exe install --url=<elastic-cloud-url> --enrollment-token=<your-token>
---
```
### 🛡 Step 4: Integrated Elastic Defend (EDR) for Endpoint Monitoring

To enhance host-level security visibility, I integrated *Elastic Defend* with the Windows EC2 server through the *Elastic Agent Policy*.

Elastic Defend acts as an *Endpoint Detection & Response (EDR)* system, allowing:
- Real-time detection of malware, ransomware, and exploits
- Process-level telemetry and file monitoring
- Response capabilities against endpoint threats

---

### 🧩 Integration Steps:

1. Navigated to *Fleet → Agent Policies* in Elastic Cloud
2. Selected *Agent policy 1* linked to my enrolled Windows server
3. Clicked *Add integration* and chose *Elastic Defend*
4. Selected *Complete EDR (Endpoint Detection & Response)* for full visibility
5. Named the integration Project1 and applied it to the policy
6. Confirmed that the policy now contains both:
   - system-1 (basic telemetry)
   - Project1 (Elastic Defend v9.1.0)

---

📸 *Screenshots:*

- Add Integration to policy   
  ![Add Integration to policy 1](docs/step4_Add_integration.jpg)

- Integration Agent selection 
  ![Select Elastic defend](docs/step4_select_elastic_defend.jpg)

- Configuring Integration as "Project1"  
  ![Config Integration](docs/step4_integration_name.jpg)

- Integration Role(complete EDR)  
  ![Select Complete EDR option](docs/step4_select_edr_options.jpg)

- Elastic Defend Successfully added  
  ![Root Privilege Required](docs/step4_integration_added.jpg)

---

### 🤖 Step 5: Built Workflow Automation in Tines for Alert Summarization & Email Notification

To automate alert response and bring AI into the pipeline, I integrated *Tines* with Elastic SIEM.

#### 🔧 What is Tines?
Tines is a no-code automation platform used to trigger workflows in response to incoming webhook data — such as alerts from Elastic.

---

### 🧩 What I Did:

1. *Signed up for a free Tines account*
   - Used Google Sign-In for quick setup.
2. *Created a new Story (Workflow Automation)* inside the Tines platform
3. Dragged in the following components:
   - ✅ Webhook Action: to receive alert data
   - ✅ AI → Summarize Webhook Data: to interpret and reduce raw alert noise
   - ✅ Send Email Action: to notify via email with a clean summary
4. Connected the blocks into a flow:
   - Webhook → AI → Email
5. Configured the email action with:
   - Recipient: mandongamusas097@gmail.com
   - Sender Name: Musa Mandonga

---

📸 *Screenshots:*

- *Tines Story Overview*  
  ![Tines Story Overview](docs/step5_tines_story_overview.jpg)

  - *Webhook Configuration*  
  ![Webhook Config](docs/step5_webhook_config.jpg)

- *AI Summarize Block Setup*  
  ![AI Summarize](docs/step5_ai_summarize_block.jpg)

- *Send Email Action Setup*  
  ![Send Email Config](docs/step5_email_config.jpg)


### 🔗 Step 6: Connect Elastic to Tines via Webhook

To enable automated alert forwarding from Elastic Security to Tines, I configured a *Webhook connector* in Elastic Stack Management.

*Purpose:*  
Forward detection rule alerts (e.g., "Admin Logged In") from Elastic SIEM to Tynes for further automation or workflow triggers.

---

#### ✅ Steps Taken:

1. Navigated to *Stack Management > Connectors* in the Elastic Cloud dashboard.

   ![Connectors page in Elastic](docs/elastic-connectors-page.png)

2. Clicked *Create connector* and selected the *Webhook* option:
   - Gave the connector a name: test-tynes-webhook
   - Set method to POST
   - Entered the *Tynes webhook URL*
   - Selected *None* under authentication

   ![Webhook connector configuration](docs/webhook-connector-settings.png)

3. Clicked *Save & test* to confirm the connection between Elastic and Tynes.


### ✅ Step 7: Create and Test Detection Rule to Trigger Alert via Webhook

In this final step, I created a custom *SIEM Detection Rule* in Elastic to simulate a real-world alert and test the full end-to-end flow to Tines and email notifications.

---

#### 🛠 Rule Creation Process

1. *Navigate To:*
   - Security → Rules → Detection Rules (SIEM) → *Create new rule*

2. *Define Rule*
   - Rule type: Custom query
   - Query:  
     kql
     event.code: "4672"
     
   - Index patterns:
     auditbeat-*, winlogbeat-*, elastic-cloud-logs-*, etc.

   ![Rule Definition](docs/rule-definition.png)

3. *About Rule*
   - *Name:* Admin Logged In
   - *Description:* Monitoring admin logged in sessions
   - *Severity:* High  
   - *Risk Score:* 73

   ![Rule About Settings](docs/rule-about.png)

4. *Schedule Rule*
   - Runs every 5 minutes
   - Additional look-back time: 1 minute

   ![Rule Schedule Settings](docs/rule-schedule.png)

5. *Actions*
   - Webhook connector: Project1 (Tines webhook)
   - Payload:
     json
     {
       "rule_name": "{{context.rule.name}}",
       "description": "{{context.rule.description}}"
     }
     

   ![Rule Actions - Webhook](docs/rule-actions.png)

---

#### 🚨 Alert Result

To simulate a detection, I manually triggered an *admin login event*:

1. Disconnected RDP session from Windows EC2 instance  
2. Reconnected to the RDP session (generating event.code: 4672)

- The alert was successfully captured in Elastic SIEM under the *"Admin Logged In"* rule.

  ![Elastic Alert Fired](docs/alert-fired.png)

- The alert was forwarded to *Tines, processed by the **AI agent, and a **summarized alert email* was delivered.

  ![Alert Email from Tines AI](docs/alert-email-summary.png)

---

#### 🤖 AI Integration with Tines

The webhook payload from Elastic was received by Tines. An AI action was configured to:

- *Summarize the log* into analyst-friendly text
- *Generate a suggested action*
- *Send a formatted email alert* to designated recipients

This closes the full loop from detection → enrichment → notification.

---

🎉 *Test successful*: The system is now capable of real-time threat detection, automated triage via AI, and alerting via email.
