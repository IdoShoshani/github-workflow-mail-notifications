# Send Email on Push Workflow

This repository automates email notifications for every push to the `main` branch. It's designed for developers or teams to stay informed about updates without manual effort.

## What It Does

- Sends an email to the person who made the push.
- Includes:
  - List of changed files.
  - Commit details (author, message, and timestamp).
  - Links to the commit and repository.

## Why This is Useful

If you're new to GitHub Actions, this workflow:
- Demonstrates how automation can save time.
- Helps you learn how to set up and use GitHub Actions.
- Teaches how to integrate email notifications into your workflow.

## Quick Setup Guide

1. **Add Secrets** to your repository:
   - `GMAIL_USERNAME`: Your Gmail email address.
   - `GMAIL_APP_PASSWORD`: An app password from Gmail (learn how [here](https://support.google.com/accounts/answer/185833?hl=en)).
2. Push to `main` and check your inbox for an email!

## Workflow Code (Copy & Paste)

```yaml
name: Send Email on Push

on:
  push:
    branches:
      - main

jobs:
  send_email:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Send Email
        uses: dawidd6/action-send-mail@v3
        with:
          server_address: smtp.gmail.com
          server_port: 465
          username: ${{ secrets.GMAIL_USERNAME }}
          password: ${{ secrets.GMAIL_APP_PASSWORD }}
          subject: "New Push to Repository: ${{ github.repository }}"
          to: ${{ github.event.head_commit.committer.email }}
          from: ${{ secrets.GMAIL_USERNAME }}
          body: |
            Commit: ${{ github.sha }}
            Author: ${{ github.event.head_commit.committer.name }}
            Files Changed: ${{ join(github.event.head_commit.modified, '\n') }}
```

Now, every push will notify the committer with essential details—try it out!
