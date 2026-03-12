---
name: gws-multi-account
description: Manages multiple Google Workspace accounts using gws CLI with named profiles. Use when the user needs to operate on multiple Google accounts, switch between accounts, or run operations across accounts simultaneously.
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
└── client-acme/
    ├── client_secret.json
    └── credentials.json
```

## Running Commands Against a Specific Profile

Set `GOOGLE_WORKSPACE_CLI_CONFIG_DIR` to the profile directory before each `gws` command:

```bash
GOOGLE_WORKSPACE_CLI_CONFIG_DIR=~/.config/gws/profiles/work gws drive files list
```

Or use a credentials file directly:

```bash
GOOGLE_WORKSPACE_CLI_CREDENTIALS_FILE=~/.config/gws/profiles/work/credentials.json gws drive files list
```

## Running Commands Across Multiple Accounts Simultaneously

Use parallel subshells or sequential commands with different config dirs:

```bash
# Parallel execution across accounts
GOOGLE_WORKSPACE_CLI_CONFIG_DIR=~/.config/gws/profiles/work gws drive files list &
GOOGLE_WORKSPACE_CLI_CONFIG_DIR=~/.config/gws/profiles/personal gws drive files list &
wait
```

## Setting Up a New Profile

```bash
mkdir -p ~/.config/gws/profiles/<profile-name>
# Copy or create client_secret.json in the profile directory
cp /path/to/client_secret.json ~/.config/gws/profiles/<profile-name>/
# Login for that profile
GOOGLE_WORKSPACE_CLI_CONFIG_DIR=~/.config/gws/profiles/<profile-name> gws auth login
```

## Important Rules

- ALWAYS use the profile-specific config dir when running gws commands
- NEVER run gws without specifying a profile — it will use the default config
- When the user says "all accounts" or "every account", iterate over all directories in ~/.config/gws/profiles/
- Use parallel execution (background processes with `wait`) when operating on multiple accounts simultaneously
- List available profiles by listing directories: `ls ~/.config/gws/profiles/`
