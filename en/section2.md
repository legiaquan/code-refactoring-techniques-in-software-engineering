We now move to **Section 2: Refactoring Principles**. In this section, we will explore the fundamental principles that help you write clean and easily refactorable code. All examples will be illustrated using **TypeScript**. ✨

---

### **Section 2: Refactoring Principles** 🚀

#### **1. DRY Principle (Don't Repeat Yourself)** 🔁
The DRY principle advises you to **avoid repeating code**. If you find yourself copying and pasting code, consider extracting it into a separate function or module.

Example:
```typescript
// ❌ Violating DRY
function calculateAreaOfSquare(side: number): number {
  return side * side;
}

function calculateAreaOfRectangle(length: number, width: number): number {
  return length * width;
}

// ✅ Following DRY
function calculateArea(shape: 'square' | 'rectangle', ...dimensions: number[]): number {
  if (shape === 'square') {
    return dimensions[0] * dimensions[0];
  } else if (shape === 'rectangle') {
    return dimensions[0] * dimensions[1];
  }
  throw new Error('Shape not supported');
}
```

---

#### **2. KISS Principle (Keep It Simple, Stupid)** 🤓
The KISS principle advises you to **keep your code simple and easy to understand**. Avoid overcomplicating things unnecessarily.

Example:
```typescript
// ❌ Overcomplicated
function isPrime(num: number): boolean {
  if (num <= 1) return false;
  for (let i = 2; i <= Math.sqrt(num); i++) {
    if (num % i === 0) return false;
  }
  return true;
}

// ✅ Simplified
function isPrime(num: number): boolean {
  if (num <= 1) return false;
  for (let i = 2; i < num; i++) {
    if (num % i === 0) return false;
  }
  return true;
}
```
Although the first version is more optimized, the second version is easier to understand and adheres to the KISS principle.

---

#### **3. YAGNI Principle (You Aren't Gonna Need It)** 🚫
The YAGNI principle advises you to **not add features or code that you don't need right now**. This helps avoid over-engineering.

Example:
```typescript
// ❌ Violating YAGNI
interface User {
  name: string;
  age: number;
  address?: string; // Adding unnecessary field
}

// ✅ Following YAGNI
interface User {
  name: string;
  age: number;
}
```
Only add `address` when you actually need it.

---

#### **4. Single Responsibility Principle (SRP)** 🛠️
Each function or class should have **only one responsibility**. This makes the code easier to understand and maintain.

Example:
```typescript
// ❌ Violating SRP
class User {
  constructor(public name: string, public email: string) {}

  saveToDatabase() {
    // Save user to database
  }

  sendEmail() {
    // Send email to user
  }
}

// ✅ Following SRP
class User {
  constructor(public name: string, public email: string) {}
}

class UserRepository {
  saveToDatabase(user: User) {
    // Save user to database
  }
}

class EmailService {
  sendEmail(user: User) {
    // Send email to user
  }
}
```

---

#### **5. Open/Closed Principle (OCP)** 🔓
Code should be **open for extension** but **closed for modification**. This allows you to add new features without changing existing code.

Example:
```typescript
// ❌ Violating OCP
class Discount {
  applyDiscount(price: number, type: string): number {
    if (type === 'fixed') {
      return price - 10;
    } else if (type === 'percentage') {
      return price * 0.9;
    }
    return price;
  }
}

// ✅ Following OCP
interface DiscountStrategy {
  apply(price: number): number;
}

class FixedDiscount implements DiscountStrategy {
  apply(price: number): number {
    return price - 10;
  }
}

class PercentageDiscount implements DiscountStrategy {
  apply(price: number): number {
    return price * 0.9;
  }
}

class Discount {
  constructor(private strategy: DiscountStrategy) {}

  applyDiscount(price: number): number {
    return this.strategy.apply(price);
  }
}
```

---

### **Practice Exercise** 📝
Apply the above principles to refactor the following TypeScript code:
```typescript
class Report {
  generateReport(data: any[], format: string): string {
    if (format === 'csv') {
      return data.map(row => row.join(',')).join('\n');
    } else if (format === 'json') {
      return JSON.stringify(data);
    } else {
      throw new Error('Unsupported format');
    }
  }

  saveReport(report: string, location: string): void {
    // Save report to location
  }
}
```

💡 **Hints**:
- Extract `generateReport` into separate classes for each format. ✂️
- Extract `saveReport` into a separate class. 🧩

---

**[Section 3: Code Smells](section3.md)** ➡️