---
name: gws-drive-operations
description: Google Drive file and folder operations using MCP tools — create folders, move files, move folders, list contents, search. Use when the user needs to manage files or folders in Google Drive.
---

# Google Drive Operations via MCP Tools

All operations require a `profile` parameter (e.g., "work", "CRM1").

## List Files / Search

Use `mcp__google-workspace__drive_list_files` with:
- `profile`: account profile name
- `query`: Google Drive search query (e.g., `name contains 'report'`)
- `folder_id`: parent folder ID to list contents of
- `page_size`: number of results (default 20)

## Create a Folder

Use `mcp__google-workspace__drive_create_folder` with:
- `profile`: account profile name
- `name`: folder name
- `parent_id`: parent folder ID (optional, omit for root)

## Move a File or Folder

Use `mcp__google-workspace__drive_move_file` with:
- `profile`: account profile name
- `file_id`: ID of the file or folder to move
- `new_parent_id`: ID of the destination folder

## Upload a File

Use `mcp__google-workspace__drive_upload_file` with:
- `profile`: account profile name
- `local_path`: local file path to upload
- `name`: file name in Drive (defaults to local filename)
- `parent_id`: parent folder ID (optional)

## Get File Metadata

Use `mcp__google-workspace__drive_get_file` with:
- `profile`: account profile name
- `file_id`: file or folder ID

## Delete a File (Trash)

Use `mcp__google-workspace__drive_delete_file` with:
- `profile`: account profile name
- `file_id`: file or folder ID to trash

## Bulk Operations Across Accounts

Use `mcp__google-workspace__bulk_drive_list` with:
- `profiles`: comma-separated profile names, or "all"
- `query`: Drive search query

## Important Notes

- Folder mimeType is always `application/vnd.google-apps.folder`
- The `query` parameter uses Google Drive query syntax: https://developers.google.com/drive/api/guides/search-files
- Always include `trashed = false` in queries to exclude trashed items
- File/folder IDs are returned in the `id` field of creation and list responses
