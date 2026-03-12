---
name: gws-gmail-operations
description: Gmail operations using MCP tools — create draft emails, list messages, read threads. Use when the user needs to draft, read, or manage emails.
---

# Gmail Operations via MCP Tools

All operations require a `profile` parameter (e.g., "work", "CRM1").

## Create a Draft Email

Use `mcp__google-workspace__gmail_create_draft` with:
- `profile`: account profile name
- `to`: recipient email address(es), comma-separated
- `subject`: email subject
- `body`: email body text
- `cc`: CC recipients (optional)
- `bcc`: BCC recipients (optional)

## List Messages / Search

Use `mcp__google-workspace__gmail_list_messages` with:
- `profile`: account profile name
- `query`: Gmail search query (e.g., `is:inbox`, `from:user@example.com subject:quarterly`)
- `max_results`: max messages to return (default 10)

## Read a Message

Use `mcp__google-workspace__gmail_read_message` with:
- `profile`: account profile name
- `message_id`: Gmail message ID

## List Drafts

Use `mcp__google-workspace__gmail_list_drafts` with:
- `profile`: account profile name
- `max_results`: max drafts to return (default 10)

## List Labels

Use `mcp__google-workspace__gmail_list_labels` with:
- `profile`: account profile name

## Bulk Draft Creation Across Accounts

Use `mcp__google-workspace__bulk_gmail_draft` with:
- `profiles`: comma-separated profile names, or "all"
- `to`: recipient email(s)
- `subject`: email subject
- `body`: email body

## Important Notes

- The `query` parameter uses Gmail search syntax: https://support.google.com/mail/answer/7190
- Draft creation returns the draft ID — save it if you need to update or send later
