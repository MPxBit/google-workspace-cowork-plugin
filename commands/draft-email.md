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
3. Build the RFC 2822 message and base64url-encode it:
   ```bash
   RAW=$(printf 'To: <to>\r\nCc: <cc>\r\nBcc: <bcc>\r\nSubject: <subject>\r\nContent-Type: text/plain; charset="UTF-8"\r\n\r\n<body>' | base64 -w 0 | tr '+/' '-_' | tr -d '=')
   ```
4. Create the draft:
   ```bash
   GOOGLE_WORKSPACE_CLI_CONFIG_DIR=~/.config/gws/profiles/<profile> \
     gws gmail users drafts create \
       --params '{"userId": "me"}' \
       --json "{\"message\": {\"raw\": \"$RAW\"}}"
   ```
5. Report success with the draft ID
