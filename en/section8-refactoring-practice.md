We will move on to **Section 8: Refactoring Practice**. In this section, we will apply all the knowledge learned to refactor a real-world code example. We will use **TypeScript** and apply refactoring techniques from basic to advanced. ✨

---

### **Section 8: Refactoring Practice** 🛠️

#### **1. Real-World Problem** 📋
Suppose you have a TypeScript code snippet for order processing as follows:
```typescript
class OrderProcessor {
  process(order: { items: number[]; discount: number; tax: number; userEmail: string }): void {
    // Validate order
    if (order.items.length === 0) throw new Error("Order must have at least one item.");
    if (order.discount < 0) throw new Error("Discount cannot be negative.");
    if (order.tax < 0) throw new Error("Tax cannot be negative.");

    // Calculate total
    let subtotal = 0;
    for (let i = 0; i < order.items.length; i++) {
      subtotal += order.items[i];
    }
    const discountedTotal = subtotal - order.discount;
    const total = discountedTotal + (discountedTotal * order.tax);

    // Save order to database
    const db = new Database();
    db.save({ ...order, total });

    // Send confirmation email
    const emailService = new EmailService();
    emailService.sendConfirmation(order.userEmail, total);
  }
}
```

---

#### **2. Current Code Analysis** 🔍
- **Code Smells**:
  - **Long Method**: The `process` method is too long and handles multiple responsibilities.
  - **Primitive Obsession**: Uses many primitive parameters instead of objects.
  - **Tight Coupling**: Directly depends on `Database` and `EmailService`.
  - **Duplicate Code**: Validation logic can be extracted.

---

#### **3. Refactoring Step-by-Step** 🧩

##### **Step 1: Extract Validation Logic** ✂️
Extract validation logic into a separate function to increase reusability.

```typescript
function validateOrder(order: { items: number[]; discount: number; tax: number }): void {
  if (order.items.length === 0) throw new Error("Order must have at least one item.");
  if (order.discount < 0) throw new Error("Discount cannot be negative.");
  if (order.tax < 0) throw new Error("Tax cannot be negative.");
}
```

##### **Step 2: Extract Calculation Logic** ✏️
Extract calculation logic into a separate function for easier testing and reusability.

```typescript
function calculateTotal(order: { items: number[]; discount: number; tax: number }): number {
  const subtotal = order.items.reduce((sum, item) => sum + item, 0);
  const discountedTotal = subtotal - order.discount;
  return discountedTotal + (discountedTotal * order.tax);
}
```

##### **Step 3: Reduce Coupling with Dependency Injection** 🔗
Instead of directly depending on `Database` and `EmailService`, use dependency injection.

```typescript
interface Database {
  save(order: any): void;
}

interface EmailService {
  sendConfirmation(email: string, total: number): void;
}

class OrderProcessor {
  constructor(private db: Database, private emailService: EmailService) {}

  process(order: { items: number[]; discount: number; tax: number; userEmail: string }): void {
    validateOrder(order);
    const total = calculateTotal(order);

    this.db.save({ ...order, total });
    this.emailService.sendConfirmation(order.userEmail, total);
  }
}
```

##### **Step 4: Use Objects Instead of Primitive Parameters** 🧾
Group related parameters into an object.

```typescript
interface Order {
  items: number[];
  discount: number;
  tax: number;
  userEmail: string;
}

class OrderProcessor {
  constructor(private db: Database, private emailService: EmailService) {}

  process(order: Order): void {
    validateOrder(order);
    const total = calculateTotal(order);

    this.db.save({ ...order, total });
    this.emailService.sendConfirmation(order.userEmail, total);
  }
}
```

---

#### **4. Final Refactored Code** 🎉
```typescript
interface Order {
  items: number[];
  discount: number;
  tax: number;
  userEmail: string;
}

interface Database {
  save(order: any): void;
}

interface EmailService {
  sendConfirmation(email: string, total: number): void;
}

function validateOrder(order: Order): void {
  if (order.items.length === 0) throw new Error("Order must have at least one item.");
  if (order.discount < 0) throw new Error("Discount cannot be negative.");
  if (order.tax < 0) throw new Error("Tax cannot be negative.");
}

function calculateTotal(order: Order): number {
  const subtotal = order.items.reduce((sum, item) => sum + item, 0);
  const discountedTotal = subtotal - order.discount;
  return discountedTotal + (discountedTotal * order.tax);
}

class OrderProcessor {
  constructor(private db: Database, private emailService: EmailService) {}

  process(order: Order): void {
    validateOrder(order);
    const total = calculateTotal(order);

    this.db.save({ ...order, total });
    this.emailService.sendConfirmation(order.userEmail, total);
  }
}
```

---

#### **5. Benefits After Refactoring** 🌟
- **Readability**: Each function handles a single responsibility.
- **Maintainability**: Changes to validation or calculation logic do not affect other parts.
- **Testability**: Unit tests can be written for each function separately.
- **Flexibility**: Easily swap out the database or email service without modifying the `OrderProcessor` class.

---

### **Practice Exercise** 📝
Refactor the following code by applying the techniques learned:
```typescript
class ReportGenerator {
  generate(data: any[], format: string): string {
    if (format === "csv") {
      return data.map(row => row.join(",")).join("\n");
    } else if (format === "json") {
      return JSON.stringify(data);
    } else {
      throw new Error("Unsupported format");
    }
  }
}
```

💡 **Hints**:
- Use the **Strategy Pattern** to separate formatting logic. 🧩
- Use **Dependency Injection** to reduce coupling. 🔗

---

### **Expected Result** 🎯
```typescript
interface FormatStrategy {
  generate(data: any[]): string;
}

class CsvStrategy implements FormatStrategy {
  generate(data: any[]): string {
    return data.map(row => row.join(",")).join("\n");
  }
}

class JsonStrategy implements FormatStrategy {
  generate(data: any[]): string {
    return JSON.stringify(data);
  }
}

class ReportGenerator {
  constructor(private strategy: FormatStrategy) {}

  generate(data: any[]): string {
    return this.strategy.generate(data);
  }
}
```

---

**[Bonus 1: Send emails to multiple user types](bonus1-send-emails-to-multiple-user-types.md)** ➡️ ✉️