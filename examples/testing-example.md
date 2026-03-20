# Testing Example: Category Service

This example demonstrates how to use the **Testing Engineer Skill** to design and implement high-quality tests.

---

## 🧩 Input

```md
You are using the "testing-engineer" skill.

Target:
Test the Category Service

Context:
- Backend: Node.js
- Architecture: layered (controller → service → repository)
- Framework: Vitest
- Mock database interactions
- Focus on unit and integration tests
```

---

## ❓ Clarification Phase

```md
1. Should we include E2E tests or only unit + integration?
2. What coverage level is expected?
3. What should be mocked?
4. Any existing test structure to follow?
```

---

## ✅ Clarified Answers

```md id="gk8zv2"
1. Unit and integration only
2. High coverage (near enterprise level)
3. Mock repository and external dependencies
4. Use existing Vitest setup
```

---

## ⚠️ Note on Clarification Phase

In this example, additional context is already provided in the input.  
Because of that, the AI may skip the clarification phase and proceed directly.

If you want the AI to **ask clarification questions first (recommended)**:
- Remove or reduce the provided context
- Keep the input more ambiguous

This allows the skill to follow its intended workflow:
**Clarification → Planning → Execution**

---

## 📋 Test Plan

### Scope

* Category Service logic
* Category API endpoints (basic integration)

### Layers Covered

* Unit tests (service)
* Integration tests (controller + service)

### Mocking Strategy

* Mock:

  * Category repository
* Do NOT mock:

  * Service logic (for unit tests)

### Key Scenarios

**Happy Path**

* Create category successfully
* Fetch categories with pagination

**Edge Cases**

* Duplicate category name
* Missing required fields

**Failure Cases**

* Repository throws error
* Invalid input

---

## 🧪 Unit Test (Service)

```ts
import { describe, test, expect, vi } from "vitest";
import { createCategory } from "@/services/category.service";

const categoryRepository = {
  findBySlug: vi.fn(),
  create: vi.fn(),
};

vi.mock("@/repositories/category.repository", () => ({
  categoryRepository,
}));

describe("Category Service", () => {
  test("should create category successfully", async () => {
    categoryRepository.findBySlug.mockResolvedValue(null);
    categoryRepository.create.mockResolvedValue({
      id: "1",
      name: "Books",
      slug: "books",
    });

    const result = await createCategory({ name: "Books" });

    expect(result.name).toBe("Books");
  });

  test("should throw error if category already exists", async () => {
    categoryRepository.findBySlug.mockResolvedValue({ id: "1" });

    await expect(createCategory({ name: "Books" }))
      .rejects
      .toThrow("Category already exists");
  });

  test("should throw error when name is missing", async () => {
    await expect(createCategory({}))
      .rejects
      .toThrow("Name is required");
  });
});
```

---

## 🔗 Integration Test (Controller)

```ts
import request from "supertest";
import app from "@/app";

describe("POST /categories", () => {
  test("should create category successfully", async () => {
    const response = await request(app)
      .post("/categories")
      .send({ name: "Books" });

    expect(response.status).toBe(201);
    expect(response.body.name).toBe("Books");
  });

  test("should return 400 if name is missing", async () => {
    const response = await request(app)
      .post("/categories")
      .send({});

    expect(response.status).toBe(400);
  });
});
```

---

## 📊 Coverage Notes

* Covered:

  * Service logic (validation, duplication check)
  * Controller behavior (request → response)
* Edge cases included (missing input, duplicates)
* Failure scenarios validated

**Potential Gaps:**

* Pagination logic not deeply tested
* No E2E coverage (acceptable for current scope)

---

## 💡 Additional Suggestions

* Add tests for:

  * Pagination behavior
  * Database failure scenarios (timeouts, connection issues)
* Consider adding E2E tests later for full user flows
* Add coverage threshold (e.g., 90%+) in CI