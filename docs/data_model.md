# Career Console – Data Model (MVP v1)

This document defines the structural data model for Career Console MVP v1.

The goal of this schema is:
- Deterministic resume storage
- Safe AI-assisted editing
- Snapshot-based application tracking
- Cost-governed AI usage logging
- Clean expansion path without refactor

---

# 1. Resume Profiles

Career Console supports multiple resume profiles.

A profile represents a strategic baseline (e.g., "Vue Base", "React Base", "Startup Lean").

Each profile contains:
- One immutable Base version
- One editable Active version

---

## 1.1 ResumeProfile

```ts
ResumeProfile {
  id: string
  name: string               // "Vue Base", "React Base", etc.
  createdAt: Date
  updatedAt: Date
}
```

---

## 1.2 ResumeVersion

Each ResumeProfile has exactly two versions:

- type: "base" (immutable)
- type: "active" (editable)

```ts
ResumeVersion {
  id: string
  profileId: string
  type: "base" | "active"
  content: StructuredResumeJSON
  createdAt: Date
  updatedAt: Date
}
```

### Rules

- Base version is never modified by AI.
- Active version may be updated only after user approval.
- Tailored resumes for jobs do NOT overwrite Base.
- Active may be intentionally updated by user.

---

# 2. StructuredResumeJSON

Resume content is stored as structured JSON.

Template layout is deterministic and not stored in the database.

```ts
StructuredResumeJSON {
  header: {
    name: string
    email: string
    phone?: string
    linkedin?: string
    github?: string
  }

  summary: string

  skills: SkillGroup[]

  experience: ExperienceItem[]

  education: EducationItem[]

  projects?: ProjectItem[]
}
```

---

## Supporting Types

```ts
SkillGroup {
  category: string
  items: string[]
}
```

```ts
ExperienceItem {
  company: string
  roleTitle: string
  startDate: string
  endDate: string
  bullets: string[]
}
```

```ts
EducationItem {
  institution: string
  degree: string
  graduationDate?: string
}
```

```ts
ProjectItem {
  name: string
  description: string
  technologies?: string[]
}
```

---

# 3. Job

Represents a job posting entered by the user.

```ts
Job {
  id: string
  company: string
  role: string
  rawDescription: string
  applicantCount?: number
  salaryMin?: number
  salaryMax?: number
  locationType?: "remote" | "hybrid" | "onsite"
  createdAt: Date
  updatedAt: Date
}
```

---

# 4. JobSummary

Generated once during Job Evaluation.

Raw job description should not be repeatedly sent to AI.

```ts
JobSummary {
  id: string
  jobId: string
  requiredSkills: string[]
  preferredSkills: string[]
  seniorityLevel: string
  responsibilities: string[]
  techEmphasis?: string
  generatedAt: Date
}
```

---

# 5. Application

Represents a submitted application.

Each application stores a snapshot of the resume used at time of submission.

```ts
Application {
  id: string
  jobId: string
  profileId: string
  status: ApplicationStatus
  dateApplied?: Date
  lastActivity?: Date
  notes?: string

  resumeSnapshot: StructuredResumeJSON

  createdAt: Date
  updatedAt: Date
}
```

---

## ApplicationStatus Enum

```
saved
applied
recruiter_screen
interview
offer
rejected
ghosted
```

---

# 6. WorkspaceMessage

Each job has its own contextual AI workspace.

```ts
WorkspaceMessage {
  id: string
  jobId: string
  role: "user" | "assistant"
  content: string
  createdAt: Date
}
```

Context rules:
- Only last N messages included in AI calls.
- Older messages may be summarized in future versions.

---

# 7. AIUsageLog (Cost Governance)

Every AI request is logged.

```ts
AIUsageLog {
  id: string
  jobId?: string
  model: string
  promptTokens: number
  completionTokens: number
  totalTokens: number
  cost: number
  createdAt: Date
}
```

---

# 8. Relationships Overview

- ResumeProfile 1 → 2 ResumeVersions (base + active)
- Job 1 → 1 JobSummary
- Job 1 → N WorkspaceMessages
- Job 1 → 1 Application
- Job 1 → N AIUsageLog (optional relation)

---

# 9. MVP Design Constraints

- No multi-user support
- No deep version trees
- No template storage in database
- No analytics tables (yet)
- No AI-generated layout control

The schema is intentionally minimal and structured to support:

- Safe AI-assisted editing
- Snapshot-based historical accuracy
- Cost transparency
- Future analytics expansion without refactor

This document defines the locked MVP data model for Career Console v1.

