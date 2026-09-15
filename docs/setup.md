# Setup Guide

This guide explains how to set up the AI Job Hunter Agent locally using n8n, Google services, Gemini, and Telegram.

## Prerequisites

Before setting up the workflow, you need:

- An n8n instance
- A Google account
- Google Drive
- Google Sheets
- A Telegram account
- A Google Gemini API key
- A resume in PDF format

## 1. Set Up n8n

Run your n8n instance and open the n8n editor.

Create a new workflow and import the workflow file available in:

`workflow/job-hunter-agent.json`

> Credentials are not included in this repository. You must configure your own credentials in n8n.

## 2. Google Drive Setup

Upload your resume PDF to Google Drive.

The workflow downloads the resume from Google Drive and extracts the text from the PDF.

Configure a Google Drive OAuth2 credential in n8n.

Recommended credential name:

`Google Drive account`

## 3. Google Sheets Setup

Create a Google Spreadsheet with two sheets.

### Filter Sheet

Use this sheet to define your job-search preferences.

Recommended columns:

| Keyword | Location | Experience Level | Mode | Job Type | Easy Apply |
|---|---|---|---|---|---|
| React JS | Gurugram | Entry level, Mid-Senior level | Remote, Hybrid | Full-time | TRUE |

### Result Sheet

Use this sheet to store jobs processed by the workflow.

Recommended columns:

| Title | Company | Location | link | description | score | Cover Letter |
|---|---|---|---|---|---|---|

Configure a Google Sheets OAuth2 credential in n8n.

Recommended credential name:

`Google Sheets account`

## 4. Google Gemini Setup

Create a Gemini API key through Google AI Studio.

Add the API key to an n8n Gemini credential.

Do not place the actual API key inside this repository.

The current workflow uses the Google Gemini Chat Model with Gemini 3.8 Flash.

## 5. Telegram Setup

Create a Telegram bot using BotFather.

1. Open Telegram.
2. Search for `@BotFather`.
3. Use `/newbot`.
4. Follow the instructions.
5. Copy the generated bot token.
6. Open your new bot and press Start.
7. Send a test message.

Create a Telegram credential in n8n using your bot token.

The Telegram Send Message node requires the correct Chat ID.

> Never publish your Telegram bot token in GitHub.

## 6. Configure the Workflow

The main workflow follows this sequence:

Schedule Trigger
→ Download file
→ Extract from File
→ Get row(s) in sheet
→ Create Search URL
→ Fetch Jobs from LinkedIn
→ Extract Jobs Links
→ Split Out
→ Loop Over Items
→ Wait
→ Fetch Job Page
→ Parse Job Attributes
→ Modify Job Attributes
→ AI Agent
→ Parse AI Output
→ Append or update row in sheet
→ Score Filter
→ Telegram

## 7. Test Individual Nodes

Before running the complete workflow, test the important nodes individually.

Recommended order:

1. Schedule Trigger
2. Download file
3. Extract from File
4. Get row(s) in sheet
5. Create Search URL
6. Fetch Jobs from LinkedIn
7. Extract Jobs Links
8. Split Out
9. Fetch Job Page
10. Parse Job Attributes
11. AI Agent
12. Parse AI Output
13. Google Sheets
14. Score Filter
15. Telegram

## 8. Run the Complete Workflow

Click **Execute Workflow** in n8n.

Verify that:

- Jobs are discovered.
- Job information is extracted.
- Gemini generates a matching score.
- Gemini generates a cover letter.
- Results are stored in Google Sheets.
- High-scoring jobs are sent to Telegram.

## Security

Never commit the following to GitHub:

- Gemini API keys
- Telegram bot tokens
- Google OAuth tokens
- Client secrets
- Passwords
- Resume files containing personal information
- Private Google Sheet data

The workflow JSON in this repository is intended to document the automation structure and configuration, not to provide working credentials.
