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

Evaluation rules:
- PASS: Architecture clearly described
- WARN: Vague or implicit
- FAIL: Missing when architecture is non-trivial

---

### 5. Performance Requirements

Agent guidance should include **concrete, testable expectations**, such as:

- Avoid N+1 queries
- Pagination requirements
- Latency targets (e.g., p95)
- Load-time expectations
- Resource constraints

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

Important:
- Do **not** require a full security policy in AGENTS.md
- Accept concise guidance plus links to deeper documentation
- Only require topics that are relevant to the repository

Evaluation rules:
- PASS: Security and authentication expectations are clearly defined or referenced
- WARN: Partially defined, vague, or missing non-critical details
- FAIL: Missing when the repo clearly involves secrets, sensitive data, production access, privileged operations, external integrations, or user authentication/authorization

---

## Evaluation Output Format

Verdict: PASS | PARTIAL PASS | FAIL  
Score: X/100  

Checks:

1. Root agent file
   - Found: yes/no
   - Lines: N
   - Status: PASS/WARN/FAIL

2. Monorepo structure
   - Detected: yes/no
   - Coverage: adequate/incomplete
   - Status: PASS/FAIL

3. Tech stack
   - Status: PASS/WARN/FAIL
   - Missing: [...]

4. Architecture
   - Status: PASS/WARN/FAIL

5. Performance
   - Status: PASS/WARN/FAIL

6. Testing
   - Status: PASS/FAIL

7. Docs (API/events)
   - Status: PASS/FAIL

8. Security & authentication
   - Status: PASS/WARN/FAIL
   - Missing: [...]

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

## Scoring Model

| Area | Weight |
|------|--------|
| Structure & presence | 20 |
| Length compliance | 10 |
| Monorepo coverage | 15 |
| Tech stack | 15 |
| Architecture | 10 |
| Performance | 10 |
| Testing | 10 |
| Docs | 5 |
| Security & auth | 5 |

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
