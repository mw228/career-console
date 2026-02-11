# Career Console – Application Assistant MVP (v1 Locked Scope)

## Product Identity

Single-user AI-powered Application Assistant focused on:

- Smarter job evaluation
- Safe, structured resume tailoring
- Contextual AI workspace per job
- Clean application tracking
- Transparent AI cost control

This MVP does NOT include analytics dashboards, multi-user support, SaaS billing, or browser extensions.

---

# 1. Profile + Resume Studio (Structured & Safe)

## Core Capabilities

- Structured resume schema (no raw text blobs)
- Section-based editing:
  - Header
  - Summary
  - Skills (grouped)
  - Experience (bullet arrays)
  - Projects (optional if time allows)
- Resume versioning (create variants)
- Save version history
- Fixed, pre-designed professional resume template
- Live preview inside dashboard
- Stable PDF export

## AI Integration (Controlled)

- Bullet-level suggestions only
- Section-level improvement suggestions
- Accept / Reject diff system
- No full automatic resume rewrites
- Original baseline version always preserved

---

# 2. Job Evaluation Engine

## User Inputs

- Job description (pasted text)
- Applicant count (optional)
- Salary range (optional)
- Location type (optional)

## System Outputs (Structured)

- Match score (0–100)
- Seniority alignment
- Tech emphasis detection
- Missing keywords
- Apply recommendation:
  - Strong Yes
  - Conditional Yes
  - Low ROI

## Optimization Rules

- Job description trimmed before sending
- Job summarized once
- Structured job summary stored for reuse

---

# 3. Job Workspace (Contextual AI Console)

Each job has a dedicated workspace.

## Capabilities

- Context-aware Q&A
- Resume-aware reasoning
- Job-aware reasoning
- Stored conversation history per job
- Sliding conversation window (limit past messages)

## Context Strategy

Each AI call includes:

- System instruction
- Stored job summary (NOT raw description)
- Resume summary (NOT full resume unless required)
- Last N messages only

Older messages may be summarized if needed.

---

# 4. Application Kit Generator

From Job Evaluation:

- Targeted resume tweak suggestions
- Tailored cover letter draft
- Recruiter talking points

(No full interview prep module in MVP.)

---

# 5. Application Tracker (Operational Control)

## Application Fields

- Company
- Role
- Date applied
- Status
- Applicant count
- Notes
- Last activity date

## Status Enum

- saved
- applied
- recruiter_screen
- interview
- offer
- rejected
- ghosted

## View (MVP)

- Table view only
- Filters:
  - Outstanding only
  - No response > X days
  - By status
  - By date

(Board view deferred.)

---

# 6. AI Cost Tracking (NON-NEGOTIABLE MVP FEATURE)

## Backend Logging

Every AI request logs:

- model
- promptTokens
- completionTokens
- totalTokens
- calculated cost
- timestamp
- associated jobId (if applicable)

## Cost Calculation

- Model pricing stored in config
- Cost computed per request

## Dashboard Display

- AI Spend This Month
- Total calls
- Total tokens
- Cost per job (optional if time allows)

## Guardrails

- Monthly hard spending cap
- Per-request token cap
- Warning at configurable threshold
- Hard stop when cap reached (manual override optional)

---

# 7. AI Guardrails & Context Discipline

- Backend-only AI calls
- Max token size enforcement
- Trimmed job descriptions
- Job summary reuse
- Resume summary reuse
- Sliding conversation window
- Duplicate job hash detection

---

# Explicitly NOT in MVP

- Multi-user authentication
- SaaS billing
- Public hosting requirements
- Chrome extension
- Automatic job scraping
- Analytics dashboards
- Board view
- Deep interview simulator
- Notification system

---

# MVP Success Criteria

User can:

1. Maintain structured resume safely.
2. Evaluate a job with cost-controlled AI.
3. Ask contextual follow-up questions inside a job workspace.
4. Generate resume tweaks and cover letter.
5. Export a stable PDF.
6. Log and track applications cleanly.
7. See exactly how much AI usage costs in real time.

This document represents the locked scope for Career Console MVP v1.

