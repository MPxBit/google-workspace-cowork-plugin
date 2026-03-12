---
description: "Run a Drive or Gmail operation across all accounts simultaneously. Usage: /gw:bulk-operation then describe what you want done"
---

# Bulk Operation Across All Accounts

The user wants to perform the same operation across all (or selected) Google account profiles simultaneously.

## Steps

1. Call `mcp__google-workspace__list_accounts` to see available profiles and their status
2. Ask the user which profiles to target (or confirm "all connected")
3. Ask what operation to perform if not clear from $ARGUMENTS
4. For bulk Drive listings, use `mcp__google-workspace__bulk_drive_list` with:
   - `profiles`: comma-separated profile names, or "all"
   - `query`: Drive search query (if applicable)
5. For bulk Gmail drafts, use `mcp__google-workspace__bulk_gmail_draft` with:
   - `profiles`: comma-separated profile names, or "all"
   - `to`, `subject`, `body`: email details
6. For other operations, call the appropriate MCP tool once per profile
7. Collect and display results from each account, clearly labeled by profile name
8. Report any failures separately
