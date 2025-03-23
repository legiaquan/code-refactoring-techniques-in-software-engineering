**Section 8: Thực hành Refactoring**. Trong phần này, chúng ta sẽ áp dụng tất cả các kiến thức đã học để refactor một đoạn mã thực tế ✨

---

### **Section 8: Thực hành Refactoring** 🛠️

#### **1. Bài toán Thực tế** 📋
Giả sử bạn có một đoạn mã TypeScript xử lý đơn hàng (order processing) như sau:
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

#### **2. Phân tích Mã Hiện tại** 🔍
- **Code Smells**:
  - **Long Method**: Hàm `process` quá dài và đảm nhận nhiều trách nhiệm.
  - **Primitive Obsession**: Sử dụng nhiều tham số nguyên thủy thay vì đối tượng.
  - **Tight Coupling**: Phụ thuộc trực tiếp vào `Database` và `EmailService`.
  - **Duplicate Code**: Logic validate có thể được tách riêng.

---

#### **3. Refactoring Step-by-Step** 🧩

##### **Bước 1: Tách Logic Validate** ✂️
Tách logic validate thành một hàm riêng để tăng khả năng tái sử dụng.

```typescript
function validateOrder(order: { items: number[]; discount: number; tax: number }): void {
  if (order.items.length === 0) throw new Error("Order must have at least one item.");
  if (order.discount < 0) throw new Error("Discount cannot be negative.");
  if (order.tax < 0) throw new Error("Tax cannot be negative.");
}
```

##### **Bước 2: Tách Logic Tính toán** ✏️
Tách logic tính toán thành một hàm riêng để dễ kiểm thử và tái sử dụng.

```typescript
function calculateTotal(order: { items: number[]; discount: number; tax: number }): number {
  const subtotal = order.items.reduce((sum, item) => sum + item, 0);
  const discountedTotal = subtotal - order.discount;
  return discountedTotal + (discountedTotal * order.tax);
}
```

##### **Bước 3: Giảm Coupling bằng Dependency Injection** 🔗
Thay vì phụ thuộc trực tiếp vào `Database` và `EmailService`, hãy sử dụng dependency injection.

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

##### **Bước 4: Sử dụng Đối tượng Thay vì Tham số Nguyên thủy** 🧾
Nhóm các tham số liên quan vào một đối tượng.

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

#### **4. Kết quả Sau khi Refactor** 🎉
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

#### **5. Lợi ích Sau khi Refactor** 🌟
- **Dễ đọc**: Mỗi hàm chỉ đảm nhận một trách nhiệm.
- **Dễ bảo trì**: Thay đổi logic validate hoặc tính toán không ảnh hưởng đến phần còn lại.
- **Dễ kiểm thử**: Có thể viết unit test riêng cho từng hàm.
- **Linh hoạt**: Có thể dễ dàng thay đổi database hoặc email service mà không cần sửa lớp `OrderProcessor`.

---

### **Bài tập Thực hành** 📝
Hãy refactor đoạn mã sau bằng cách áp dụng các kỹ thuật đã học:
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

💡 **Gợi ý**:
- Sử dụng **Strategy Pattern** để tách logic xử lý định dạng. 🧩
- Sử dụng **Dependency Injection** để giảm coupling. 🔗

---

### **Kết quả Mong đợi** 🎯
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