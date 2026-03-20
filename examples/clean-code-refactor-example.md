# Clean Code Refactor Example: Order Processing Function

This example demonstrates how to use the **Clean Code Refactor Skill** to improve messy code.

---

## 🧩 Input

```ts
function process(data) {
  let total = 0;

  for (let i = 0; i < data.length; i++) {
    if (data[i].price && data[i].qty) {
      total += data[i].price * data[i].qty;
    }
  }

  if (total > 1000) {
    return total - total * 0.1;
  } else {
    return total;
  }
}
```

---

## ❌ Before

```ts
function process(data) {
  let total = 0;

  for (let i = 0; i < data.length; i++) {
    if (data[i].price && data[i].qty) {
      total += data[i].price * data[i].qty;
    }
  }

  if (total > 1000) {
    return total - total * 0.1;
  } else {
    return total;
  }
}
```

---

## ✅ After

```ts
const DISCOUNT_THRESHOLD = 1000;
const DISCOUNT_RATE = 0.1;

/**
 * Calculate total price of items
 */
const calculateTotal = (items) => {
  return items.reduce((total, item) => {
    if (!item.price || !item.qty) return total;
    return total + item.price * item.qty;
  }, 0);
};

/**
 * Apply discount if eligible
 */
const applyDiscount = (total) => {
  if (total <= DISCOUNT_THRESHOLD) return total;
  return total - total * DISCOUNT_RATE;
};

/**
 * Process order
 */
export const processOrder = (items) => {
  const total = calculateTotal(items);
  return applyDiscount(total);
};
```

---

## 🧠 Refactor Decisions

* Renamed `process` → `processOrder` for clearer intent
* Extracted `calculateTotal` to separate calculation responsibility
* Extracted `applyDiscount` to isolate business rule logic
* Replaced magic numbers (`1000`, `0.1`) with named constants
* Replaced `for` loop with `reduce` for cleaner aggregation
* Used early return to simplify conditional logic
* Improved variable naming (`data` → `items`) for domain clarity