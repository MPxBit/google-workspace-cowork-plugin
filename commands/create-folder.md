---
description: "Create a folder in Google Drive. Usage: /gw:create-folder <profile> <folder-name> [parent-folder-id]"
---

# Create Folder in Google Drive

Parse $ARGUMENTS for: profile name, folder name, and optional parent folder ID.

## Steps

1. Parse arguments: `<profile> <folder-name> [parent-folder-id]`
2. If parent folder ID is provided, create as subfolder:
   ```bash
   GOOGLE_WORKSPACE_CLI_CONFIG_DIR=~/.config/gws/profiles/<profile> \
     gws drive files create --json '{
       "name": "<folder-name>",
       "mimeType": "application/vnd.google-apps.folder",
       "parents": ["<parent-folder-id>"]
     }'
   ```
3. If no parent, create at root:
   ```bash
   GOOGLE_WORKSPACE_CLI_CONFIG_DIR=~/.config/gws/profiles/<profile> \
     gws drive files create --json '{
       "name": "<folder-name>",
       "mimeType": "application/vnd.google-apps.folder"
     }'
   ```
4. Report the created folder's ID and a link to it
