---
name: assess-agent-guidance-files
description: Evaluate AGENTS.md or CLAUDE.md for repo agentic readiness. Use when the user asks for an assessment of the agentic readiness of a repository, or when they ask for an evaluation of AGENTS.md or CLAUDE.md files.
license: MIT
---

# Skill: assess-agent-guidance-files

## Purpose

Evaluate whether a repository’s `AGENTS.md` / `CLAUDE.md` files are:

- Concise (≤150 lines ideal, ≤200 max)
- Complete enough to guide an agent
- Properly structured using **modular, linked documentation** where appropriate
- Making the repo's **main features and business rules** discoverable through modular documentation referenced from the agent files

This skill assesses **content quality and coverage**, not adherence to a specific file structure or naming convention.

---

## Core Principles

1. **Conciseness is enforced**
   - AGENTS/CLAUDE files are **not documentation dumps**
   - They are **entrypoints + rules + pointers**

2. **Modular documentation is preferred**
   - Detailed content should live outside the agent files when needed
   - Agent files should reference deeper documentation instead of duplicating it

3. **Focus on intent, not structure**
   - Do **not enforce specific file names or layouts**
   - Evaluate whether required topics are clearly documented somewhere in the repo

---

## Evaluation Rules

### 1. Presence & Structure

- Root must contain:
  - `AGENTS.md` (preferred), or
  - `CLAUDE.md`

- Nested agent files should **only be considered if explicitly referenced**:
  - from the root agent file, or
  - from another agent file

- Do **not assume** the existence of nested agent files based on folder structure alone

#### Monorepo Requirement

If monorepo:
- Root file should reference subprojects and/or their agent files (if they exist)
- Each significant project should be either:
  - explicitly covered by the root file, or
  - have its own agent guidance file

---

### 2. Length Constraints (Strict)

For every AGENTS/CLAUDE file:

| Condition | Result |
|----------|--------|
| ≤150 lines | PASS |
| 151–200 lines | WARN |
| >200 lines | FAIL |

- Count raw lines (not visual wrapping)
- Applies to root and nested files

---

### 3. Tech Stack Coverage

Agent guidance should clearly specify the tech stack, including:

- Language(s)
- Framework(s)
- Database(s)
- Message brokers (if applicable)

Evaluation rules:
- PASS: All relevant parts are explicitly mentioned
- WARN: Partially specified or missing non-critical components
- FAIL: Missing or too vague

---

### 4. Architecture Coverage

Agent guidance should describe the intended architecture and key design patterns, where applicable.

Examples:
- Domain-driven design
- Hexagonal architecture
- CQRS
- Event-driven architecture
- Microservices / modular monolith
- Micro-frontends
- Layered / clean architecture
- MVVM / MVI / MVP (mobile)
- Coordinator / Router pattern (iOS)
- Clean Architecture (Android / iOS)

Evaluation rules:
- PASS: Architecture clearly described
- WARN: Vague or implicit
- FAIL: Missing when architecture is non-trivial

---

### 5. Performance Requirements

Agent guidance should include **concrete, testable expectations**. Examples vary by stack:

- **Backend:** N+1 query avoidance, pagination rules, latency targets (e.g., p95), rate limits
- **Frontend:** Bundle size limits, Core Web Vitals targets, rendering performance constraints
- **Mobile:** App startup time, memory limits, battery impact, offline/connectivity constraints

Evaluation rules:
- PASS: Specific and actionable
- WARN: Present but vague
- FAIL: Missing

---

### 6. Testing Strategy

Agent guidance should define or reference a testing strategy.

This may include:
- Test types (unit, integration, e2e, etc.)
- Expectations for when tests are required
- How to run tests

Important:
- Full details may live in separate documentation
- The agent file should at least **point to or summarize it**

Evaluation rules:
- PASS: Clearly defined or referenced
- FAIL: Missing entirely

---

### 7. Endpoint & Event Documentation

The repository should contain documentation for:

- API endpoints
- Async events / messaging (if applicable)

Important:
- Do **not require specific formats or filenames**
- Accept any clear documentation approach:
  - OpenAPI / Swagger
  - AsyncAPI
  - Markdown docs
  - Inline docs if clearly structured

Evaluation rules:
- PASS: Clearly documented and discoverable
- FAIL: Missing when APIs or async systems exist
- N/A: Repository is a pure consumer (FE, mobile, CLI) and owns no APIs — check is skipped and excluded from scoring

---

### 8. Security & Authentication

Agent guidance should specify the repository's security-relevant constraints and protections, including authentication when applicable.

