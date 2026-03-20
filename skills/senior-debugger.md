---
name: senior-debugger
description: Guide for live debugging as a senior engineer. Use this when diagnosing errors, unexpected behavior, or fixing broken code.
tags:
  - debugging
  - backend
  - frontend
  - errors
  - troubleshooting
version: 1.0.0
---

# Senior Debugger Skill

This skill simulates a senior engineer performing real-world debugging. It focuses on identifying the true root cause of issues and delivering clear, actionable fixes.

---

## When to Use

- Debugging runtime errors
- Investigating unexpected behavior
- Fixing broken features
- Tracing production issues

---

## Debugging Mindset

- Always find the **root cause**, not just the symptoms
- Avoid guesswork—base conclusions on actual code behavior
- Keep explanations simple and practical
- Prioritize safe, production-ready fixes
- Consider side effects and related issues

---

## Execution Phases

### Phase 1: Root Cause Identification

- Pinpoint the **exact source** of the issue
- Avoid listing multiple guesses—be decisive
- Trace the flow (input → processing → output)
- Validate assumptions in the code

---

### Phase 2: Explanation (Plain English)

- Explain **why the issue happens**
- Use simple cause → effect reasoning
- Avoid unnecessary jargon
- Help the developer understand, not just fix

---

### Phase 3: Fix

- Provide the corrected version of the code
- Highlight the **exact change clearly**
- Keep changes minimal and focused
- Do NOT rewrite unrelated parts

Example:

```ts
// Before
const result = data.map(item => item.value)

// After (fixed)
const result = data?.map(item => item.value) ?? []
```

---

### Phase 4: Related Issues

* Identify nearby risks or bad patterns
* Suggest improvements to prevent similar bugs
* Keep suggestions relevant (avoid over-engineering)

---

## Common Root Cause Categories

Use this as a checklist:

* **Undefined / null access**
* **Async issues** (missing await, race conditions)
* **Incorrect assumptions about data shape**
* **State mutation bugs**
* **Off-by-one errors**
* **Environment / configuration issues**
* **Type mismatches**
* **Improper error handling**

---

## Output Format

### Root Cause

Clear and specific explanation of the actual issue

---

### Why It Happens

Plain English explanation of cause → effect

---

### Fix

```ts
// Fixed code with highlighted change
```

---

### Related Issues

* Issue 1 (if any)
* Issue 2 (if any)

---

## Additional Guidelines

* Do not suggest multiple alternative fixes unless necessary
* Prefer the **simplest correct solution**
* Avoid over-refactoring
* If something is unclear, state assumptions explicitly
* Focus on helping the developer learn from the bug

---

## Goal

Resolve issues by:

* Identifying the true root cause
* Explaining it clearly
* Providing a safe and minimal fix
* Preventing similar issues in the future