# AI Lead Qualification Agent

An autonomous, event-driven B2B lead triage and qualification system built with **n8n**, **Google Gemini**, **LangChain Agent**, **Google Sheets CRM**, and **Gmail**.

This automated pipeline ingests inbound leads via webhooks, normalizes intake payloads, executes multi-factor qualification using Google Gemini (scoring leads 0–100, assigning buying stage classifications and priority tiers), logs enriched audit trails into a Google Sheets CRM, and dispatches real-time sales alert notifications for high-intent opportunities.

---

## Overview

Inbound sales pipelines frequently suffer from manual qualification delays, inconsistent lead scoring, and delayed outreach to high-value prospects. Generic contact forms leave sales teams wading through low-budget inquiries and unverified personal email submissions.

This pipeline automates instant lead analysis and qualification:

1. **Instant Webhook Ingestion:** Listens for inbound prospect submissions via HTTP POST payloads (`/new-lead`).
2. **Payload Normalization:** Sanitizes contact information, project budget, and service requirements.
3. **AI Qualification & Scoring Engine:** Leverages Google Gemini via LangChain agent architecture to analyze intent, calculate a numeric lead score (0–100), classify buying status (SQL / MQL / Disqualified), determine engagement priority, and write consultative sales notes.
4. **Automated CRM Logging:** Appends fully enriched records with millisecond timestamps into a centralized Google Sheets CRM (`AI Lead CRM`).
5. **Conditional Sales Escalation:** Evaluates lead quality threshold (`Lead Score >= 40`) and instantly dispatches responsive HTML email alerts to the sales team for qualified accounts.

---

## Workflow Architecture

```text
Inbound Lead Submission (Webhook: POST /new-lead)
                 ↓
      Prepare Lead Data (Parameter Normalization)
                 ↓
        [AI Sales Qualification Agent]
          (Google Gemini Chat Model)
                 ↓
       Parse AI Output (JavaScript Code Node)
  [Extracts: lead_score | qualification | priority | notes]
                 ↓
      Deterministic Gate: If (lead_score >= 40)
            ┌────┴────┐
         [True]    [False]
            │         │
      ┌─────┴─────┐  (Filtered Out - No Append / Alert)
      │           │
      ↓           ↓
Append Row     Send a Message
 in Sheet         (Gmail)
(Google Sheets)

```
---

## Technical Highlights & Key Features

- **Event-Driven Webhook Ingestion:** Handles incoming lead requests asynchronously with instant `200 OK` responses, passing raw payloads into normalization nodes.
- **JSON Sanitization & Error Handling:** Custom JavaScript normalization removes Markdown formatting and code fences from LLM responses, ensuring strict JSON schema compliance before downstream operations.
- **Centralized CRM Ledger:** Appends complete prospect records directly into Google Sheets with dynamic unique lead IDs and execution timestamps.
- **Automated Sales Outreach Alerts:** Formatted HTML notification emails dispatched via Gmail OAuth2, alerting sales representatives with key discovery insights for immediate outreach.

### Multidimensional AI Sales Scoring

| Output | Description |
|--------|-------------|
| **Lead Score** | 0–100 index based on budget, service alignment, and company profile |
| **Qualification Tier** | SQL (Sales Qualified Lead), MQL (Marketing Qualified Lead), or Disqualified |
| **Priority Classification** | High, Medium-High, Medium, or Low |
| **Strategic AI Notes** | Automated discovery analysis identifying budget realism, domain authority, and follow-up recommendations |

---

## Structured Data Contract

**Lead Qualification Output Schema**

```json
{
  "lead_score": 85,
  "qualification": "Qualified (SQL)",
  "priority": "High",
  "notes": "The lead shows high intent by specifying a particular tool (n8n). The budget of 80,000 is substantial for automation workflows. Use of a personal Gmail account suggests an SMB or early-stage founder, but the 'Tech Solutions' company name implies a relevant industry. Immediate follow-up recommended to define project scope and technical requirements."
}
```

---

## Technology Stack

- **Orchestration:** n8n (webhook triggers, LangChain agent integration, conditional branching)
- **AI Core:** Google Gemini Chat Model (`@n8n/n8n-nodes-langchain.lmChatGoogleGemini`)
- **Agent Architecture:** LangChain Agent (`@n8n/n8n-nodes-langchain.agent`)
- **Data Transformation:** Custom JavaScript (Node.js engine)
- **CRM Database:** Google Sheets API (Service Account authentication)
- **Email Dispatch:** Gmail API (OAuth2 integration)

---

## Evidence & Verified Visual Proof

### 1. n8n Workflow Architecture & Execution Canvas

**Full Workflow Canvas (Editor)**

![Workflow Canvas](Screenshot%202026-07-01%20164356.png)

**Active Execution Engine (Live Runs)**

![Active Execution](Screenshot%202026-07-01%20210723.png)

---

### 2. Live CRM Synchronization (Google Sheets AI Lead CRM)

**Centralized Lead Database with AI Qualification & Strategic Notes**

![Google Sheets CRM Audit](Screenshot%202026-09-17%20175931.png)

---

### 3. Automated Sales Alert Notifications (Gmail)

**Qualified Lead Alert: Amit Verma (Score: 85)**

![Amit Verma Alert](Screenshot%202026-09-17%20172530.png)

**Qualified Lead Alert: Karan Malhotra (Score: 78)**

![Karan Malhotra Alert](Screenshot%202026-09-17%20172459.png)

**Enterprise AI Lead Alert: Ananya Mehra (Score: 68)**

![Ananya Mehra Alert](Screenshot%202026-07-01%20205850.png)
---

## Data & Demo Notice

All applicant names, corporate entities, email contacts, and budgets depicted in screenshots represent **simulated evaluation records** designed for testing and portfolio demonstration purposes.

---

## Security

Confidential API credentials, Google Service Account keys, OAuth tokens, and webhook secrets are kept securely in local environment variables and sanitized in the exported workflow template.

---

## Project Status

**Completed — Portfolio Project**

---

## Author

**Faiziya Khan**

AI Automation Engineer | n8n | AI Agents | RAG | Workflow Automation

📧 Email: khanfaiziya2@gmail.com
