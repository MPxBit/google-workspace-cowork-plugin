---
description: "Create a draft email. Usage: /gw:draft-email <profile> then follow prompts for to/subject/body"
---

# Create Draft Email

The user wants to create a draft email on the account profile specified in $ARGUMENTS.

## Steps

1. Parse $ARGUMENTS for the profile name
2. Ask the user for (if not already provided):
   - **To**: recipient email address(es)
   - **Cc**: (optional)
   - **Bcc**: (optional)
   - **Subject**: email subject line
   - **Body**: email body text
3. Call the `mcp__google-workspace__gmail_create_draft` tool with:
   - `profile`: the profile name
   - `to`: recipient email(s)
   - `subject`: email subject
   - `body`: email body text
   - `cc`: CC recipients (if provided)
   - `bcc`: BCC recipients (if provided)
4. Report success with the draft ID
