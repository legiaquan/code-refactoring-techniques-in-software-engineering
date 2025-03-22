Great! We will move on to **Section 5: Advanced Refactoring Techniques**. These techniques help address more complex issues in code, often related to architectural design and data modeling. All examples use **TypeScript**.

---

### **Section 5: Advanced Refactoring Techniques**

#### **1. Replace Conditional with Polymorphism**
Replace complex `if/else` or `switch` statements by using inheritance and polymorphism.

**Example**:
```typescript
// Before refactoring
class Animal {
  constructor(public type: string) {}

  makeSound(): string {
    if (this.type === "dog") return "Woof!";
    else if (this.type === "cat") return "Meow!";
    else return "Unknown sound";
  }
}

// After refactoring
abstract class Animal {
  abstract makeSound(): string;
}

class Dog extends Animal {
  makeSound(): string {
    return "Woof!";
  }
}

class Cat extends Animal {
  makeSound(): string {
    return "Meow!";
  }
}
```

**Benefits**:
- Easier to extend when adding new animal types.
- Reduces dependency on conditional logic.

---

#### **2. Introduce Parameter Object**
Group related parameters into an object to clean up the parameter list.

**Example**:
```typescript
// Before refactoring
function createUser(
  name: string,
  email: string,
  age: number,
  country: string
) {
  // Logic
}

// After refactoring
interface UserParams {
  name: string;
  email: string;
  age: number;
  country: string;
}

function createUser(params: UserParams) {
  // Logic
}
```

**Benefits**:
- Reduces the number of parameters passed to a function.
- Easier to add/remove properties without breaking the API.

---

#### **3. Decompose Conditional**
Break down complex conditions into meaningful functions.

**Example**:
```typescript
// Before refactoring
function calculateDiscount(user: { age: number; isVIP: boolean }) {
  if (user.age > 60 || user.isVIP) {
    return 0.2;
  }
  return 0;
}

// After refactoring
function isEligibleForDiscount(user: { age: number; isVIP: boolean }): boolean {
  return user.age > 60 || user.isVIP;
}

function calculateDiscount(user: { age: number; isVIP: boolean }) {
  return isEligibleForDiscount(user) ? 0.2 : 0;
}
```

**Benefits**:
- Improves readability and reusability.

---

#### **4. Replace Magic Number with Symbolic Constant**
Use constants to replace unclear numeric or string values.

**Example**:
```typescript
// Before refactoring
function calculateCircleArea(radius: number): number {
  return 3.14159 * radius * radius;
}

// After refactoring
const PI = 3.14159;

function calculateCircleArea(radius: number): number {
  return PI * radius * radius;
}
```

**Advanced**:
```typescript
enum DiscountRate {
  VIP = 0.3,
  SENIOR = 0.25,
  DEFAULT = 0.1,
}

function getDiscountRate(user: { isVIP: boolean; age: number }): number {
  if (user.isVIP) return DiscountRate.VIP;
  if (user.age > 60) return DiscountRate.SENIOR;
  return DiscountRate.DEFAULT;
}
```

---

#### **5. Replace Error Code with Exception**
Instead of returning error codes, use exceptions for clearer error handling.

**Example**:
```typescript
// Before refactoring
function divide(a: number, b: number): number | string {
  if (b === 0) return "Division by zero!";
  return a / b;
}

// After refactoring
class DivisionByZeroError extends Error {}

function divide(a: number, b: number): number {
  if (b === 0) throw new DivisionByZeroError("Division by zero!");
  return a / b;
}
```

**Benefits**:
- Separates main logic from error handling.
- Forces users to handle exceptions.

---

### **Practice Exercise**
Refactor the following TypeScript code using advanced techniques:
```typescript
class PaymentProcessor {
  processPayment(method: string, amount: number): string {
    if (method === "credit_card") {
      return `Processing credit card payment: $${amount}`;
    } else if (method === "paypal") {
      return `Processing PayPal payment: $${amount}`;
    } else if (method === "crypto") {
      return `Processing crypto payment: $${amount}`;
    } else {
      throw new Error("Unsupported payment method");
    }
  }
}
```

**Hints**:
- Use **Replace Conditional with Polymorphism**.
- Create subclasses for each payment method.

---

### **Expected Result After Refactoring**:
```typescript
abstract class PaymentMethod {
  abstract processPayment(amount: number): string;
}

class CreditCardPayment extends PaymentMethod {
  processPayment(amount: number): string {
    return `Processing credit card payment: $${amount}`;
  }
}

class PayPalPayment extends PaymentMethod {
  processPayment(amount: number): string {
    return `Processing PayPal payment: $${amount}`;
  }
}

class CryptoPayment extends PaymentMethod {
  processPayment(amount: number): string {
    return `Processing crypto payment: $${amount}`;
  }
}

class PaymentProcessor {
  processPayment(method: PaymentMethod, amount: number): string {
    return method.processPayment(amount);
  }
}
```

---

**[Section 6: Refactoring and Software Architecture](section6.md)**