---
name: gws-multi-account
description: Manages multiple Google Workspace accounts using MCP tools with named profiles. Use when the user needs to operate on multiple Google accounts, switch between accounts, or run operations across accounts simultaneously.
---

# Multi-Account Google Workspace Management

## Architecture

Each Google account is a **named profile** stored in its own config directory:

```
~/.config/gws/profiles/
├── work/
│   ├── client_secret.json
│   └── credentials.json
├── personal/
│   ├── client_secret.json
│   └── credentials.json
└── CRM1/
    ├── client_secret.json
    └── credentials.json
```

## Listing All Accounts

Use `mcp__google-workspace__list_accounts` to see all profiles and their connection status.

## Running Operations Against a Specific Profile

All MCP tools accept a `profile` parameter. Use the profile name (e.g., "work", "CRM1") when calling any tool.

## Running Operations Across Multiple Accounts

For Drive listings across accounts, use `mcp__google-workspace__bulk_drive_list` with `profiles` set to comma-separated names or "all".

For Gmail drafts across accounts, use `mcp__google-workspace__bulk_gmail_draft`.

For other operations, call the appropriate MCP tool once per target profile.

## Setting Up a New Profile

Profile setup requires direct bash access for authentication (OAuth flow):

```bash
mkdir -p ~/.config/gws/profiles/<profile-name>
cp /path/to/client_secret.json ~/.config/gws/profiles/<profile-name>/
GOOGLE_WORKSPACE_CLI_CONFIG_DIR=~/.config/gws/profiles/<profile-name> gws auth login -s drive,gmail
```

## Important Rules

- ALWAYS use the `profile` parameter when calling MCP tools
- When the user says "all accounts" or "every account", use the bulk tools or iterate over profiles from `list_accounts`
- Only use bash for initial profile setup/authentication — all other operations go through MCP tools
