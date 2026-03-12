---
description: Set up a new Google account profile for multi-account operations
---

# Setup Google Account Profile

The user wants to add a new Google account profile named "$ARGUMENTS".

## Steps

1. Ask for the profile name if not provided in $ARGUMENTS
2. Create the profile directory:
   ```bash
   mkdir -p ~/.config/gws/profiles/<profile-name>
   ```
3. Check if `gws` is installed: `which gws`
   - If not installed, run: `npm install -g @googleworkspace/cli`
4. Ask the user if they have a `client_secret.json` for this account:
   - If yes: ask for the path and copy it to the profile directory
   - If no: run `GOOGLE_WORKSPACE_CLI_CONFIG_DIR=~/.config/gws/profiles/<profile-name> gws auth setup`
5. Run login for the profile:
   ```bash
   GOOGLE_WORKSPACE_CLI_CONFIG_DIR=~/.config/gws/profiles/<profile-name> gws auth login
   ```
6. Verify the profile works:
   ```bash
   GOOGLE_WORKSPACE_CLI_CONFIG_DIR=~/.config/gws/profiles/<profile-name> gws gmail users getProfile --params '{"userId": "me"}'
   ```
7. Report success with the email address connected
