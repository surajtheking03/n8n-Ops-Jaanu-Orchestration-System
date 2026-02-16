🛠️ n8n Ops Jaanu – Production Retry & Control Plane System

A production-style operational orchestration system built using:

n8n (Self-Hosted)

PostgreSQL

Slack

Structured DB-backed state management

Modular workflow architecture

This repository combines:

Task 29 — Retry Queue Processor

Task 30 — Mini Ops Control Plane

Together, they simulate a realistic operational backend system with retries, dead-letter routing, centralized control state, and Slack observability.

📦 System Overview

This project demonstrates how to design a real-world operational system with:

Controlled retries

Dead-letter queue

Centralized DB-backed control state

Slack operational visibility

Modular workflow architecture

Sub-workflow orchestration

This is not a demo flow.

This is production-thinking applied inside n8n.

🔁 Task 29 — Retry Queue Processor
🎯 Purpose

Simulate a production-grade retry engine with dead-letter handling.

🗄️ Database Tables
ops_retry_queue

Stores retryable events.

id SERIAL PRIMARY KEY
event_id TEXT NOT NULL
payload JSONB NOT NULL
retry_count INT DEFAULT 0
last_attempt TIMESTAMP
created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP

ops_dead_letter

Stores permanently failed events.

id SERIAL PRIMARY KEY
event_id TEXT NOT NULL
payload JSONB NOT NULL
failure_reason TEXT
moved_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP

🔄 Workflow Logic

Fetch oldest event from ops_retry_queue

Check:

If retry_count < max_retry_limit
→ Increment retry count
→ Update last_attempt
→ Notify Slack

Else
→ Insert into ops_dead_letter
→ Notify Slack (Moved to Dead Letter)

💬 Slack Visibility

Retry fired

Retry count incremented

Event moved to dead letter

🏗️ Production Concepts Demonstrated

Controlled retry policy

Dead letter queue pattern

SQL-based state persistence

Slack observability

Error routing architecture

🖥️ Task 30 — Mini Ops Control Plane
🎯 Purpose

Introduce centralized operational control over the retry system.

Simulates how real production systems:

Pause processing

Disable retries

Adjust retry limits dynamically

🗄️ Control Table
ops_control_state
id SERIAL PRIMARY KEY
retries_enabled BOOLEAN DEFAULT true
max_retry_limit INT DEFAULT 3
system_paused BOOLEAN DEFAULT false
updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP

🧠 Control Logic

The Control Plane:

Reads system state from ops_control_state

If system_paused = true
→ Slack notification: Processing paused

If retries_enabled = false
→ Slack notification: Retries disabled

If system healthy
→ Calls Task 29 as a sub-workflow

🔗 Modular Architecture

Task 30 uses:

Execute Sub-Workflow → Calls Task 29

This creates:

Separation of concerns

Reusable retry processor

Central orchestration control

🏛️ Final Architecture
Manual Trigger / Schedule Trigger
        ↓
Read ops_control_state
        ↓
IF paused → Slack notification
        ↓
IF retries disabled → Slack notification
        ↓
Else → Call Retry Processor (Task 29)
        ↓
Retry logic + Dead letter handling
        ↓
Slack operational logging

🧪 How To Use

Import both JSON workflows into n8n

Set up PostgreSQL tables

Insert initial rows into:

ops_retry_queue

ops_control_state

Add your Slack credentials

Run Task 30 (Control Plane)

🔐 Credentials Notice

All credentials have been intentionally removed from the JSON files before publishing.

You must configure your own:

PostgreSQL credentials

Slack credentials

Database user permissions (including sequence usage)

No sensitive information is stored in this repository.

🧠 What This Demonstrates (Interview Ready)

This project showcases:

Production-grade retry design

Dead-letter queue pattern

DB-backed orchestration control

Modular workflow architecture

Sub-workflow execution

Slack-based observability

Real operational system simulation inside n8n

This is no longer automation.

This is orchestration engineering.

🚀 Portfolio Impact

This system reflects:

Systems thinking

Failure handling strategy

Operational visibility

Backend reliability architecture

n8n used beyond simple automations

❤️ Credits

Created by Suraj
With love and support from Agent Jaanu