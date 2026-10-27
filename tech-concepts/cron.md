---
title: Cron and Scheduled Jobs
tags: [infrastructure, automation]
updated: 2026-10-27
---

# Cron and Scheduled Jobs

Cron is the classic Unix scheduler: it runs commands at fixed times or intervals. Its five-field schedule syntax is now used almost everywhere scheduled work happens, from GitHub Actions to cloud functions.

## How it works

A cron expression has five fields:

```
┌──────── minute (0-59)
│ ┌────── hour (0-23)
│ │ ┌──── day of month (1-31)
│ │ │ ┌── month (1-12)
│ │ │ │ ┌ day of week (0-6, Sunday = 0)
│ │ │ │ │
0 9 * * 1-5     → 9:00 every weekday
*/15 * * * *    → every 15 minutes
0 0 1 * *       → midnight on the 1st of each month
```

A daemon wakes up every minute, checks each schedule, and runs the jobs that match.

## Key terms

- **crontab**: the per-user table of scheduled jobs (`crontab -e` to edit).
- **Time zone**: most hosted schedulers run in **UTC** unless you say otherwise, so 9 AM New York time is `0 13` in summer and `0 14` in winter.
- **launchd**: macOS's replacement for cron, configured with `.plist` files.
- **Idempotent job**: running it twice does no harm. Schedulers sometimes run a job late, twice, or not at all, so this matters.

## In practice

```bash
crontab -e
# Back up notes every night at 2 AM, logging output
0 2 * * * /usr/local/bin/backup-notes >> ~/backup.log 2>&1
```

[crontab.guru](https://crontab.guru) translates any expression into plain English.

## Common pitfalls

- Jobs that work in a terminal but fail under cron, because cron runs with a minimal `PATH` and environment. Use absolute paths.
- Forgetting the UTC offset, or daylight saving time shifting a job by an hour.
- No logging, so failures go unnoticed.
- Everyone scheduling at minute 0, which piles load onto the top of the hour. Pick an odd minute.

## Related

- [CI/CD](ci-cd.md)
- [Serverless](serverless.md)
