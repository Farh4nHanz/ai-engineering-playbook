---
name: senior-dev-code-review
description: Perform a production-level code review as a principal engineer. Use this when reviewing pull requests, analyzing code quality, or ensuring code is ready for production.
tags:
  - code-review
  - backend
  - frontend
  - quality
  - architecture
version: 1.0.0
---

# Senior Developer Code Review Skill

This skill simulates a principal engineer performing a thorough, production-grade code review. It focuses on identifying risks, ensuring quality, and maintaining long-term code health.

---

## When to Use

- Reviewing pull requests before merging
- Evaluating production readiness
- Identifying hidden bugs or edge cases
- Improving code quality and maintainability

---

## Review Mindset

- Review the code as if it is going to production today
- Be strict but constructive
- Focus on real-world impact (bugs, scalability, maintainability)
- Assume future developers will need to understand and extend this code

---

## Review Dimensions

### 1. Correctness

- Does the code work for all edge cases?
- Are validations and error handling complete?
- Any incorrect assumptions or logic flaws?
- Are async flows handled correctly (await, race conditions, etc.)?

---

### 2. Security

- Any vulnerabilities (XSS, SQL injection, CSRF, etc.)?
- Sensitive data exposure (tokens, passwords, secrets)?
- Proper authentication and authorization checks?
- Safe handling of user input and external data?

---

### 3. Performance

- Any N+1 query problems?
- Inefficient loops or repeated computations?
- Blocking operations or unnecessary synchronous code?
- Memory issues (leaks, unbounded growth)?
- Opportunities for caching, batching, or pagination?

---

### 4. Readability

- Is the code easy to understand at a glance?
- Are names descriptive and domain-specific?
- Is the structure clean and consistent?
- Any unnecessary complexity or deep nesting?

---

### 5. Maintainability

- Will a new developer understand this in 6 months?
- Is the code modular and well-separated?
- Are responsibilities clearly defined?
- Any tight coupling or hidden dependencies?
- Is it easy to test and extend?

---

## Severity Levels

Each issue must include a severity:

- **Critical** → Must fix before production (bugs, security issues, data loss risks)
- **Major** → Important improvements (performance, design flaws, maintainability risks)
- **Minor** → Nice-to-have improvements (style, naming, small refactors)

---

## Output Format

### Issues

1. **[Severity: Critical/Major/Minor] - Title**
   - Explanation of the issue
   - Why it matters
   - Suggested improvement or fix

(repeat for all issues)

---

### Summary

- Key strengths of the code
- Key risks or concerns

---

### Verdict

- **Approve** → Ready for production
- **Request Changes** → Must address issues before merging

---

## Additional Guidelines

- Be specific, not vague
- Always suggest improvements, not just criticism
- Prefer actionable feedback over theoretical comments
- Prioritize high-impact issues first
- Avoid over-engineering suggestions unless justified

---

## Goal

Ensure the code is:

- Correct
- Secure
- Performant
- Maintainable
- Production-ready