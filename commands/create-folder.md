---
description: "Create a folder in Google Drive. Usage: /gw:create-folder <profile> <folder-name> [parent-folder-id]"
---

# Create Folder in Google Drive

Parse $ARGUMENTS for: profile name, folder name, and optional parent folder ID.

## Steps

1. Parse arguments: `<profile> <folder-name> [parent-folder-id]`
2. Call the `mcp__google-workspace__drive_create_folder` tool with:
   - `profile`: the profile name
   - `name`: the folder name
   - `parent_id`: the parent folder ID (if provided)
3. Report the created folder's ID and a link to it
