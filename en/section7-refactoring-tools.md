We will move on to **Section 7: Refactoring Tools**. In this section, we will explore tools and IDEs that support refactoring, helping you make source code changes quickly and safely. Specifically, we will focus on how to use these tools with **TypeScript**. ✨

---

### **Section 7: Refactoring Tools** 🛠️

#### **1. Refactoring Tools in IDEs** 💻
Modern IDEs like **Visual Studio Code (VS Code)**, **WebStorm**, and **IntelliJ IDEA** provide powerful refactoring features. Below are some common features:

##### **a. Rename Symbol** ✏️
- **Shortcut**: `F2` (VS Code) or `Shift + F6` (WebStorm/IntelliJ).
- **Example**: Rename a variable, function, or class and automatically update all references.

```typescript
// ❌ Before renaming
function calculateTotal(price: number, quantity: number): number {
  return price * quantity;
}

// ✅ After renaming (using Rename Symbol)
function computeTotal(price: number, quantity: number): number {
  return price * quantity;
}
```

##### **b. Extract Method** ✂️
- **Shortcut**: `Ctrl + .` (VS Code) or `Ctrl + Alt + M` (WebStorm/IntelliJ).
- **Example**: Extract a block of code into a new function.

```typescript
// ❌ Before extraction
function processOrder(order: { items: number[]; discount: number }): number {
  const subtotal = order.items.reduce((sum, item) => sum + item, 0);
  return subtotal - order.discount;
}

// ✅ After extraction (using Extract Method)
function calculateSubtotal(items: number[]): number {
  return items.reduce((sum, item) => sum + item, 0);
}

function processOrder(order: { items: number[]; discount: number }): number {
  const subtotal = calculateSubtotal(order.items);
  return subtotal - order.discount;
}
```

##### **c. Extract Variable** 🧩
- **Shortcut**: `Ctrl + .` (VS Code) or `Ctrl + Alt + V` (WebStorm/IntelliJ).
- **Example**: Extract a complex expression into a separate variable.

```typescript
// ❌ Before extraction
function calculateTotal(price: number, quantity: number, tax: number): number {
  return price * quantity + (price * quantity * tax);
}

// ✅ After extraction (using Extract Variable)
function calculateTotal(price: number, quantity: number, tax: number): number {
  const subtotal = price * quantity;
  const taxAmount = subtotal * tax;
  return subtotal + taxAmount;
}
```

---

#### **2. Static Analysis Tools** 🔍
Static analysis tools help detect potential issues in code and suggest refactoring.

##### **a. ESLint** 🛡️
- **Purpose**: Detect syntax errors, code smells, and violations of coding conventions.
- **Setup**:
  ```bash
  npm install eslint --save-dev
  npx eslint --init
  ```
- **Example ESLint configuration**:
  ```json
  {
    "rules": {
      "no-unused-vars": "error",
      "no-duplicate-imports": "error",
      "prefer-const": "warn"
    }
  }
  ```

##### **b. SonarQube** 🧭
- **Purpose**: Analyze code quality, detect bugs, code smells, and security vulnerabilities.
- **Integration with TypeScript**: Use the SonarTS plugin.

---

#### **3. Automated Refactoring Tools** 🤖
Some tools automate the refactoring process, saving you time.

##### **a. Prettier** 🎨
- **Purpose**: Automatically format code to ensure consistency.
- **Setup**:
  ```bash
  npm install prettier --save-dev
  ```
- **Example Prettier configuration**:
  ```json
  {
    "semi": true,
    "singleQuote": true,
    "tabWidth": 2
  }
  ```

##### **b. TypeScript Refactoring Tools** 🛠️
- **Purpose**: Plugins and extensions that support TypeScript refactoring.
- **Example**: The **TypeScript Hero** extension for VS Code helps with auto-imports and module organization.

---

#### **4. Automated Testing Tools** 🧪
Refactoring should be supported by automated tests to ensure existing functionality is not broken.

##### **a. Jest** 🧾
- **Purpose**: Write and run unit tests.
- **Example**:
  ```typescript
  // sum.ts
  export function sum(a: number, b: number): number {
    return a + b;
  }

  // sum.test.ts
  import { sum } from './sum';

  test('adds 1 + 2 to equal 3', () => {
    expect(sum(1, 2)).toBe(3);
  });
  ```

##### **b. Cypress** 🌐
- **Purpose**: Automate UI testing.
- **Example**:
  ```typescript
  describe('My First Test', () => {
    it('Visits the app', () => {
      cy.visit('/');
      cy.contains('Welcome');
    });
  });
  ```

---

### **Practice Exercise** 📝
Use the refactoring tools in your IDE to improve the following code:
```typescript
function processOrder(order: { items: number[]; discount: number; tax: number }): number {
  let total = 0;
  for (let i = 0; i < order.items.length; i++) {
    total += order.items[i];
  }
  total = total - order.discount;
  total = total + (total * order.tax);
  return total;
}
```

💡 **Hints**:
- Use **Extract Method** to separate the calculation logic. ✂️
- Use **Rename Symbol** to give variables clearer names. ✏️

---

### **Expected Result** 🎉
```typescript
function calculateSubtotal(items: number[]): number {
  return items.reduce((sum, item) => sum + item, 0);
}

function applyDiscount(total: number, discount: number): number {
  return total - discount;
}

function applyTax(total: number, tax: number): number {
  return total + (total * tax);
}

function processOrder(order: { items: number[]; discount: number; tax: number }): number {
  const subtotal = calculateSubtotal(order.items);
  const discountedTotal = applyDiscount(subtotal, order.discount);
  const finalTotal = applyTax(discountedTotal, order.tax);
  return finalTotal;
}
```

---

**[Section 8: Refactoring Practice](section8-refactoring-practice.md)** ➡️ 😊