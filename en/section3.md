We will move on to **Section 3: Code Smells**. These are signs that your code has issues and needs to be refactored. We will explore common code smells and how to fix them using **TypeScript**. ✨

---

### **Section 3: Code Smells** 🚨

#### **1. Long Method** 📏
A method that is too long is often hard to read and maintain. Break it into smaller methods.

Example:
```typescript
// ❌ Code smell: Long Method
function processOrder(order: { items: number[]; discount: number; tax: number }): number {
  let total = 0;
  for (let i = 0; i < order.items.length; i++) {
    total += order.items[i];
  }
  total = total - order.discount;
  total = total + (total * order.tax);
  return total;
}

// ✅ Refactor: Break into smaller methods
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

#### **2. Large Class** 🏢
A class that is too large often takes on too many responsibilities. Break it into smaller classes.

Example:
```typescript
// ❌ Code smell: Large Class
class UserManager {
  constructor(private users: { name: string; email: string }[]) {}

  addUser(user: { name: string; email: string }) {
    this.users.push(user);
  }

  removeUser(email: string) {
    this.users = this.users.filter(u => u.email !== email);
  }

  sendEmailToAllUsers(message: string) {
    this.users.forEach(user => {
      console.log(`Sending email to ${user.email}: ${message}`);
    });
  }

  generateReport() {
    return this.users.map(user => `${user.name} (${user.email})`).join('\n');
  }
}

// ✅ Refactor: Break into smaller classes
class UserRepository {
  constructor(private users: { name: string; email: string }[]) {}

  addUser(user: { name: string; email: string }) {
    this.users.push(user);
  }

  removeUser(email: string) {
    this.users = this.users.filter(u => u.email !== email);
  }

  getAllUsers() {
    return this.users;
  }
}

class EmailService {
  sendEmailToAllUsers(users: { email: string }[], message: string) {
    users.forEach(user => {
      console.log(`Sending email to ${user.email}: ${message}`);
    });
  }
}

class ReportGenerator {
  generateReport(users: { name: string; email: string }[]) {
    return users.map(user => `${user.name} (${user.email})`).join('\n');
  }
}
```

---

#### **3. Duplicated Code** 🔁
Duplicated code increases the chance of errors and makes maintenance harder. Extract it into a separate function or module.

Example:
```typescript
// ❌ Code smell: Duplicated Code
function calculateAreaOfSquare(side: number): number {
  return side * side;
}

function calculateAreaOfRectangle(length: number, width: number): number {
  return length * width;
}

// ✅ Refactor: Use a common function
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

#### **4. Feature Envy** 🤷‍♂️
A method that uses another class's data more than its own. Move the method to the class containing the data.

Example:
```typescript
// ❌ Code smell: Feature Envy
class Order {
  constructor(public items: number[], public discount: number) {}
}

class OrderProcessor {
  calculateTotal(order: Order): number {
    const subtotal = order.items.reduce((sum, item) => sum + item, 0);
    return subtotal - order.discount;
  }
}

// ✅ Refactor: Move the method to the Order class
class Order {
  constructor(public items: number[], public discount: number) {}

  calculateTotal(): number {
    const subtotal = this.items.reduce((sum, item) => sum + item, 0);
    return subtotal - this.discount;
  }
}
```

---

#### **5. Primitive Obsession** 🔢
Overusing primitive data types (like `string`, `number`) instead of creating appropriate classes or objects.

Example:
```typescript
// ❌ Code smell: Primitive Obsession
function createUser(name: string, email: string, age: number) {
  // Process logic
}

// ✅ Refactor: Use a class or interface
interface User {
  name: string;
  email: string;
  age: number;
}

function createUser(user: User) {
  // Process logic
}
```

---

### **Practice Exercise** 📝
Refactor the following TypeScript code to eliminate code smells:
```typescript
class Product {
  constructor(public name: string, public price: number, public quantity: number) {}

  calculateTotalPrice(): number {
    return this.price * this.quantity;
  }

  applyDiscount(discount: number): number {
    return this.price * this.quantity * (1 - discount);
  }

  generateReport(): string {
    return `Product: ${this.name}, Total Price: ${this.calculateTotalPrice()}`;
  }
}
```

💡 **Hints**:
- Extract `generateReport` into a separate class. ✂️
- Check for duplicated code. 🔍

---

**[Section 4: Basic Refactoring Techniques](section4.md)** ➡️