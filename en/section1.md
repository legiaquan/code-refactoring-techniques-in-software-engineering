**Section 1: Introduction to Refactoring** using **TypeScript** for illustration. TypeScript is an excellent language for refactoring because of its strong type system, which helps detect errors early and makes the code more readable. ✨

---

### **Section 1: Introduction to Refactoring** 🚀

#### **1. What is Refactoring?** 🤔
Refactoring is the process of improving the structure of source code **without changing its external behavior**. The goal is to make the code more readable, maintainable, and extensible.

Example in TypeScript:
```typescript
// ❌ Before refactoring
function calculateTotal(price: number, quantity: number): number {
  return price * quantity;
}

// ✅ After refactoring
function calculateTotal(price: number, quantity: number): number {
  const total = price * quantity;
  return total;
}
```
📌 Here, we added a `total` variable to make the code more readable without changing the logic.

---

#### **2. Why Refactor?** 💡
- **Improve code readability**: 👀 Readable code helps teammates and yourself understand it faster.
- **Ease of maintenance**: 🛠️ Well-organized code makes it easier to fix bugs or add features.
- **Reduce risks**: ⚠️ Refactoring helps identify and fix potential issues.
- **Optimize performance**: 🚀 Sometimes refactoring helps optimize performance.

Example:
```typescript
// ❌ Before refactoring
function getUserInfo(user: { name: string; age: number }): string {
  return user.name + " is " + user.age + " years old.";
}

// ✅ After refactoring
function getUserInfo(user: { name: string; age: number }): string {
  return `${user.name} is ${user.age} years old.`;
}
```
✅ Using template strings makes the code shorter and more readable.

---

#### **3. When to Refactor and When Not to Refactor?** ⏰
- **Refactor when**: ✔️
  - You need to add new features, and the current code is hard to understand.
  - You find duplicated code. 🔁
  - You want to improve performance or scalability. 📈

- **Do not refactor when**: ❌
  - You are under a tight deadline. ⏳
  - You do not have enough test coverage to ensure existing functionality is not broken. 🧪

Example:
```typescript
// ❌ Before refactoring
function isEligible(age: number): boolean {
  if (age >= 18) {
    return true;
  } else {
    return false;
  }
}

// ✅ After refactoring
function isEligible(age: number): boolean {
  return age >= 18;
}
```
✨ Refactoring makes the code shorter without changing the logic.

---

### **Practice Exercise** 📝
Refactor the following TypeScript code to make it more readable:
```typescript
function processOrder(order: { items: number[]; discount: number }): number {
  let total = 0;
  for (let i = 0; i < order.items.length; i++) {
    total += order.items[i];
  }
  return total - order.discount;
}
```

💡 **Hint**:
- Use `reduce` to calculate the total.
- Use clearer variable names.

---

**[Section 2: Refactoring Principles](section2.md)** ➡️