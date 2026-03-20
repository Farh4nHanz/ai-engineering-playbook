# Debugging Example: Missing await in Async Flow

This example demonstrates how to use the **Senior Debugger Skill** to identify and fix a real bug.

---

## 🧩 Input

```ts
// service
export const getUserProfile = async (userId) => {
  const user = userRepository.findById(userId);

  if (!user) {
    throw new Error("User not found");
  }

  return {
    id: user.id,
    email: user.email,
  };
};
```

### Error / Unexpected Behavior

```md
- Sometimes returns undefined fields (id, email)
- No error thrown even when user does not exist
```

---

## 🔍 Root Cause

The `userRepository.findById` call is **missing `await`**, so `user` is a Promise, not the actual user object.

---

## 💡 Why It Happens

Because `findById` is asynchronous, it returns a Promise.

Without `await`:

* `user` becomes a Promise
* The condition `if (!user)` always evaluates to false (Promise is truthy)
* Accessing `user.id` and `user.email` results in `undefined`

---

## ✅ Fix

```ts
// Before
const user = userRepository.findById(userId);

// After (fixed)
const user = await userRepository.findById(userId);
```

---

## ⚠️ Related Issues

* Missing error handling for repository failures (e.g., DB errors)
* No input validation for `userId`
* Potential need for typed return to prevent undefined access