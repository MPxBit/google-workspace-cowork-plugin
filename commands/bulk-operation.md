---
description: "Run a Drive or Gmail operation across all accounts simultaneously. Usage: /gw:bulk-operation then describe what you want done"
---

# Bulk Operation Across All Accounts

The user wants to perform the same operation across all (or selected) Google account profiles simultaneously.

## Steps

1. List available profiles:
   ```bash
   ls ~/.config/gws/profiles/
   ```
2. Ask the user which profiles to target (or confirm "all")
3. Ask what operation to perform if not clear from $ARGUMENTS
4. For each target profile, run the operation in parallel using background processes:
   ```bash
   for profile in <profiles>; do
     GOOGLE_WORKSPACE_CLI_CONFIG_DIR=~/.config/gws/profiles/$profile \
       <gws command> &
   done
   wait
   ```
5. Collect and display results from each account, clearly labeled by profile name
6. Report any failures separately
