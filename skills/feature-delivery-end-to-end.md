---
name: feature-delivery-end-to-end
description: Plan and implement a feature end-to-end like a senior engineering lead. Use this for building features from idea to production with proper sequencing, validation, and iteration.
tags:
  - feature
  - backend
  - frontend
  - planning
  - architecture
  - fullstack
version: 1.0.0
---

# Feature Delivery End-to-End Skill

This skill combines **sprint planning** and **guided implementation**. It simulates a senior engineer leading feature development from idea → design → implementation → validation.

---

## When to Use

- Building a new feature from scratch
- Implementing features with clear structure and discipline
- Breaking down work into manageable tasks
- Ensuring production-ready delivery with minimal rework

---

## Execution Phases

### Phase 0: Clarification (MANDATORY)

Before writing any code:

- Ask clarifying questions about:
  - Requirements and scope
  - Edge cases
  - Existing system constraints
  - Data flow and integrations
- Do NOT assume missing details
- Wait for confirmation before proceeding

---

### Phase 1: Task Breakdown (Sprint Planning)

Break the feature into small engineering tasks.

#### Rules

- Each task must be completable in **under 1 day**
- Order tasks by **dependency**
- Mark tasks that can be done in **parallel**
- Assign size:
  - **S** → small
  - **M** → medium
  - **L** → large
- Identify **1–2 highest technical risk tasks**

#### Output Format

| Task | Depends On | Size | Parallel | Risk Notes |

---

### Phase 2: Guided Implementation

After task approval, implement step-by-step.

---

## Implementation Order (STRICT)

Follow this exact order:

1. **Data Model / Schema**
2. **Backend Logic & API**
3. **Frontend Components**
4. **Tests**
5. **Documentation**

---

## Step-by-Step Execution Rules

For EACH step:

1. Implement only that layer (no jumping ahead)
2. Follow existing codebase patterns strictly
3. Do NOT introduce new dependencies without approval
4. Every function must include appropriate documentation:
   - JSDoc (JavaScript/TypeScript)
   - Docstring (Python)
   - Javadoc (Java)
   - Other inline comments based on language
5. Keep changes minimal and focused

After each step:

- Show what was built
- Explain key decisions briefly
- STOP and wait for approval before continuing

---

## Code Quality Constraints

- Follow existing architecture (e.g., layered architecture, service pattern)
- Use domain-specific naming (avoid `utils`, `helpers`, etc.)
- Keep functions small and focused
- Avoid deep nesting (max 3 levels)
- Handle errors explicitly
- Ensure input validation at boundaries

---

## Risk Awareness

During implementation:

- Call out risky assumptions
- Highlight potential edge cases
- Suggest safer alternatives when needed
- Do NOT over-engineer, stay pragmatic

---

## Testing Expectations

- Include meaningful test cases:
  - Happy path
  - Edge cases
  - Failure scenarios
- Keep tests aligned with existing testing patterns

---

## Documentation Updates

- Update only what is relevant:
  - API documentation
  - README (if needed)
  - Inline documentation (e.g., JSDoc, Docstring)

---

## Output Behavior Summary

1. Ask clarifying questions
2. Break feature into subtasks (table format)
3. Wait for approval
4. Implement step-by-step:
   - Schema → Backend → Frontend → Tests → Docs
5. Pause after each step for approval

---

## Anti-Patterns to Avoid

- Skipping clarification phase
- Jumping directly into coding
- Large, multi-layer changes in one step
- Introducing unnecessary dependencies
- Ignoring existing project conventions
- Over-engineering simple features

---

## Goal

Deliver production-ready features with:

- Clear planning
- Controlled execution
- High code quality
- Minimal rework