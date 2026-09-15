# Architecture

## System Architecture

The AI Job Hunter Agent connects multiple services through n8n.

```text
                    ┌───────────────────┐
                    │   Schedule Trigger│
                    └─────────┬─────────┘
                              ↓
                    ┌───────────────────┐
                    │   Google Drive    │
                    │     Resume PDF    │
                    └─────────┬─────────┘
                              ↓
                    ┌───────────────────┐
                    │ Extract Resume    │
                    │      Text         │
                    └─────────┬─────────┘
                              ↓
                    ┌───────────────────┐
                    │   Google Sheets   │
                    │ Search Preferences│
                    └─────────┬─────────┘
                              ↓
                    ┌───────────────────┐
                    │ JavaScript        │
                    │ Search URL Builder│
                    └─────────┬─────────┘
                              ↓
                    ┌───────────────────┐
                    │ LinkedIn Job      │
                    │ Search / HTTP     │
                    └─────────┬─────────┘
                              ↓
                    ┌───────────────────┐
                    │ Job Link & HTML   │
                    │ Extraction        │
                    └─────────┬─────────┘
                              ↓
                    ┌───────────────────┐
                    │ Job Processing    │
                    │ & Parsing         │
                    └─────────┬─────────┘
                              ↓
                    ┌───────────────────┐
                    │ Google Gemini     │
                    │ AI Agent          │
                    └─────────┬─────────┘
                              ↓
                    ┌───────────────────┐
                    │ Match Score +     │
                    │ Cover Letter      │
                    └─────────┬─────────┘
                              ↓
                    ┌───────────────────┐
                    │   Google Sheets   │
                    │     Results       │
                    └─────────┬─────────┘
                              ↓
                    ┌───────────────────┐
                    │   Score Filter    │
                    └─────────┬─────────┘
                              ↓
                    ┌───────────────────┐
                    │ Telegram          │
                    │ Notification      │
                    └───────────────────┘
```

## Components

### n8n

Acts as the central workflow automation platform.

It coordinates the complete process and connects the external services.

### Google Drive

Stores the resume PDF used by the AI matching system.

### Google Sheets

Provides two major functions:

1. Stores job-search preferences.
2. Stores processed job results.

### JavaScript

JavaScript is used inside n8n to dynamically construct the job-search URL and transform search parameters into the required filter values.

### LinkedIn

The workflow retrieves job-search results and individual job-page information through HTTP requests and HTML extraction.

### Google Gemini

Gemini analyzes the candidate resume against each job description.

It produces:

- Matching score
- Personalized cover letter

### Telegram

Telegram provides notifications when a job reaches the configured score threshold.

## Data Flow

```text
Resume
   +
Search Preferences
   ↓
Job Search
   ↓
Job Description
   ↓
AI Analysis
   ↓
Score + Cover Letter
   ↓
Google Sheets
   ↓
Score Filter
   ↓
Telegram Notification
```

## Main Design Goals

- Automate repetitive job searching
- Reduce manual job screening
- Use AI for resume-to-job matching
- Generate personalized cover letters
- Keep job information organized
- Notify the user about higher-scoring opportunities
