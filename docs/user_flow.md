# Career Console – Intended User Experience (MVP v1)

This document defines the planned user experience for Career Console MVP v1.

The purpose is to ensure product alignment between architecture and implementation.

---

# 1. First-Time Setup

## 1.1 Upload Base Resume

User uploads or pastes their primary resume.

System:
- Converts resume into StructuredResumeJSON
- Creates a new ResumeProfile
- Creates Base version (immutable)
- Clones Base into Active version

User sees:
- Structured resume editor
- Deterministic template preview

Base is never modified automatically.

---

# 2. Managing Resume Profiles

User can:
- Create new profile from existing profile
- Rename profile
- Switch between profiles

Each profile contains:
- One immutable Base version
- One editable Active version

Active is used for tailoring.
Base is preserved as canonical truth.

---

# 3. Evaluating a Job

## 3.1 Paste Job Description

User copies full job description and pastes into input field.

User selects profile to evaluate against.

User clicks: "Evaluate Job"

System:
- Sends raw job description + profile summary to AI
- AI extracts structured JobSummary
- System stores Job and JobSummary
- System calculates match score

User sees:
- Match score (0–100)
- Seniority alignment
- Required skills
- Missing skills
- Apply recommendation

Raw description is not re-sent in future calls.

---

# 4. Tailoring Resume for Job

User clicks: "Tailor Resume"

System:
- Starts from selected profile Active version
- Sends Base/Active summary + JobSummary to AI
- AI suggests section-level rewrites

System does NOT:
- Allow AI to modify immutable fields
- Allow AI to change companies, dates, degrees

User sees:
- Side-by-side diff view
- Highlighted changes (added / removed / modified)

User can:
- Accept all
- Accept per section
- Reject changes

Upon approval:
- Tailored resume is generated
- Active profile is NOT automatically overwritten

---

# 5. Exporting Resume

User clicks: "Export PDF"

System:
- Uses deterministic resume template
- Renders from structured JSON
- No AI involvement in PDF generation

Exported resume reflects approved tailored content.

---

# 6. Submitting Application

User marks job as Applied.

System:
- Creates Application record
- Stores resumeSnapshot (exact structured JSON used)
- Sets status to "applied"

Application snapshot is immutable.

---

# 7. Application Tracking

User can:
- View table of applications
- Filter by status
- Filter by no-response duration
- Update status manually

Statuses:
- saved
- applied
- recruiter_screen
- interview
- offer
- rejected
- ghosted

---

# 8. Job Workspace (Contextual AI Q&A)

Inside each Job view:

User can ask:
- Clarification questions
- Salary strategy questions
- Skill alignment questions
- Resume refinement follow-ups

System:
- Sends JobSummary + Profile summary + last N messages
- Logs AIUsage

Conversation is stored per job.

---

# 9. AI Cost Visibility

User sees:
- AI Spend This Month
- Total calls
- Total tokens

If spending cap reached:
- AI actions disabled
- User notified

---

# MVP Experience Principles

Career Console should feel:

- Fast
- Deterministic
- Safe
- Transparent
- Controlled
- Minimal friction

It is not:

- A resume generator
- A job board
- An analytics dashboard

It is an AI-assisted application console.

This document defines the intended MVP experience for Career Console v1.

