Chúng ta sẽ chuyển sang **Section 3: Các Mùi Code (Code Smells)**. Đây là những dấu hiệu cho thấy mã của bạn có vấn đề và cần được refactor. Chúng ta sẽ tìm hiểu các mùi code phổ biến và cách khắc phục chúng bằng **TypeScript**. 🛠️

---

### **Section 3: Các Mùi Code (Code Smells)** 🚨

#### **1. Long Method (Hàm quá dài)** 📏
Một hàm quá dài thường khó đọc và khó bảo trì. Hãy tách nó thành các hàm nhỏ hơn.

Ví dụ:
```typescript
// ❌ Mùi code: Long Method
function processOrder(order: { items: number[]; discount: number; tax: number }): number {
  let total = 0;
  for (let i = 0; i < order.items.length; i++) {
    total += order.items[i];
  }
  total = total - order.discount;
  total = total + (total * order.tax);
  return total;
}

// ✅ Refactor: Tách thành các hàm nhỏ
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

#### **2. Large Class (Lớp quá lớn)** 🏢
Một lớp quá lớn thường đảm nhận quá nhiều trách nhiệm. Hãy tách nó thành các lớp nhỏ hơn.

Ví dụ:
```typescript
// ❌ Mùi code: Large Class
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

// ✅ Refactor: Tách thành các lớp nhỏ
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

#### **3. Duplicated Code (Mã trùng lặp)** 🔁
Mã trùng lặp làm tăng khả năng lỗi và khó bảo trì. Hãy tách nó thành một hàm hoặc module riêng.

Ví dụ:
```typescript
// ❌ Mùi code: Duplicated Code
function calculateAreaOfSquare(side: number): number {
  return side * side;
}

function calculateAreaOfRectangle(length: number, width: number): number {
  return length * width;
}

// ✅ Refactor: Sử dụng hàm chung
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

#### **4. Feature Envy (Hàm quá quan tâm đến dữ liệu của lớp khác)** 🤷‍♂️
Một hàm sử dụng dữ liệu của một lớp khác nhiều hơn dữ liệu của chính nó. Hãy di chuyển hàm đó vào lớp chứa dữ liệu.

Ví dụ:
```typescript
// ❌ Mùi code: Feature Envy
class Order {
  constructor(public items: number[], public discount: number) {}
}

class OrderProcessor {
  calculateTotal(order: Order): number {
    const subtotal = order.items.reduce((sum, item) => sum + item, 0);
    return subtotal - order.discount;
  }
}

// ✅ Refactor: Di chuyển hàm vào lớp Order
class Order {
  constructor(public items: number[], public discount: number) {}

  calculateTotal(): number {
    const subtotal = this.items.reduce((sum, item) => sum + item, 0);
    return subtotal - this.discount;
  }
}
```

---

#### **5. Primitive Obsession (Lạm dụng kiểu dữ liệu nguyên thủy)** 🔢
Sử dụng quá nhiều kiểu dữ liệu nguyên thủy (như `string`, `number`) thay vì tạo các lớp hoặc đối tượng phù hợp.

Ví dụ:
```typescript
// ❌ Mùi code: Primitive Obsession
function createUser(name: string, email: string, age: number) {
  // Xử lý logic
}

// ✅ Refactor: Sử dụng lớp hoặc interface
interface User {
  name: string;
  email: string;
  age: number;
}

function createUser(user: User) {
  // Xử lý logic
}
```

---

### **Bài tập thực hành** 📝
Hãy refactor đoạn mã TypeScript sau để loại bỏ các mùi code:
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

💡 **Gợi ý**:
- Tách `generateReport` thành một lớp riêng.
- Kiểm tra xem có mã trùng lặp không.

---

**[Section 4: Basic Refactoring Techniques](section4-basic-refactoring-techniques.md)** ➡️