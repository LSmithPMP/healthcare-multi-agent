# Security Policy

## Credentials & Secrets

All credentials in this workflow are managed through n8n's built-in credential store:

- **OpenAI API keys** — stored as named n8n credentials, never hardcoded
- **Google Sheets OAuth2** — stored as named n8n credentials
- **Gmail OAuth2** — stored as named n8n credentials, used only for sending prep instruction emails

No API keys, tokens, or secrets appear in workflow node logic or this repository.

## Data Handling

- Google Sheets datasets contain **synthetic/demo data only** — no real patient information
- No PII is stored, logged, or persisted beyond the active workflow execution
- Session memory (Window Buffer) is scoped to the active n8n session and not written to disk

## Responsible Use

This system was built for academic demonstration purposes. If adapted for real healthcare use:

- Ensure HIPAA compliance for any PHI handling
- Implement proper authentication before exposing the chat interface publicly
- Replace demo datasets with properly governed data sources
- Conduct a full security and compliance review

## Reporting a Vulnerability

1. **Do not** open a public GitHub issue
2. Email: lamontesmithpmp@gmail.com with subject line `[SECURITY] healthcare-multi-agent`
3. Expected response time: 72 hours
