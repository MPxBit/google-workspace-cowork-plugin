---
description: "Move a file or folder in Google Drive. Usage: /gw:move-file <profile> <file-id> <new-parent-id>"
---

# Move File or Folder in Google Drive

Parse $ARGUMENTS for: profile name, file/folder ID, and destination parent folder ID.

## Steps

1. Parse arguments: `<profile> <file-id> <new-parent-id>`
2. Call the `mcp__google-workspace__drive_move_file` tool with:
   - `profile`: the profile name
   - `file_id`: the file or folder ID
   - `new_parent_id`: the destination folder ID
3. Confirm the move with the file name and new location
