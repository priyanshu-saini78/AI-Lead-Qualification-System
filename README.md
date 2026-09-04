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
Gmail
```

### Invalid Lead Flow

```text
Webhook
   ↓
IF — Validate Required Fields
   ↓ FALSE
Edit Fields
   ↓
Google Sheets
```
### Complete Workflow

```text
                         ┌──────────────────────┐
                         │       Webhook        │
                         │    Receive Lead      │
                         └──────────┬───────────┘
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │         IF           │
                         │ Validate Lead Data   │
                         └───────┬───────┬──────┘
                                 │       │
                               TRUE    FALSE
                                 │       │
                                 ▼       ▼
                      ┌────────────────┐  ┌────────────────┐
                      │ Google Gemini  │  │  Edit Fields   │
                      │ AI Qualification│  │ Rejection Data │
                      └───────┬────────┘  └───────┬────────┘
                              │                   │
                              ▼                   ▼
                      ┌────────────────┐  ┌────────────────┐
                      │ JavaScript     │  │ Google Sheets  │
                      │ Data Processing│  │ Store Rejected │
                      └───────┬────────┘  └────────────────┘
                              │
                              ▼
                      ┌────────────────┐
                      │  Edit Fields   │
                      │  Format Output │
                      └───────┬────────┘
                              │
                              ▼
                      ┌────────────────┐
                      │ Google Sheets  │
                      │ Store Qualified│
                      └───────┬────────┘
                              │
                              ▼
                      ┌────────────────┐
                      │     Gmail      │
                      │   Send Email   │
                      └────────────────┘
```

---

## Validation Rules

The IF node checks the following required fields:

| Field | Validation |
|---|---|
| Email | Must not be empty |
| Name | Must not be empty |
| Company | Must not be empty |
| Requirement | Must not be empty |

All four conditions must be satisfied for the lead to follow the **TRUE** branch.

The **Budget** field is not used as a required validation condition. A lead can still be processed if the budget is missing.

---

## Workflow Nodes

### 1. Webhook

The Webhook node is the entry point of the workflow.

It receives lead information through an HTTP POST request.

Example input:

```json
{
  "name": "Rahul Sharma",
  "email": "rahul@example.com",
  "company": "ABC Technologies",
  "requirement": "We need an AI chatbot for customer support.",
  "budget": 200000
}
```

The webhook passes the incoming data to the IF node.

---

### 2. IF — Validate Required Fields

The IF node validates whether the required lead information is present.

The workflow checks:

```text
Email       → Must not be empty
Name        → Must not be empty
Company     → Must not be empty
Requirement → Must not be empty
```

The conditions use **AND** logic.

This means all required fields must be available for the lead to enter the TRUE branch.

The Budget field is not required for validation.

#### TRUE Branch

```text
Google Gemini
      ↓
JavaScript
      ↓
Edit Fields
      ↓
Google Sheets
      ↓
Gmail
```

#### FALSE Branch

```text
Edit Fields
      ↓
