# Workflow Documentation

## Overview

The AI Job Hunter Agent is an n8n automation that searches for relevant jobs, extracts job information, evaluates job compatibility using AI, generates cover letters, stores results, and sends notifications for high-scoring opportunities.

## Complete Workflow

```text
Schedule Trigger
        ↓
Download file
        ↓
Extract from File
        ↓
Get row(s) in sheet
        ↓
Create Search URL
        ↓
Fetch Jobs from LinkedIn
        ↓
Extract Jobs Links
        ↓
Split Out
        ↓
Loop Over Items
        ↓
Wait
        ↓
HTTP Request
        ↓
Parse Job Attributes
        ↓
Modify Job Attributes
        ↓
AI Agent
        ↓
Parse AI Output
        ↓
Append or update row in sheet
        ↓
Score Filter
       / \
      /   \
   True   False
    ↓       ↓
Telegram   Loop
    ↓
Loop Over Items
```

## 1. Schedule Trigger

The Schedule Trigger starts the automation automatically.

The workflow is configured to run once per day.

## 2. Download File

The Google Drive node downloads the resume PDF.

The resume is used as the candidate profile for AI matching.

## 3. Extract from File

The Extract from File node extracts text from the resume PDF.

The extracted text is later provided to the AI Agent.

## 4. Get Job Search Preferences

Google Sheets stores the search criteria.

The workflow reads values such as:

- Keyword
- Location
- Experience Level
- Mode
- Job Type
- Easy Apply

## 5. Create Search URL

A JavaScript Code node converts the search preferences into a LinkedIn job-search URL.

The node dynamically adds filters such as:

- Keywords
- Location
- Experience level
- Work mode
- Job type
- Easy Apply

### Work Mode Mapping

```text
On-Site → 1
Remote  → 2
Hybrid  → 3
```

## 6. Fetch Jobs from LinkedIn

An HTTP Request node sends a GET request to the generated search URL.

The response contains the job-search page HTML.

## 7. Extract Job Links

The HTML extraction node finds job links from the search results.

The extracted links are returned as an array.

## 8. Split Out

The Split Out node converts the array of job links into individual n8n items.

Each item represents one job.

## 9. Loop Over Items

The Loop Over Items node processes jobs individually.

The batch size is configured to process one job at a time.

## 10. Wait

A Wait node introduces a delay between requests.

This reduces the frequency of requests while processing multiple jobs.

## 11. Fetch Job Page

An HTTP Request node opens each individual job page.

The workflow retrieves the job-page HTML.

## 12. Parse Job Attributes

The HTML extraction step extracts structured information such as:

- Job title
- Company
- Location
- Description
- Job ID

## 13. Modify Job Attributes

The extracted information is cleaned and formatted.

The node also creates the direct LinkedIn job URL.

Example:

```text
https://www.linkedin.com/jobs/view/JOB_ID
```

## 14. AI Agent

The AI Agent compares the job description with the extracted resume.

It generates:

- Job matching score
- Personalized cover letter

The current implementation uses Google Gemini as the language model.

## 15. Parse AI Output

The AI output is cleaned and converted into usable JSON fields.

Expected fields include:

```json
{
  "score": 80,
  "coverLetter": "..."
}
```

## 16. Store Results

The Google Sheets node stores the processed job.

The result includes:

- Title
- Company
- Location
- Link
- Description
- Score
- Cover Letter

The job link is used to prevent duplicate entries.

## 17. Score Filter

The Score Filter determines whether a job should generate a Telegram notification.

Current threshold:

```text
Score >= 50
```

Jobs meeting the threshold continue to Telegram.

Jobs below the threshold continue to the next job.

## 18. Telegram Notification

High-scoring jobs are sent to Telegram.

The notification includes information such as:

```text
Title
Company
Location
Job Score
Apply Link
```

The complete process then loops back to process the next job.

## Result

The final automation provides an end-to-end job discovery pipeline:

```text
Search
  ↓
Extract
  ↓
Analyze
  ↓
Score
  ↓
Generate Cover Letter
  ↓
Store
  ↓
Notify
```
