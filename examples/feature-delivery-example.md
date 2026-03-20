# Feature Delivery Example: Create Category API

This example demonstrates how to use the **Feature Delivery End-to-End Skill** to plan and implement a feature step-by-step.

---

## 🧩 Input

```md
You are using the "feature-delivery-end-to-end" skill.

Feature:
Create a category management system (CRUD).

Context:
- Backend: Node.js (Express)
- Architecture: layered (controller → service → repository)
- Database: PostgreSQL
```

---

## ❓ Clarification Phase

```md
1. What fields should a category have?
2. Should slug be auto-generated?
3. Any validation rules?
4. Should soft delete be used?
5. Pagination needed for listing?
```

---

## ✅ Clarified Answers

```md
1. id, name, slug, description, created_at
2. Yes, slug should be auto-generated from name
3. Name must be unique and required
4. No, hard delete
5. Yes, pagination required
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

## 📋 Task Breakdown

| Task                           | Depends On | Size | Parallel | Risk Notes          |
| ------------------------------ | ---------- | ---- | -------- | ------------------- |
| Design category schema         | -          | S    | No       | Low                 |
| Implement repository methods   | Schema     | M    | No       | Medium (DB queries) |
| Implement service logic        | Repository | M    | No       | Medium (validation) |
| Implement controller endpoints | Service    | S    | Yes      | Low                 |
| Add pagination support         | Repository | S    | Yes      | Medium              |
| Write unit tests (service)     | Service    | M    | Yes      | Medium              |
| Write integration tests (API)  | Controller | M    | Yes      | Medium              |
| Update documentation           | All        | S    | Yes      | Low                 |

**High Risk Tasks:**

* Repository queries (data consistency)
* Service validation (unique constraints)

---

## 🏗️ Step 1: Data Model / Schema

```sql
CREATE TABLE categories (
  id UUID PRIMARY KEY,
  name TEXT UNIQUE NOT NULL,
  slug TEXT UNIQUE NOT NULL,
  description TEXT,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

---

## ⚙️ Step 2: Backend Logic (Service)

```ts
/**
 * Create a new category
 */
export const createCategory = async (input) => {
  const { name, description } = input;

  if (!name) {
    throw new Error("Name is required");
  }

  const slug = slugify(name, { lower: true });

  const existing = await categoryRepository.findBySlug(slug);
  if (existing) {
    throw new Error("Category already exists");
  }

  return categoryRepository.create({
    name,
    slug,
    description,
  });
};
```

---

## 🌐 Step 3: Controller

```ts
/**
 * Handle create category request
 */
export const createCategoryHandler = async (req, res) => {
  try {
    const category = await createCategory(req.body);
    return res.status(201).json(category);
  } catch (err) {
    return res.status(400).json({ message: err.message });
  }
};
```

---

## 🧪 Step 4: Unit Test (Service)

```ts
test("should create category successfully", async () => {
  categoryRepository.findBySlug.mockResolvedValue(null);
  categoryRepository.create.mockResolvedValue({ id: "1", name: "Books" });

  const result = await createCategory({ name: "Books" });

  expect(result.name).toBe("Books");
});
```

---

## 📄 Step 5: Documentation (API)

### POST /categories

Create a new category

```json
// Request:
{
  "name": "Books",
  "description": "All books"
}

// Response:
{
  "id": "uuid",
  "name": "Books",
  "slug": "books"
}
```