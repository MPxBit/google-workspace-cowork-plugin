---
description: List all configured Google account profiles
---

# List Google Account Profiles

List all configured profiles and verify each one's connectivity.

## Steps

1. List all profile directories:
   ```bash
   ls ~/.config/gws/profiles/
   ```
2. For each profile, fetch the authenticated email:
   ```bash
   GOOGLE_WORKSPACE_CLI_CONFIG_DIR=~/.config/gws/profiles/<profile> gws gmail users getProfile --params '{"userId": "me"}'
   ```
3. Display a summary table: profile name, email address, status (connected/disconnected)
