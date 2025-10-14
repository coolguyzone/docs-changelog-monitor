# Sentry Docs Monitor

This repository monitors the `getsentry/sentry-docs` repository for changes and triggers webhooks to update the Sentry Content Aggregator.

## How it works

### Real-time Monitoring (monitor-sentry-docs.yml)
- Runs every 15 minutes via GitHub Actions
- Checks for new commits in the `getsentry/sentry-docs` repository
- Filters for commits that change documentation files
- Triggers webhook to the main application for each new commit

### Daily Backfill (backfill-docs-feed.yml)
- Runs daily at 3 AM UTC via GitHub Actions
- Fetches all commits from the last 10 days
- Ensures the docs feed always contains recent changes
- Automatically repopulates the feed if data is lost
- Can be manually triggered via workflow_dispatch

## Setup

1. Fork or clone this repository
2. Add the following secrets in Settings → Secrets and variables → Actions:
   - `WEBHOOK_URL`: Your application's webhook URL
   - `WEBHOOK_SECRET`: Your webhook secret
3. Enable GitHub Actions in the Actions tab

## Monitoring

- Check the Actions tab to see when workflows run
- View logs to see what commits are being processed
- Both workflows will automatically start running once enabled

### Manual Backfill

If you need to immediately populate the feed (e.g., after data loss):

1. Go to the "Actions" tab
2. Select "Backfill Last 10 Days of Docs Changes"
3. Click "Run workflow"
4. Wait for completion (~2-5 minutes depending on commit volume)

## Configuration

### Real-time Monitoring
- Runs every 15 minutes
- Checks for new commits in the `master` branch since last check
- Only processes commits that change documentation files (`.md`, `.mdx`, files in `/docs/` directories)
- Triggers webhook only for relevant changes

### Daily Backfill
- Runs daily at 3 AM UTC
- Fetches all commits from the last 10 days
- Ensures feed always contains at least 10 days of history
- Filters for documentation file changes before sending webhooks
# Force workflow refresh
