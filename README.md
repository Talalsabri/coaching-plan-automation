<div align="center">

🇬🇧 **English** | 🇸🇦 [العربية](README.ar.md)

# Coaching Plan Automation

**Payment in, personalised training plan out.**
AI drafts it, the coach approves it twice, the trainee gets a branded PDF.

<sub>Built for **Mosabri | موصبري**, an online running coach.</sub>

<img src="assets/n8n-workflow-en.png" alt="The full n8n workflow: purchase, survey, Claude, text to PDF, send, and a missing-email guard" width="100%">

</div>

## The Problem

An online running coach was selling plans to a growing number of trainees. Every plan meant checking the payment, chasing the questionnaire, writing the plan, designing a PDF, emailing it and remembering who is on which week. All by hand.

## The Solution

One pipeline from payment to inbox. Claude drafts each plan from the trainee's answers and the coach's own rules. The coach edits it in Notion, approves the text, then approves the generated PDF. Only then does it send.

## How It Works

<div align="center">
<img src="assets/architecture-en.png" alt="Swimlane diagram: trainee, automation and coach across nine steps from payment to delivery" width="720">
</div>

The coach never leaves Notion. Each plan is a row whose **status** drives the pipeline, and schedulers pick up every status change within minutes.

<div align="center">
<img src="assets/status-flow-en.png" alt="Status flow with two approval gates and three ways to request a change" width="860">
</div>

## Key Features

### Intake runs itself
A payment webhook filters paid packages and first-time buyers, then emails the survey. The survey answers become a new plan request and a new Notion page.

<img src="assets/n8n-purchase-survey-en.png" alt="Purchase and survey stages" width="100%">

<div align="center">
<img src="assets/results-email-survey.png" alt="The survey email a new trainee receives" width="720">
</div>

### Four ways to write a plan
New (from the survey), **revise** (applies only the coach's note and copies the rest word for word), **weekly update** (progressive load) and **regenerate** (rebuilds from edited trainee data). One shared block holds the coach's rules.

<img src="assets/n8n-claude-en.png" alt="The Claude stage" width="100%">

### What the coach approves is what is sent
The PDF is rebuilt from the approved text, one file per part (training, prep, nutrition). A late edit can never be missed.

<img src="assets/n8n-text-to-pdf-en.png" alt="Text to PDF stage" width="100%">

### Two gates and guards against silent failure
Nothing reaches a trainee before both approvals. A missing email sends the file back for approval, an in-progress status prevents double processing, and error, infrastructure and uptime alerts reach the coach by email.

<img src="assets/n8n-send-en.png" alt="Send stage and missing-email guard" width="100%">

### Never stops silently
An external monitor checks the system every 5 minutes and emails the team the moment it stops responding.

<div align="center">
<img src="assets/monitoring-en.png" alt="Uptime monitor: HTTP check every 5 minutes, 100% uptime, response-time chart" width="640">
</div>

### Safety rules built in
Injuries are never diagnosed, food allergies are excluded, and beginners get a run/walk program instead of a race plan.

## Tech Stack & Tools

| Layer | Tool |
|---|---|
| Orchestration | n8n, self-hosted |
| AI | Claude API Key |
| Payments | Whop webhooks |
| Survey | Tally |
| Workspace and database | Notion |
| PDF | Gotenberg + a custom slide renderer |
| Email | Gmail |
| Hosting | Railway (n8n, Postgres, renderer services) |
| Monitoring | UptimeRobot + an n8n error workflow |

## Results

<div align="center">
<img src="assets/results-stats-en.png" alt="52 nodes, 5 stages, 2 approval gates, 4 plan modes, 20+ plans imported" width="860">
</div>

Purchase → survey → plan → PDF → email, verified end to end for both new and returning trainees. No plan is sent without both approvals, by design.

**The coach's board in Notion**

<div align="center">
<img src="assets/results-notion-table.png" alt="Notion board of trainees with package, week and status" width="860">
</div>

**An AI-drafted plan with its reasoning, ready to edit**

<div align="center">
<img src="assets/results-notion-notes.png" alt="Notion plan page with the AI's notes on its decisions" width="520">
</div>

**What the trainee receives**

<div align="center">
<img src="assets/results-pdf.png" alt="The branded PDF: cover and contents pages" width="460">
<br><br>
<img src="assets/results-email-plan.png" alt="The delivery email with the PDF attached and a WhatsApp button" width="720">
</div>

<sub>Plans are written in Arabic. Trainee names, contact details and credentials are blurred or excluded.</sub>
