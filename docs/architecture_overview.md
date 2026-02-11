# Career Console – High Level Architecture Overview

## Architectural Philosophy

Career Console is designed as a single-user, full-stack, AI-assisted application console.

Core principles:
- Deterministic data model first
- AI as an augmentation layer, not the source of truth
- Backend-controlled AI orchestration
- Strict cost governance
- Modular structure for future expansion

This architecture supports long-term extensibility while remaining lightweight for a single-user MVP.

---

# 1. System Overview

Career Console consists of three primary layers:

1. Frontend (React Application)
2. Backend API (Node service layer)
3. AI Provider (External API)

High-level flow:

User → Frontend → Backend → AI Provider → Backend → Database → Frontend

The backend is responsible for:
- Context construction
- AI request orchestration
- Token + cost tracking
- Data persistence
- Guardrail enforcement

The frontend is responsible for:
- Resume editing UI
- Job evaluation UI
- Workspace chat interface
- Application tracker table
- Spend visibility dashboard

---

# 2. Core Modules (Backend-Oriented Design)

The backend is organized by domain modules rather than technical layers.

/modules
  /resume
  /jobs
  /workspace
  /applications
  /ai
  /usage

Each module contains:
- Route definitions
- Service logic
- Type definitions
- Validation logic

This keeps concerns isolated and scalable.

---

# 3. Data Ownership Model

AI is stateless.

Career Console is stateful.

All persistent memory lives inside the application database:
- Structured resume data
- Resume versions
- Job descriptions
- Job summaries
- Evaluation results
- Workspace conversation history
- Application statuses
- AI usage logs

Every AI call reconstructs context from stored data.

---

# 4. AI Orchestration Layer

The AI module contains:

- Provider abstraction interface
- Context builder service
- Cost calculation service
- Token guardrails
- Usage logging

The provider interface allows future model replacement without altering business logic.

Example abstraction concept:

AiProvider
  - evaluateJob()
  - generateResumeSuggestions()
  - generateCoverLetter()
  - workspaceResponse()

This prevents vendor lock-in and keeps the architecture modular.

---

# 5. Context Strategy (High-Level)

To prevent token ballooning:

- Job descriptions are summarized once
- Structured job summary reused
- Resume summary reused
- Sliding window for workspace messages
- Hard token caps enforced

Only relevant context is included per request.

---

# 6. Cost Governance Layer

Every AI call logs:
- Prompt tokens
- Completion tokens
- Total tokens
- Calculated cost
- Associated job (if applicable)

Guardrails include:
- Monthly hard spending cap
- Per-request token cap
- Usage meter visible in UI
- Hard stop when cap exceeded (manual override optional)

This makes AI cost predictable and controlled.

---

# 7. Persistence Layer

Initial MVP database:
- SQLite

Future upgrade path:
- PostgreSQL

ORM layer will abstract database implementation.

All entities are relationally connected:
- Resume ↔ ResumeVersion
- Job ↔ JobSummary
- Job ↔ WorkspaceMessages
- Job ↔ Application
- AIUsage ↔ Job (optional relation)

---

# 8. Separation of Concerns

Frontend does not:
- Call AI directly
- Calculate AI cost
- Build context payloads

Backend does not:
- Render resume templates
- Handle UI state logic

This clean separation improves testability and long-term maintainability.

---

# 9. Expansion Path (Post-MVP)

The architecture supports future additions without refactor:

- Board view for tracker
- Analytics dashboard
- Multi-user authentication
- Hosted SaaS deployment
- Usage tiering
- Extended interview simulation

Because AI orchestration and cost governance are centralized, scaling features does not compromise control.

---

# Summary

Career Console MVP architecture is:

- Modular
- Backend-driven for AI control
- Cost-governed
- Context-disciplined
- Structured-data first

It is intentionally designed as a production-grade single-user system that can expand without architectural overhaul.

