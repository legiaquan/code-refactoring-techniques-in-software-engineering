(Due to technical issues, the search service is temporarily unavailable.)

Tuyệt vời! Chúng ta sẽ chuyển sang **Section 5: Các Kỹ thuật Refactoring Nâng cao**. Những kỹ thuật này giúp giải quyết các vấn đề phức tạp hơn trong mã, thường liên quan đến thiết kế kiến trúc và mô hình hóa dữ liệu. Tất cả ví dụ đều sử dụng **TypeScript**. ✨

---

### **Section 5: Các Kỹ thuật Refactoring Nâng cao** 🚀

#### **1. Replace Conditional with Polymorphism (Thay thế Điều kiện bằng Đa hình)** 🐾
Thay thế các câu lệnh `if/else` hoặc `switch` phức tạp bằng cách sử dụng kế thừa và đa hình.

**Ví dụ**:
```typescript
// ❌ Trước khi refactor
class Animal {
  constructor(public type: string) {}

  makeSound(): string {
    if (this.type === "dog") return "Woof!";
    else if (this.type === "cat") return "Meow!";
    else return "Unknown sound";
  }
}

// ✅ Sau khi refactor
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

**🎯 Lợi ích**:
- Dễ mở rộng khi thêm loại động vật mới.
- Giảm sự phụ thuộc vào logic điều kiện.

---

#### **2. Introduce Parameter Object (Giới thiệu Đối tượng Tham số)** 🧩
Nhóm các tham số liên quan vào một đối tượng để làm sạch danh sách tham số.

**Ví dụ**:
```typescript
// ❌ Trước khi refactor
function createUser(
  name: string,
  email: string,
  age: number,
  country: string
) {
  // Logic
}

// ✅ Sau khi refactor
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

**🎯 Lợi ích**:
- Giảm số lượng tham số truyền vào hàm.
- Dễ dàng thêm/xóa thuộc tính mà không phá vỡ API.

---

#### **3. Decompose Conditional (Phân rã Điều kiện)** 🔄
Tách các điều kiện phức tạp thành các hàm có tên ý nghĩa.

**Ví dụ**:
```typescript
// ❌ Trước khi refactor
function calculateDiscount(user: { age: number; isVIP: boolean }) {
  if (user.age > 60 || user.isVIP) {
    return 0.2;
  }
  return 0;
}

// ✅ Sau khi refactor
function isEligibleForDiscount(user: { age: number; isVIP: boolean }): boolean {
  return user.age > 60 || user.isVIP;
}

function calculateDiscount(user: { age: number; isVIP: boolean }) {
  return isEligibleForDiscount(user) ? 0.2 : 0;
}
```

**🎯 Lợi ích**:
- Tăng khả năng đọc và tái sử dụng.

---

#### **4. Replace Magic Number with Symbolic Constant (Thay số "ma thuật" bằng Hằng số)** 🔢
Sử dụng hằng số để thay thế các giá trị số hoặc chuỗi khó hiểu.

**Ví dụ**:
```typescript
// ❌ Trước khi refactor
function calculateCircleArea(radius: number): number {
  return 3.14159 * radius * radius;
}

// ✅ Sau khi refactor
const PI = 3.14159;

function calculateCircleArea(radius: number): number {
  return PI * radius * radius;
}
```

**Nâng cao**:
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

#### **5. Replace Error Code with Exception (Thay mã lỗi bằng Exception)** ⚠️
Thay vì trả về mã lỗi, hãy sử dụng exception để xử lý lỗi rõ ràng hơn.

**Ví dụ**:
```typescript
// ❌ Trước khi refactor
function divide(a: number, b: number): number | string {
  if (b === 0) return "Division by zero!";
  return a / b;
}

// ✅ Sau khi refactor
class DivisionByZeroError extends Error {}

function divide(a: number, b: number): number {
  if (b === 0) throw new DivisionByZeroError("Division by zero!");
  return a / b;
}
```

**🎯 Lợi ích**:
- Tách biệt logic chính và xử lý lỗi.
- Buộc người dùng phải xử lý exception.

---

### **Bài tập thực hành** 📝
Hãy refactor đoạn mã TypeScript sau bằng các kỹ thuật nâng cao:
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

💡 **Gợi ý**:
- Sử dụng **Replace Conditional with Polymorphism**.
- Tạo các lớp con cho từng phương thức thanh toán.

---

### **Kết quả mong đợi sau refactor**: 🎉
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

**[Section 6: Refactoring and Software Architecture](section6.md)** ➡️