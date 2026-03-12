---
name: gws-gmail-operations
description: Gmail operations using gws CLI — create draft emails, list messages, read threads. Use when the user needs to draft, read, or manage emails.
---

# Gmail Operations via gws CLI

All commands require a profile. Prefix every command with:
```bash
GOOGLE_WORKSPACE_CLI_CONFIG_DIR=~/.config/gws/profiles/<profile> gws ...
```

## Create a Draft Email

Gmail API requires the message in RFC 2822 format, base64url-encoded in the `raw` field.

### Step 1: Build the raw message

```bash
RAW=$(printf 'From: sender@example.com\r\nTo: recipient@example.com\r\nSubject: Your Subject\r\nContent-Type: text/plain; charset="UTF-8"\r\n\r\nEmail body goes here.' | base64 -w 0 | tr '+/' '-_' | tr -d '=')
```

### Step 2: Create the draft

```bash
gws gmail users drafts create \
  --params '{"userId": "me"}' \
  --json "{\"message\": {\"raw\": \"$RAW\"}}"
```

### Helper: One-liner draft creation

```bash
gws gmail users drafts create \
  --params '{"userId": "me"}' \
  --json "{\"message\": {\"raw\": \"$(printf 'To: recipient@example.com\r\nSubject: Hello\r\nContent-Type: text/plain; charset=\"UTF-8\"\r\n\r\nMessage body' | base64 -w 0 | tr '+/' '-_' | tr -d '=')\"}}"
```

## Create a Draft with CC/BCC

```bash
RAW=$(printf 'To: main@example.com\r\nCc: cc@example.com\r\nBcc: bcc@example.com\r\nSubject: Subject Line\r\nContent-Type: text/plain; charset="UTF-8"\r\n\r\nBody text.' | base64 -w 0 | tr '+/' '-_' | tr -d '=')

gws gmail users drafts create \
  --params '{"userId": "me"}' \
  --json "{\"message\": {\"raw\": \"$RAW\"}}"
```

## Create a Draft Reply (in-thread)

To reply in an existing thread, include `In-Reply-To`, `References` headers and the `threadId`:

```bash
RAW=$(printf 'To: recipient@example.com\r\nSubject: Re: Original Subject\r\nIn-Reply-To: <original-message-id>\r\nReferences: <original-message-id>\r\nContent-Type: text/plain; charset="UTF-8"\r\n\r\nReply body.' | base64 -w 0 | tr '+/' '-_' | tr -d '=')

gws gmail users drafts create \
  --params '{"userId": "me"}' \
  --json "{\"message\": {\"raw\": \"$RAW\", \"threadId\": \"<thread-id>\"}}"
```

## List Drafts

```bash
gws gmail users drafts list --params '{"userId": "me", "maxResults": 10}'
```

## List Messages (Inbox)

```bash
gws gmail users messages list --params '{
  "userId": "me",
  "maxResults": 10,
  "q": "is:inbox"
}'
```

## Search Messages

```bash
gws gmail users messages list --params '{
  "userId": "me",
  "q": "from:someone@example.com subject:quarterly after:2026/01/01"
}'
```

## Read a Message

```bash
gws gmail users messages get --params '{
  "userId": "me",
  "id": "<message-id>",
  "format": "full"
}'
```

## List Labels

```bash
gws gmail users labels list --params '{"userId": "me"}'
```

## Batch Draft Creation Across Accounts

```bash
for profile in $(ls ~/.config/gws/profiles/); do
  RAW=$(printf 'To: team@example.com\r\nSubject: Weekly Update\r\nContent-Type: text/plain; charset="UTF-8"\r\n\r\nUpdate from '$profile' account.' | base64 -w 0 | tr '+/' '-_' | tr -d '=')
  GOOGLE_WORKSPACE_CLI_CONFIG_DIR=~/.config/gws/profiles/$profile \
    gws gmail users drafts create \
      --params '{"userId": "me"}' \
      --json "{\"message\": {\"raw\": \"$RAW\"}}" &
done
wait
```

## Important Notes

- `userId` is always `"me"` (authenticated user)
- The `q` parameter uses Gmail search syntax: https://support.google.com/mail/answer/7190
- Draft creation returns the draft ID — save it if you need to update or send later
- RFC 2822 headers must use `\r\n` line endings
- Base64url encoding: standard base64, then replace `+` with `-`, `/` with `_`, remove `=` padding
