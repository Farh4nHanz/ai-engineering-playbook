# Code Review Example: Create User Endpoint

This example demonstrates how to use the **Senior Developer Code Review Skill** to review production-bound code.

---

## 🧩 Input

```ts
// controller
export const createUser = async (req, res) => {
  const { email, password } = req.body;

  const user = await userRepository.findByEmail(email);

  if (user) {
    return res.status(400).json({ message: "User already exists" });
  }

  const newUser = await userRepository.create({
    email,
    password,
  });

  return res.json(newUser);
};
```

---

## 🔍 Review

### Issues

1. **[Severity: Critical] - Password stored in plain text**

   * The password is saved directly without hashing
   * This is a major security vulnerability
   * **Fix**: Hash the password using a library like bcrypt before storing

---

2. **[Severity: Major] - Missing input validation**

   * No validation for email format or password strength
   * Invalid data could enter the system
   * **Fix**: Validate input at the controller or service layer

---

3. **[Severity: Major] - Business logic inside controller**

   * The controller directly interacts with the repository
   * Violates separation of concerns
   * **Fix**: Move logic to a service layer

---

4. **[Severity: Minor] - Missing proper HTTP status code**

   * Successful creation should return `201 Created`, not default `200`
   * **Fix**: Use `res.status(201).json(...)`

---

5. **[Severity: Minor] - No error handling**

   * If the database call fails, the request will crash
   * **Fix**: Wrap logic in try-catch or use global error handler

---

### Summary

**Strengths:**

* Simple and readable structure
* Handles duplicate user check

**Concerns:**

* Critical security flaw (plain text password)
* Missing validation and proper layering
* Lack of error handling

---

### Verdict

**Request Changes** → Must fix critical and major issues before merging