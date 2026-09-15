# Troubleshooting

## No Jobs Found

### Possible Cause

The generated job-search URL may not return any results.

### Solution

Check the output of the `Create Search URL` node.

Open the generated URL in a browser and verify that jobs are available.

Also check:

- Keyword
- Location
- Experience Level
- Mode
- Job Type
- Easy Apply

---

## LinkedIn HTML Extraction Fails

### Possible Cause

The HTML structure of the page may have changed.

### Solution

Check the CSS selectors used by:

- Extract Jobs Links
- Parse Job Attributes

If LinkedIn changes its page structure, these selectors may need to be updated.

---

## AI Agent Fails

### Possible Causes

- Gemini API quota exhausted
- API key configuration issue
- Temporary Gemini service availability issue
- Invalid model configuration

### Solution

Check the Google Gemini credential in n8n.

Verify that the selected Gemini model is available for the configured API project.

Also check the AI Agent execution output for the exact error.

---

## Gemini Rate Limit / Temporary Errors

### Solution

The AI Agent can retry failed requests.

Recommended configuration:

```text
Retry On Fail: ON
Max Tries: 3
Wait Between Tries: 5000 ms
```

If Gemini is temporarily unavailable, retrying after a short delay may allow the request to succeed.

---

## Malformed JSON from AI

### Possible Cause

The AI model may return JSON wrapped in Markdown code blocks.

### Solution

The `Parse AI Output` node removes Markdown code-block markers before parsing the response.

Expected structure:

```json
{
  "score": 80,
  "coverLetter": "Generated cover letter..."
}
```

---

## Google Sheets Not Updating

### Possible Causes

- Incorrect spreadsheet selected
- Incorrect sheet selected
- Column names do not match
- Google Sheets credential is not authorized

### Solution

Check the `Append or update row in sheet` node.

Verify the column names and mappings.

The matching column should use the job link to prevent duplicate entries.

---

## Telegram Says "Chat Not Found"

### Possible Cause

The Telegram Chat ID is incorrect, or the bot has not received a message from the user.

### Solution

1. Open the Telegram bot.
2. Press Start.
3. Send `/start`.
4. Send a test message.
5. Use a Telegram Trigger in n8n to retrieve the correct `message.chat.id`.
6. Put that Chat ID into the Telegram Send Message node.

Make sure the Telegram Trigger and Send Message node use the same Telegram credential.

---

## Telegram Bot Does Not Send Messages

Check:

- Bot token
- Telegram credential
- Chat ID
- Score Filter output
- Telegram node configuration

The `Score Filter` must send the job through its True branch before Telegram is executed.

---

## Google Drive Resume Cannot Be Downloaded

Check:

- Google Drive credential
- File ID
- Resume location
- Google Drive permissions

The Download File node must be able to access the resume.

---

## Duplicate Jobs

The Google Sheets node uses the job link as the matching value.

This allows existing jobs to be updated instead of creating unnecessary duplicate entries.

---

## Workflow Is Too Slow

The workflow intentionally processes jobs individually and uses a delay between requests.

This helps control request frequency.

If necessary, review:

- Number of jobs returned
- Wait duration
- AI processing time
- HTTP retry settings
