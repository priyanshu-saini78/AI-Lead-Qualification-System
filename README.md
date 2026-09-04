# AI Lead Qualification & Processing System

An AI-powered lead processing and qualification workflow built with n8n, Google Gemini, Google Sheets, and Gmail.

The system receives lead information through a webhook, validates the required fields, uses an AI model to analyze valid leads, stores the qualification results in Google Sheets, and automatically sends qualified leads an email with the result and next steps.

Invalid leads are routed separately and recorded in Google Sheets with the reason for rejection.

---

## Project Overview

This project demonstrates how AI and workflow automation can be used to automate a real-world business lead qualification process.

Instead of manually reviewing every incoming lead, the workflow automatically:

1. Receives lead information through a webhook.
2. Validates the incoming lead data.
3. Separates valid and invalid leads.
4. Sends valid leads to Google Gemini.
5. Generates an AI-based lead score and priority.
6. Processes the AI response using JavaScript.
7. Stores the qualification result in Google Sheets.
8. Sends a personalized email through Gmail.
9. Records rejected leads with the reason for rejection.

The project demonstrates practical experience with AI automation, APIs, webhooks, conditional logic, JavaScript, data transformation, Google Sheets, and Gmail automation.

---

## Business Problem

Businesses receive leads from websites, contact forms, advertising campaigns, landing pages, and other sources.

Manually reviewing every lead can be slow and inconsistent.

A typical manual process requires a person to:

1. Check whether the lead information is complete.
2. Read and understand the customer's requirement.
3. Estimate the quality of the lead.
4. Determine the lead priority.
5. Record the lead information.
6. Decide what action should be taken.
7. Contact the qualified lead.

This creates delays and increases the possibility of inconsistent lead handling.

The goal of this project is to automate this process using an AI-powered workflow.

---

## Solution

I built an automated lead qualification workflow using n8n.

The workflow receives lead data through a webhook and first validates the required information.

If the lead contains all required fields:

- The lead is sent to Google Gemini for AI-based qualification.
- Gemini analyzes the lead requirement.
- The AI generates a lead score.
- The AI determines the lead priority.
- The AI creates a requirement summary.
- The AI recommends the next action.
- JavaScript transforms the AI response into structured fields.
- The result is stored in Google Sheets.
- A personalized qualification email is sent through Gmail.

If required information is missing:

- The lead is routed to the rejection branch.
- The workflow identifies the missing information.
- The rejected lead is stored in Google Sheets.
- The lead is not sent to Gemini.
- No qualification email is triggered.

This creates an automated pipeline from lead intake to qualification and follow-up.

---

## Tech Stack

- **n8n** — Workflow automation and orchestration
- **Google Gemini API** — AI-powered lead qualification
- **JavaScript** — Data transformation and AI response processing
- **Webhooks** — Receive lead data through HTTP POST requests
- **Google Sheets** — Store lead and qualification results
- **Gmail** — Send automated emails
- **PowerShell** — Test webhook requests
- **JSON** — Data format used between workflow components
- **VS Code** — Development and documentation
- **Git/GitHub** — Version control and project portfolio

---

## Workflow Architecture

The workflow is divided into two paths:

- Valid Lead Path
- Invalid Lead Path

### Valid Lead Flow

```text
Webhook
   ↓
IF — Validate Required Fields
   ↓ TRUE
Message a Model — Google Gemini
   ↓
Code in JavaScript
   ↓
Edit Fields
   ↓
Google Sheets
   ↓
Gmail```

---

## Invalid Lead Flow

```text
Webhook
   ↓
IF — Validate Required Fields
   ↓ FALSE
Edit Fields
   ↓
Google Sheets

s