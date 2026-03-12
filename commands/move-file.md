---
description: "Move a file or folder in Google Drive. Usage: /gw:move-file <profile> <file-id> <new-parent-id>"
---

# Move File or Folder in Google Drive

Parse $ARGUMENTS for: profile name, file/folder ID, and destination parent folder ID.

## Steps

1. Parse arguments: `<profile> <file-id> <new-parent-id>`
2. Get the file's current parent:
   ```bash
   GOOGLE_WORKSPACE_CLI_CONFIG_DIR=~/.config/gws/profiles/<profile> \
     gws drive files get --params '{
       "fileId": "<file-id>",
       "fields": "id,name,parents"
     }'
   ```
3. Move the file using the current parent as `removeParents`:
   ```bash
   GOOGLE_WORKSPACE_CLI_CONFIG_DIR=~/.config/gws/profiles/<profile> \
     gws drive files update \
       --params '{"fileId": "<file-id>", "addParents": "<new-parent-id>", "removeParents": "<old-parent-id>"}' \
       --json '{}'
   ```
4. Confirm the move with the file name and new location