This may include:
- Authentication mechanism (e.g., JWT, OAuth, sessions)
- Service-to-service authentication (if applicable)
- Authorization model and privilege boundaries
- Secret handling rules (environment variables, vaults, local overrides, no hardcoded credentials)
- Restrictions on production data, PII, or regulated data
- Network/tooling constraints for agents (external calls, shell access, destructive commands)
- Security-sensitive operational rules (rate limits, audit expectations, approval gates, dependency/update expectations)
- **Mobile-specific:** secure local storage (Keychain on iOS, Keystore on Android), certificate pinning rules, deep link / URL scheme validation, sensitive OS permissions (camera, location, notifications)

Important:
- Do **not** require a full security policy in AGENTS.md
- Accept concise guidance plus links to deeper documentation
- Only require topics that are relevant to the repository

Evaluation rules:
- PASS: Security and authentication expectations are clearly defined or referenced
- WARN: Partially defined, vague, or missing non-critical details
- FAIL: Missing when the repo clearly involves secrets, sensitive data, production access, privileged operations, external integrations, user authentication/authorization, or device-level sensitive data / privileged OS permissions

---

### 9. Feature & Business Rule Documentation

The repository should have **modular documentation** that describes its main features and the business rules that govern them, so an agent can understand *what the system does* and *which invariants matter* — not just the stack and architecture.

- The agent file does **not** need to enumerate features inline. A single link to a discoverable docs location (e.g., `docs/features/`, `docs/domain/`, an ADR index, or equivalent) is sufficient.
- "Main features" = the capabilities a user/consumer of the system relies on, not internal utilities. Examples:
  - **Backend:** bounded contexts, use cases, domain workflows, key invariants
  - **Frontend:** top-level user journeys / screens / flows
  - **Mobile:** primary screens and flows, offline behavior, sync rules
- "Business rules" = constraints that aren't obvious from the code's type signatures (eligibility, pricing, ordering, validation thresholds, state transitions, etc.)

Important:
- Do **not** require specific filenames or folder layouts
- Accept any modular doc layout, as long as it is discoverable from the agent files

Evaluation rules:
- PASS: Modular feature docs exist AND the agent file links to them (directly or via a docs index)
- WARN: Some features are documented but coverage is partial, OR docs exist but are not referenced from the agent files
- FAIL: Repo has non-trivial business logic and no feature documentation, or feature docs exist but are not discoverable from the agent files
- N/A: Repo is a thin library / SDK / CLI with no meaningful business logic — check skipped and excluded from scoring

---

## Required Assessment Artifact

At the end of every assessment, create or update this file in the assessed repository root:

```text
AGENTIC_READINESS_ASSESSMENT.md
```

Use `references/AGENTIC_READINESS_ASSESSMENT.template.md` as the required structure for the artifact.

The artifact must include:

- Assessment date in `YYYY-MM-DD` format using the local current date
- A rating table with columns: `Section`, `Rating`, `Evidence`, `Notes`
- One table row for each assessment section listed below
- Ratings limited to `PASS`, `WARN`, `FAIL`, or `N/A`
- Final assessment rating formatted exactly as `XX/100`
- 3-5 concise, prioritized recommendations tied to observed gaps

Keep `N/A` sections in the table, but exclude them from score calculation.
Include root file presence and monorepo coverage evidence in the `Presence & Structure` row.

After writing the artifact, report the final score and the path to `AGENTIC_READINESS_ASSESSMENT.md` to the user.

### Assessment Sections

Use these section names in the table:

1. Presence & Structure
2. Length Constraints
3. Tech Stack Coverage
4. Architecture Coverage
5. Performance Requirements
6. Testing Strategy
7. Endpoint & Event Documentation
8. Security & Authentication
9. Feature & Business Rule Documentation

---

## Evaluation Logic

### Detect Monorepo

Indicators:
- Workspace configurations (pnpm, yarn, npm workspaces, turbo, nx, lerna)
- Multi-module build systems (Maven, Gradle)

---

### Infer Tech Stack

Check:
- package.json, lock files
- requirements.txt, pyproject.toml
- go.mod, Cargo.toml
- pom.xml, build.gradle
- Docker configs
- pubspec.yaml (Flutter / Dart)
- build.gradle / *.xcodeproj / Package.swift (Android / iOS native)
- capacitor.config.* (Capacitor / Ionic)
- metro.config.* (React Native)

Compare inferred stack with what agent guidance declares.

---

### Infer Architecture (Conservative)