Google Sheets
```

Gemini is not called for invalid leads.

---

### 3. Google Gemini

Valid leads are sent to Google Gemini for AI-based qualification.

The model analyzes information such as:

- Lead requirement
- Company
- Budget
- Business need

The AI generates:

- Lead Score
- Priority
- Requirement Summary
- Recommended Action

Example AI result:

```json
{
  "lead_score": 90,
  "priority": "High",
  "requirement_summary": "AI chatbot for customer support with a significant budget of 200,000.",
  "recommended_action": "Schedule a discovery call immediately to discuss technical requirements and scope."
}
```

---

### 4. Code in JavaScript

The JavaScript Code node processes the AI response.

Its purpose is to convert the AI output into structured fields that can be mapped into Google Sheets.

Example structured result:

```json
{
  "lead_score": 90,
  "priority": "High",
  "requirement_summary": "AI chatbot for customer support with a significant budget of 200,000.",
  "recommended_action": "Schedule a discovery call immediately to discuss technical requirements and scope."
}
```

---

### 5. Edit Fields — Qualified Lead

The Edit Fields node combines the original lead information with the AI qualification result.

The final structure contains:

```text
Name
Email
Company
Requirement
Budget
Lead Score
Priority
Requirement Summary
Recommended Action
Status
```

For qualified leads:

```text
Status = Qualified
```

---

### 6. Google Sheets — Qualified Leads

Qualified leads are stored in Google Sheets.

The spreadsheet contains the following columns:

| Column |
|---|
| Name |
| Email |
| Company |
| Requirement |
| Budget |
| Lead Score |
| Priority |
| Requirement Summary |
| Recommended Action |
| Status |
| Reason |

The TRUE branch uses **Append or Update Row** with **Email** as the matching field.

This helps prevent duplicate qualified records when the same email is processed again.

---

### 7. Gmail — Qualified Lead Email

After the qualified lead is stored in Google Sheets, the Gmail node sends an automated email.

#### Recipient

```text
{{ $json.Email }}
```

#### Subject

```text
AI Lead Qualified - {{ $json.Name }} - {{ $json.Priority }} Priority
```

#### HTML Email Body

```html
<p>Hello {{ $json.Name }},</p>

<p>Thank you for contacting us regarding your AI solution requirement.</p>

<p>We have reviewed your requirement and your lead has been qualified.</p>

<p><strong>Company:</strong> {{ $json.Company }}</p>

<p><strong>Requirement:</strong> {{ $json.Requirement }}</p>

<p><strong>Lead Score:</strong> {{ $json["Lead Score"] }}/100</p>

<p><strong>Priority:</strong> {{ $json.Priority }}</p>

<p><strong>Recommended Action:</strong> {{ $json["Recommended Action"] }}</p>

<p>Our team will contact you shortly to discuss the technical requirements and next steps.</p>

<p>Regards,<br>
AI Solutions Team</p>
```

---

## Invalid Lead Handling

Invalid leads are handled through the FALSE branch of the IF node.

The workflow does not send invalid leads to Gemini.

Instead, the Edit Fields node creates rejection information.

The rejected lead is then stored in Google Sheets.

### Rejection Status

```text
Status = Rejected
```

### Rejection Reason

The workflow dynamically identifies the missing field.

Expression:

```javascript
{{ !$json.body.email ? "Email address is missing" : !$json.body.name ? "Name is missing" : !$json.body.company ? "Company is missing" : !$json.body.requirement ? "Requirement is missing" : "Invalid lead data" }}
```

The result is stored in the **Reason** column.

---

## Rejection Logic

The rejection logic checks the fields in this order:

1. Email
2. Name
3. Company
4. Requirement

Example:

If email is missing:

```text
Status → Rejected
Reason → Email address is missing
```

If name is missing:

```text
Status → Rejected
Reason → Name is missing
```

If company is missing:

```text
Status → Rejected
Reason → Company is missing
```

If requirement is missing:

```text
Status → Rejected
Reason → Requirement is missing
```

---

## Duplicate Lead Handling

Qualified leads use Google Sheets **Append or Update Row**.

The Email field is used as the matching field.

If the same email is processed again, the workflow can update the existing qualified record instead of creating another one.

Invalid leads use **Append Row** because an invalid lead may not contain an email address and therefore may not have a reliable unique matching field.

---

## Testing

The workflow was tested using PowerShell HTTP POST requests.

Example PowerShell test:

```powershell
$body = @{
    name = "Rahul Sharma"
    email = "rahul@example.com"
    company = "ABC Technologies"
    requirement = "We need an AI chatbot for customer support."
    budget = 200000
} | ConvertTo-Json

Invoke-RestMethod `
    -Uri "http://localhost:5678/webhook/ai-lead" `
    -Method POST `
    -ContentType "application/json" `
    -Body $body
