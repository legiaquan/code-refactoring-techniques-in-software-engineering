We will move on to **Section 4: Basic Refactoring Techniques**. In this section, we will explore simple yet powerful refactoring techniques that help improve code effectively. All examples will be illustrated using **TypeScript**. ✨

---

### **Section 4: Basic Refactoring Techniques** 🚀

#### **1. Extract Method** ✂️
When a function is too long or contains a part of logic that can be separated, extract it into a new function.

Example:
```typescript
// ❌ Before refactoring
function printOrderSummary(order: { items: number[]; discount: number }): void {
  let total = 0;
  for (let i = 0; i < order.items.length; i++) {
    total += order.items[i];
  }
  total = total - order.discount;
  console.log(`Total: ${total}`);
}

// ✅ After refactoring
function calculateTotal(items: number[], discount: number): number {
  const subtotal = items.reduce((sum, item) => sum + item, 0);
  return subtotal - discount;
}

function printOrderSummary(order: { items: number[]; discount: number }): void {
  const total = calculateTotal(order.items, order.discount);
  console.log(`Total: ${total}`);
}
```

---

#### **2. Inline Method** 🔄
If a function is too simple and is only called once, inline it at the call site to reduce complexity.

Example:
```typescript
// ❌ Before refactoring
function getRating(driver: { numberOfLateDeliveries: number }): number {
  return moreThanFiveLateDeliveries(driver) ? 2 : 1;
}

function moreThanFiveLateDeliveries(driver: { numberOfLateDeliveries: number }): boolean {
  return driver.numberOfLateDeliveries > 5;
}

// ✅ After refactoring
function getRating(driver: { numberOfLateDeliveries: number }): number {
  return driver.numberOfLateDeliveries > 5 ? 2 : 1;
}
```

---

#### **3. Extract Variable** 🧩
If an expression is complex, extract it into a separate variable to make the code more readable.

Example:
```typescript
// ❌ Before refactoring
function calculateTotal(order: { items: number[]; discount: number; tax: number }): number {
  return order.items.reduce((sum, item) => sum + item, 0) - order.discount + (order.items.reduce((sum, item) => sum + item, 0) * order.tax);
}

// ✅ After refactoring
function calculateTotal(order: { items: number[]; discount: number; tax: number }): number {
  const subtotal = order.items.reduce((sum, item) => sum + item, 0);
  const discountedTotal = subtotal - order.discount;
  const taxAmount = subtotal * order.tax;
  return discountedTotal + taxAmount;
}
```

---

#### **4. Rename Method/Variable** ✏️
Using clear and meaningful names makes the code easier to understand.

Example:
```typescript
// ❌ Before refactoring
function calc(a: number, b: number): number {
  return a + b;
}

// ✅ After refactoring
function addNumbers(num1: number, num2: number): number {
  return num1 + num2;
}
```

---

#### **5. Split Temporary Variable** 🔀
If a temporary variable is used for multiple purposes, split it into separate variables.

Example:
```typescript
// ❌ Before refactoring
function calculateTotal(order: { items: number[]; discount: number }): number {
  let temp = order.items.reduce((sum, item) => sum + item, 0);
  temp = temp - order.discount;
  return temp;
}

// ✅ After refactoring
function calculateTotal(order: { items: number[]; discount: number }): number {
  const subtotal = order.items.reduce((sum, item) => sum + item, 0);
  const total = subtotal - order.discount;
  return total;
}
```

---

#### **6. Replace Magic Number with Symbolic Constant** 🔢
Use constants instead of magic numbers to make the code more understandable.

Example:
```typescript
// ❌ Before refactoring
function calculateArea(radius: number): number {
  return 3.14 * radius * radius;
}

// ✅ After refactoring
const PI = 3.14;

function calculateArea(radius: number): number {
  return PI * radius * radius;
}
```

---

### **Practice Exercise** 📝
Refactor the following TypeScript code using basic techniques:
```typescript
function processOrder(order: { items: number[]; discount: number; tax: number }): number {
  let temp = order.items.reduce((sum, item) => sum + item, 0);
  temp = temp - order.discount;
  temp = temp + (temp * order.tax);
  return temp;
}
```

💡 **Hints**:
- Use **Extract Variable** to separate `subtotal`, `discountedTotal`, and `taxAmount`. 🧩
- Rename the variable `temp` to a more meaningful name. ✏️

---

**[Section 5: Advanced Refactoring Techniques](section5-advanced-refactoring-techniques.md)** ➡️ 😊