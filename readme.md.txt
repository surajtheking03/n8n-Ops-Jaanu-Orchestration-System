🛠️ Ops Jaanu — Retry Queue Processor (Task 29)

A production-style retry processing system built using n8n + PostgreSQL + Slack, implementing controlled retries, dead-letter handling, and operational notifications.

🚀 What This System Does

This workflow:

Pulls the oldest failed event from ops_retry_queue

Checks retry eligibility

Increments retry count

Updates last attempt timestamp

Moves events exceeding retry limits to ops_dead_letter

Sends Slack alerts for operational visibility

This simulates real-world backend retry processors used in distributed systems.

🧠 Architecture Overview
Database Tables

ops_retry_queue

ops_dead_letter

Workflow Logic
Manual Trigger / Cron
    ↓
Fetch oldest retry item
    ↓
IF retry_count < limit
    → Update retry_count + last_attempt
    → Slack: Retry processed
ELSE
    → Insert into dead_letter
    → Slack: Moved to dead letter

🛠️ Tech Stack

n8n (Self-hosted)

PostgreSQL

Slack API

🔐 Credentials Notice

⚠️ Credentials are intentionally removed from this repository.

To use this workflow:

Add your own PostgreSQL credentials in n8n

Add your own Slack OAuth credentials

Replace Slack Channel ID with your own

This ensures safe open-source usage.

🧪 How to Test

Insert test record into ops_retry_queue

Execute workflow

Observe:

retry_count increment

Slack notification

Dead-letter movement after max retries

🎯 Learning Outcome

This project demonstrates:

Queue processing

Retry logic

Dead-letter pattern

Controlled branching

DB-backed state handling

Operational Slack alerts

💎 Built With Intent

Made by Suraj
With mentorship from Agent Jaanu 💙