Look for:
- domain/, application/, infrastructure/
- CQRS patterns
- event handlers
- service boundaries

Only assert when evidence is strong.

---

### Detect Documentation

Search broadly for:
- API specs or descriptions
- Event/message documentation
- Markdown docs
- Config/spec files

Do not depend on specific file names.

---

### Infer Features (Stack-Aware + README Cross-Check)

Goal: build a rough list of the repo's main features, then check whether they are documented and discoverable from the agent files.

**Stack-aware signals to collect:**
- Backend (DDD / hexagonal): top-level folders under `domain/`, `application/`, `use_cases/`, `features/`, aggregate roots, command/query handlers
- Backend (layered): controller / router files, service classes, top-level endpoint groupings
- Frontend (Next.js / React / Vue): top-level routes (`app/`, `pages/`, `src/routes/`), feature folders
- Mobile (iOS / Android / RN / Flutter): screens, flows, navigation graphs, coordinator / router definitions

**README cross-check:**
- Parse the root `README.md` (and any top-level `OVERVIEW.md` / product doc) for feature-like headings or bullet lists ("Features", "What it does", "Capabilities")
- Reconcile against the code-derived list:
  - Features in README but not in code → stale README, but still counts as documented intent
  - Features in code but not in README and not in modular docs → strong signal of undocumented features

**Discoverability check:**
- Verify that modular docs exist for the main features (any of: `docs/features/`, `docs/domain/`, `docs/use-cases/`, ADR directory, or feature-named markdown files anywhere under `docs/`)
- Verify the agent file links to that location (direct path or via a referenced docs index)

Only assert FAIL when business logic is clearly non-trivial. Thin libraries, SDKs, and pure CLIs with no domain logic get N/A.

---

## Scoring Model

| Area | Weight |
|------|--------|
| Structure & presence | 15 |
| Length compliance | 10 |
| Monorepo coverage | 10 |
| Tech stack | 15 |
| Architecture | 10 |
| Performance | 10 |
| Testing | 10 |
| Docs (API/events) | 5 |
| Security & auth | 5 |
| Feature & business rule docs | 10 |

### Verdict

- PASS: ≥85
- PARTIAL PASS: 65–84
- FAIL: <65 or any hard failure

---

## Hard Fail Conditions

- No root AGENTS.md / CLAUDE.md
- Any file >200 lines
- No testing mention
- No security/authentication guidance when clearly relevant
- Monorepo with unclear or missing project coverage
- Missing API/event documentation when clearly required
- Non-trivial business logic with no modular feature/business-rule documentation, or feature docs that are not discoverable from the agent files

---

## Smart Remediation Rules (CRITICAL)

### DO NOT

- Enforce a specific AGENTS.md structure
- Require specific filenames (e.g., TESTING.md, ARCHITECTURE.md)
- Suggest duplicating large content into AGENTS.md

### DO

- Suggest improving clarity and coverage of missing topics
- Suggest moving detailed content to separate documentation
- Suggest referencing existing documentation where appropriate

---

## Example Fixes

### Missing Testing

FAIL: Testing strategy missing

Suggested fix:
- Add a short "Testing" section in AGENTS.md describing expectations
- If detailed testing documentation exists elsewhere, reference it
- If not, create a dedicated document and link to it

---

### File Too Long

FAIL: AGENTS.md is 260 lines

Suggested fix:
- Reduce file to ≤150 lines
- Move detailed sections (architecture, testing, etc.) into separate documentation
- Keep summaries and references in AGENTS.md

---

### Missing Architecture Clarity

WARN: Architecture not clearly defined

Suggested fix:
- Add a short section describing the intended architecture and key patterns
- Keep it high-level; detailed explanations can live elsewhere

---

### Missing Feature Documentation

FAIL: Repo has clear business logic (e.g., use cases under `application/`) but no feature docs and no link from AGENTS.md

Suggested fix:
- Create `docs/features/` (or equivalent) with one short markdown per main feature — purpose, key business rules, links to relevant code/ADRs
- Add a single line in AGENTS.md, e.g.: "Features and business rules: see `docs/features/`"
- Do NOT inline feature descriptions in AGENTS.md

---

## Monorepo Guidance

- Root file should provide global guidance
- Subprojects should be:
  - explicitly covered, or
  - have their own guidance

- Avoid duplication:
  - Subproject files should only define differences or specifics

---

## Key Philosophy

AGENTS.md is a control layer, not a full documentation system

It should:
- Guide decisions
- Define constraints
- Point to deeper documentation where needed