```

### Test Scenarios

| Test Case | Expected Result |
|---|---|
| All required fields provided | Qualified lead |
| Email missing | Rejected |
| Name missing | Rejected |
| Company missing | Rejected |
| Requirement missing | Rejected |
| Same valid email submitted again | Existing qualified record updated |
| Invalid lead | Gemini not called |

---

## Example Test Cases

### Test 1 — Qualified Lead

Input:

```json
{
  "name": "Rahul Sharma",
  "email": "rahul@example.com",
  "company": "ABC Technologies",
  "requirement": "We need an AI chatbot for customer support.",
  "budget": 200000
}
```

Expected result:

```text
Name: Rahul Sharma
Email: rahul@example.com
Company: ABC Technologies
Lead Score: 90
Priority: High
Status: Qualified
```

The lead is stored in Google Sheets and a qualification email is sent through Gmail.

---

### Test 2 — Missing Requirement

Input:

```json
{
  "name": "Raj Verma",
  "email": "raj.verma@testcompany.com",
  "company": "TestCompany",
  "requirement": "",
  "budget": 250000
}
```

Expected result:

```text
Name: Raj Verma
Email: raj.verma@testcompany.com
Company: TestCompany
Status: Rejected
Reason: Requirement is missing
```

Gemini is not called.

---

### Test 3 — Missing Email

Input:

```json
{
  "name": "Amit Kumar",
  "email": "",
  "company": "TestCompany",
  "requirement": "We need an AI chatbot for customer support.",
  "budget": 300000
}
```

Expected result:

```text
Name: Amit Kumar
Email: 
Company: TestCompany
Status: Rejected
Reason: Email address is missing
```

Gemini is not called.

---

### Test 4 — Missing Name

Input:

```json
{
  "name": "",
  "email": "test@company.com",
  "company": "TestCompany",
  "requirement": "We need an AI chatbot.",
  "budget": 300000
}
```

Expected result:

```text
Name:
Email: test@company.com
Company: TestCompany
Status: Rejected
Reason: Name is missing
```

Gemini is not called.

---

### Test 5 — Missing Company

Input:

```json
{
  "name": "Sahil Verma",
  "email": "sahil@testcompany.com",
  "company": "",
  "requirement": "We need an AI chatbot for customer support.",
  "budget": 250000
}
```

Expected result:

```text
Name: Sahil Verma
Email: sahil@testcompany.com
Company:
Status: Rejected
Reason: Company is missing
```

Gemini is not called.

---

## Data Flow

The overall data flow can be summarized as:

```text
Lead Data
   ↓
Webhook
   ↓
Validation
   ↓
┌─────────────────────┐
│                     │
│   Valid Lead        │   Invalid Lead
│                     │
▼                     ▼
Gemini AI             Rejection Logic
│                     │
▼                     ▼
JavaScript            Google Sheets
│
▼
Edit Fields
│
▼
Google Sheets
│
▼
Gmail
```

---

## Setup

### Prerequisites

To reproduce this project, the following tools and accounts are required:

- n8n
- Google Gemini API access
- Google account
- Google Sheets
- Gmail
- PowerShell
- VS Code
- Git
- GitHub account

---

## n8n Workflow Setup

Create a new n8n workflow and add the following nodes:

```text
Webhook
   ↓
IF
   ↓ TRUE
Message a Model
   ↓
Code in JavaScript
   ↓
Edit Fields
   ↓
Google Sheets
   ↓
Gmail
```

For the FALSE branch:

```text
IF
   ↓ FALSE
Edit Fields
   ↓
