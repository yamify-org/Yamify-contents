# Step-by-step n8n workflow to automate the Yamify Hackathon registration and onboarding process.
#

### 🧩 n8n Workflow: Hackathon Registration & Onboarding

#### 🔘 Trigger Node

Trigger: Google Sheets → New Row Added

Purpose: Detect new participant form submissions

Connect your Google Sheet containing responses from your registration form

Use columns like: Name, Email, Company, Growth Stage, Tech Needs
#

#### 📤 Step 1: Send Welcome Email

Node: Gmail (or SMTP Email)

Use templated email with:


**Subject: Welcome to Yamify Hackathon** 🌍🚀

Hi {{Name}},

Thanks for registering for the Yamify Hackathon – Ocean & NASA Editions!

Here are some important links:

🧠 Yamify AI Platform: https://bit.ly/Yamify-hack-fr
📅 Prep Workshop: Thursday, July 3
🛠️ Hackathon Guide: 

We’re excited to build the future with you!

– Yamify Team
#

### 📊 Step 2: Add to Airtable (or Update Sheet)

Node: Airtable (or Google Sheets again)

Action: Insert or update the user’s full profile into a Participant Database

Fields: Name, Email, Company, Growth Stage, Tags
#

### 🏷️ Step 3: Tag Based on Growth Stage / Tech Needs

Node: IF / Switch Node

Conditions:

If Growth Stage = “Startup” → Tag = “Startup”

If Tech Needs contains “AI” → Tag = “AI Interest”

Else → Tag = “General”
#

### 🧠 Step 4: Call Yamify API (Optional)

Node: HTTP Request

Method: POST

URL: https://api.yamify.com/provision-ai-access (example)

Headers: Auth token

Body:

{
  "email": "{{$json["Email"]}}",
  "access_level": "trial",
  "event": "hackathon"
}

Purpose: Auto-provision AI credits for participants.
#

### 🔁 Step 5: Schedule Reminder Emails

Node: Wait + Email

Wait 2 days → Send prep reminder

Wait 7 days → Send feedback form

You can use n8n’s "Wait" node to delay email sending

### 🔄 Optional: Slack Auto-Invite

Node: HTTP POST to Slack API

Automate Slack invite for new users
#

### 🔧 Final Flow Overview:

[Google Sheet Trigger]
      ↓
[Send Welcome Email]
      ↓
[Update Airtable]
      ↓
[Check Tags → Branching]
      ↓
[Provision Yamify AI Access (optional)]
      ↓
[Send Follow-up Emails (delayed)]

