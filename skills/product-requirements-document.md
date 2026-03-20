---
name: product-requirements-document
description: Create a well-structured, lightweight PRD with deep technical clarity. Use this to define features clearly for a small team, including product, engineering, and QA perspectives.
tags:
  - prd
  - product
  - planning
  - backend
  - frontend
  - architecture
version: 1.0.0
---

# Product Requirements Document (PRD) Skill

This skill simulates a **senior product manager + tech lead** working together to define a feature clearly, quickly, and with enough depth for implementation.

---

## When to Use

- Defining a new feature before development
- Aligning product, engineering, and QA
- Preparing implementation-ready specifications
- Reducing ambiguity before coding starts

---

## PRD Philosophy

- Keep it **lightweight but complete**
- Optimize for **clarity and execution**, not documentation overhead
- Cover both:
  - **Product thinking** (why, user value)
  - **Technical clarity** (how it will be built)
- Make it immediately usable by developers and testers

---

## Phase 0: Clarification (MANDATORY)

Before writing the PRD, ask:

### Required Questions

1. **Feature Overview**
   - What feature are we building?
   - What problem does it solve?

2. **Target Users**
   - Who will use this feature?

3. **Core Use Cases**
   - What are the main actions users will take?

4. **Scope Boundaries**
   - What is explicitly **in scope** and **out of scope**?

5. **Constraints**
   - Any constraints? (deadline, tech, integrations, etc.)

6. **Existing System Context**
   - Does this feature integrate with an existing system?
   - If yes, what does the current architecture look like?

7. **Technical Preferences (Optional)**
   - Any preferred stack?
   - If not, recommend a suitable stack

Do NOT proceed until clarified.

---

## Phase 1: PRD Generation

Generate the PRD in **Markdown format** with clear sections.

---

# 📄 Product Requirements Document

## 1. Overview

- **Feature Name**
- **Summary**
- **Problem Statement**
- **Goals**

---

## 2. Users & Use Cases

### Target Users
- Describe primary users

### User Stories
- As a [user], I want to [action], so that [value]

---

## 3. Scope

### In Scope
- Bullet list

### Out of Scope
- Bullet list

---

## 4. Functional Requirements

| ID  | Requirement | Description |
| --- | ----------- | ----------- |

---

## 5. Edge Cases

- List important edge cases explicitly

---

## 6. UX / Flow Description

- Step-by-step flow of user interaction
- Focus on behavior and logic (no UI design)

---

## 7. Technical Design

### Architecture Overview
- High-level system design
- Recommend stack if not provided

---

### Data Model / Schema

```sql
-- Example schema (if applicable)
```

Or JSON if NoSQL:

```json
{}
```

---

### API Design

| Method | Endpoint | Description |
| ------ | -------- | ----------- |

Example:

```http
POST /api/v1/resource
```

Request / Response examples:

```json
{}
```

---

### Key Technical Decisions

* Explain trade-offs briefly
* Justify chosen approach

---

## 8. Non-Functional Requirements

* Performance
* Security
* Scalability
* Reliability

---

## 9. Success Metrics

* Define measurable KPIs:

  * Response time
  * Conversion rate
  * Error rate

---

## 10. Testing Considerations

* Key scenarios to test
* Edge cases to validate
* Failure conditions

---

## 11. Implementation Checklist

* [ ] Data model created
* [ ] API implemented
* [ ] Frontend implemented
* [ ] Tests written
* [ ] Documentation updated

---

## Execution Rules

* Ask clarifying questions first
* Keep explanations concise but meaningful
* Prefer **tables for structured data**
* Keep it **implementation-ready**
* Avoid unnecessary verbosity

---

## Technical Guidance Rules

* Recommend a stack if none is provided
* Prefer scalable and maintainable solutions
* Highlight assumptions clearly
* Avoid over-engineering

---

## Anti-Patterns to Avoid

* Vague requirements
* Missing edge cases
* Lack of technical clarity
* Overly long documentation
* Mixing unrelated features

---

## Goal

Produce a PRD that is:

* Clear enough for a developer to start immediately
* Structured enough for team alignment
* Deep enough to avoid ambiguity
* Lightweight enough to stay practical