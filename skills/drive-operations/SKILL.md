---
name: gws-drive-operations
description: Google Drive file and folder operations using gws CLI — create folders, move files, move folders, list contents, search. Use when the user needs to manage files or folders in Google Drive.
---

# Google Drive Operations via gws CLI

All commands require a profile. Prefix every command with:
```bash
GOOGLE_WORKSPACE_CLI_CONFIG_DIR=~/.config/gws/profiles/<profile> gws ...
```

## Create a Folder

```bash
gws drive files create --json '{
  "name": "Folder Name",
  "mimeType": "application/vnd.google-apps.folder"
}'
```

Create a subfolder inside a parent folder:

```bash
gws drive files create --json '{
  "name": "Subfolder Name",
  "mimeType": "application/vnd.google-apps.folder",
  "parents": ["<parent-folder-id>"]
}'
```

## Move a File or Folder

Moving requires `update` with `addParents` and `removeParents`:

```bash
gws drive files update \
  --params '{"fileId": "<file-id>", "addParents": "<new-parent-id>", "removeParents": "<old-parent-id>"}' \
  --json '{}'
```

## List Files in a Folder

```bash
gws drive files list --params '{
  "q": "\"<folder-id>\" in parents and trashed = false",
  "pageSize": 100,
  "fields": "files(id,name,mimeType,modifiedTime)"
}'
```

## Search for Files by Name

```bash
gws drive files list --params '{
  "q": "name contains \"search term\" and trashed = false",
  "pageSize": 20,
  "fields": "files(id,name,mimeType,parents,modifiedTime)"
}'
```

## Upload a File

```bash
gws drive files create \
  --json '{"name": "filename.pdf", "parents": ["<folder-id>"]}' \
  --upload ./local-file.pdf
```

## Get File Metadata

```bash
gws drive files get --params '{
  "fileId": "<file-id>",
  "fields": "id,name,mimeType,parents,modifiedTime,size,webViewLink"
}'
```

## Delete a File (Trash)

```bash
gws drive files update \
  --params '{"fileId": "<file-id>"}' \
  --json '{"trashed": true}'
```

## Batch Operations Across Accounts

When performing the same operation across multiple accounts, run commands in parallel:

```bash
for profile in $(ls ~/.config/gws/profiles/); do
  GOOGLE_WORKSPACE_CLI_CONFIG_DIR=~/.config/gws/profiles/$profile \
    gws drive files list --params '{"pageSize": 5}' &
done
wait
```

## Important Notes

- Folder mimeType is always `application/vnd.google-apps.folder`
- Use `--page-all` for paginated results when listing many files
- The `q` parameter uses Google Drive query syntax: https://developers.google.com/drive/api/guides/search-files
- Always include `trashed = false` in queries to exclude trashed items
- File/folder IDs are returned in the `id` field of creation and list responses