Google Sheets
```

---

## Webhook Configuration

The webhook is configured to receive:

```text
Method: POST
Path: ai-lead
```

### Local Test URL

```text
http://localhost:5678/webhook-test/ai-lead
```

### Local Production URL

```text
http://localhost:5678/webhook/ai-lead
```

The production webhook can be used when the workflow is active.

---

## Input Format

The webhook expects JSON data.

### Required Fields

| Field | Type | Required |
|---|---|---|
| name | String | Yes |
| email | String | Yes |
| company | String | Yes |
| requirement | String | Yes |
| budget | Number | No |

Example:

```json
{
  "name": "Rahul Sharma",
  "email": "rahul@example.com",
  "company": "ABC Technologies",
  "requirement": "We need an AI chatbot for customer support.",
  "budget": 200000
}
```

---

## Output

### Qualified Lead

A qualified lead produces structured information such as:

```json
{
  "Name": "Rahul Sharma",
  "Email": "rahul@example.com",
  "Company": "ABC Technologies",
  "Requirement": "We need an AI chatbot for customer support.",
  "Budget": 200000,
  "Lead Score": 90,
  "Priority": "High",
  "Requirement Summary": "AI chatbot for customer support with a significant budget of 200,000.",
  "Recommended Action": "Schedule a discovery call immediately to discuss technical requirements and scope.",
  "Status": "Qualified"
}
```

### Rejected Lead

An invalid lead produces information such as:

```json
{
  "Name": "Raj Verma",
  "Email": "raj.verma@testcompany.com",
  "Company": "TestCompany",
  "Requirement": "",
  "Budget": 250000,
  "Status": "Rejected",
  "Reason": "Requirement is missing"
}
```

---

## Problems & Solutions

### 1. Google Sheets Authentication

During development, Google Sheets authentication produced an insufficient authentication scope error.

The issue was resolved by reconnecting/configuring the Google credentials with the required permissions.

---

### 2. Gemini API Rate Limit

During testing, the Gemini API returned a rate-limit/quota error.

This demonstrated an important real-world issue when integrating external AI APIs.

The workflow design also separates validation from AI processing, which prevents invalid leads from unnecessarily reaching the AI model.

---

### 3. Invalid Leads Reaching Gemini

Initially, invalid lead data could reach the AI processing branch because the validation conditions were incomplete.

The IF node was updated to validate:

- Email
- Name
- Company
- Requirement

All conditions use AND logic.

This ensures that incomplete leads are routed to the FALSE branch.

---

### 4. Incorrect Rejection Reason

During testing, the rejection reason was initially mapped incorrectly.

The Edit Fields configuration was corrected so that:

```text
Status → Rejected
Reason → Dynamic missing-field message
```

This produced clearer rejection records in Google Sheets.

---

### 5. Duplicate Qualified Leads

Repeated valid leads could create duplicate records.

The qualified branch was configured to use:

**Append or Update Row**

with **Email** as the matching field.

This allows an existing qualified lead to be updated instead of creating another record for the same email.

---

## Business Value

This automation can help businesses reduce manual lead-processing work.

Potential benefits include:

- Faster lead qualification
- Consistent lead evaluation
- Automated data storage
- Automated customer communication
- Reduced manual data entry
- Better lead prioritization
- Separation of valid and invalid leads
- Reduced unnecessary AI API usage
- Improved follow-up speed

---

## Real-World Use Cases

The same architecture can be adapted for:

- Sales lead qualification
- Website contact forms
- Marketing campaign leads
- SaaS demo requests
- AI consulting inquiries
- B2B service requests
- Customer support requests
- Recruitment lead processing
- Real-estate inquiries
- Business inquiry forms

---

## Future Improvements

Possible future improvements include:

### 1. Lead ID

Add a unique Lead ID to every incoming request.

This would provide a stronger identifier than email for tracking leads.

### 2. Lead Source

Add a field such as:

```text
Website
LinkedIn
Google Ads
Referral
Email
```

This would allow lead-source analysis.

### 3. CRM Integration

Connect the workflow to a CRM such as HubSpot or Salesforce.

### 4. Slack Notifications

Send an internal notification when a high-priority lead is detected.

### 5. Automatic Follow-Up

Create automated follow-up sequences for qualified leads.

### 6. Better AI Qualification

Improve the AI prompt using a defined scoring framework with factors such as:

- Budget
- Business need
- Urgency
- Company information
- Project complexity

### 7. Database Storage

Store lead records in a database such as PostgreSQL instead of relying only on Google Sheets.

### 8. Monitoring and Error Handling

Add dedicated error handling, logging, retry logic, and monitoring for production environments.

---

## Skills Demonstrated

This project demonstrates practical experience with:

### Automation

- n8n workflow design
- Conditional branching
- Data transformation
- Workflow orchestration

### AI

- Google Gemini API
- LLM integration
- Prompt-based lead analysis
- AI-generated structured output

### APIs

- HTTP POST requests
- Webhooks
- JSON
- API authentication
- External API integration

### Programming

- JavaScript
- Expressions
- JSON data processing
- PowerShell testing

### Business Integrations

- Google Sheets
- Gmail
- Google Gemini

### Testing & Debugging

- Webhook testing
- Multiple test scenarios
- Authentication troubleshooting
- API quota troubleshooting
- Conditional workflow debugging
- Duplicate data handling

---

## Project Structure

Current project documentation:

```text
AI-Lead-Qualification-System/
└── README.md
```

A recommended future project structure is:

```text
AI-Lead-Qualification-System/
│
├── README.md
├── workflow/
│   └── ai-lead-qualification-workflow.json
├── screenshots/
│   ├── workflow.png
│   ├── google-sheets.png
│   └── gmail-output.png
└── tests/
    └── test-leads.json
