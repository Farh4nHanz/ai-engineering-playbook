---
name: clean-code-refactor
description: Refactor messy code using clean code principles and best practices. Use this when improving readability, structure, and maintainability of existing code.
tags:
  - refactor
  - clean-code
  - backend
  - frontend
  - maintainability
version: 1.0.0
---

# Clean Code Refactor Skill

This skill simulates a senior engineer specializing in transforming messy code into clean, maintainable, and production-quality code.

---

## When to Use

- Improving messy or legacy code
- Preparing code for production or review
- Reducing complexity and duplication
- Enhancing readability and maintainability

---

## Refactoring Mindset

- Focus on **clarity over cleverness**
- Improve structure without changing behavior
- Make the code easier for future developers to understand
- Prefer small, safe, incremental improvements
- Avoid over-engineering

---

## Refactoring Goals

Apply these systematically:

### 1. SOLID Principles

- **Single Responsibility** → Each function/class should have one clear purpose
- **Open/Closed** → Extend behavior without modifying existing logic unnecessarily
- **Liskov Substitution** → Ensure consistent behavior across abstractions
- **Interface Segregation** → Avoid forcing unused dependencies
- **Dependency Inversion** → Depend on abstractions, not concrete implementations (when applicable)

---

### 2. Function & Structure Improvements

- Extract functions if:
  - Function exceeds **20 lines**
  - It does more than one thing
- Break complex logic into smaller, named units
- Reduce nesting (max 3 levels)

---

### 3. Naming Improvements

- Replace vague names:
  - `data`, `temp`, `value`, `handler`
- Use **domain-specific, descriptive names**
- Ensure function names describe **intent**, not implementation

---

### 4. Remove Magic Values

- Replace:
  - Magic numbers
  - Hardcoded strings
- Use named constants with clear meaning

---

### 5. Remove Duplication

- Extract repeated logic into reusable functions
- Centralize shared behavior
- Avoid copy-paste patterns

---

## Refactoring Rules

- Do NOT change external behavior
- Do NOT introduce new dependencies
- Keep consistency with existing architecture
- Avoid unnecessary abstractions
- Keep refactor minimal but impactful

---

## Output Format

### Before

```ts
// Original code (unchanged)
```

---

### After

```ts
// Refactored code
```

---

### Refactor Decisions

List each change with a **one-sentence explanation**:

* Extracted function `X` to separate responsibility
* Renamed `foo` → `calculateTotalPrice` for clarity
* Replaced magic number `7` with `MAX_RETRY_COUNT`
* Removed duplicated logic by creating `Y`
* Simplified nested condition using early return

---

## Additional Guidelines

* Prefer early returns over nested conditions
* Keep functions under ~20 lines where possible
* Balance readability vs abstraction (avoid over-splitting)
* If trade-offs exist, favor maintainability

---

## Goal

Transform messy code into code that is:

* Readable
* Maintainable
* Scalable
* Easy to reason about

Without altering its behavior.