```

The additional workflow, screenshot, and test files can be added as the project portfolio is expanded.

---

## Interview Explanation

### 30-Second Explanation

> I built an AI-powered lead qualification system using n8n, Google Gemini, Google Sheets, and Gmail. The workflow receives lead data through a webhook and validates the required fields using conditional logic. Valid leads are sent to Gemini, which generates a lead score, priority, requirement summary, and recommended action. I then use JavaScript to structure the AI response and store the result in Google Sheets. Qualified leads also receive an automated email through Gmail. Invalid leads are routed separately, recorded with the rejection reason, and are not sent to the AI model.

---

## Interview Topics Demonstrated

This project can be used to demonstrate knowledge of:

- What is n8n?
- What is workflow automation?
- What is a webhook?
- What is an API?
- How does HTTP POST work?
- How is JSON used in automation?
- How do conditional nodes work?
- How do you integrate an LLM into an automation workflow?
- How do you process AI output?
- Why use JavaScript in n8n?
- How do you prevent invalid data from reaching an AI model?
- How do you handle duplicate records?
- How do you authenticate Google services?
- How do you test webhooks?
- How do you handle API rate limits?
- How can this workflow be improved for production?

---

## Key Learning

Through this project, I learned how individual automation components work together as a complete business workflow.

The project connected:

```text
Webhook
   ↓
Validation
   ↓
AI Processing
   ↓
JavaScript
   ↓
Data Transformation
   ↓
Google Sheets
   ↓
Gmail
```

This helped demonstrate the difference between learning individual tools and building an end-to-end automation system.

---

## Project Status

**Status: Completed — Portfolio Project**

The workflow currently supports:

- Webhook-based lead intake
- Required-field validation
- Valid and invalid lead routing
- Google Gemini AI qualification
- AI-generated lead scoring
- AI-generated priority
- AI-generated requirement summary
- AI-generated recommended action
- JavaScript data processing
- Google Sheets storage
- Duplicate qualified-lead handling
- Rejected-lead tracking
- Automated Gmail communication
- PowerShell webhook testing

---

## Conclusion

The AI Lead Qualification & Processing System demonstrates how AI, APIs, workflow automation, conditional logic, programming, and business integrations can be combined to solve a practical business problem.

The project goes beyond a simple AI API call by implementing:

- Input validation
- Conditional routing
- AI processing
- Structured data transformation
- Database-like record management using Google Sheets
- Automated communication
- Error and quota awareness
- Duplicate handling
- Testing with multiple scenarios

This project forms part of my practical preparation for entry-level AI Automation Engineer and AI Automation Specialist roles.

---

## Author

**Priyanshu Saini**

Aspiring AI Automation Engineer / AI Automation Specialist

Skills demonstrated in this project:

**n8n · AI Automation · Google Gemini · JavaScript · Webhooks · APIs · JSON · Google Sheets · Gmail · PowerShell**

---