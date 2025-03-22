Chúng ta sẽ chuyển sang **Section 2: Các Nguyên tắc Refactoring**. Trong phần này, chúng ta sẽ tìm hiểu các nguyên tắc cơ bản giúp bạn viết mã sạch và dễ refactor hơn. Tất cả các ví dụ sẽ được minh họa bằng **TypeScript**. ✨

---

### **Section 2: Các Nguyên tắc Refactoring** 🚀

#### **1. Nguyên tắc DRY (Don't Repeat Yourself)** 🔁
Nguyên tắc DRY khuyên bạn **không lặp lại mã**. Nếu bạn thấy mình đang sao chép và dán mã, hãy xem xét việc tách nó thành một hàm hoặc module riêng.

Ví dụ:
```typescript
// ❌ Vi phạm DRY
function calculateAreaOfSquare(side: number): number {
  return side * side;
}

function calculateAreaOfRectangle(length: number, width: number): number {
  return length * width;
}

// ✅ Tuân thủ DRY
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

#### **2. Nguyên tắc KISS (Keep It Simple, Stupid)** 🤓
Nguyên tắc KISS khuyên bạn **giữ mã đơn giản và dễ hiểu**. Đừng làm phức tạp hóa vấn đề khi không cần thiết.

Ví dụ:
```typescript
// ❌ Phức tạp hóa
function isPrime(num: number): boolean {
  if (num <= 1) return false;
  for (let i = 2; i <= Math.sqrt(num); i++) {
    if (num % i === 0) return false;
  }
  return true;
}

// ✅ Đơn giản hóa
function isPrime(num: number): boolean {
  if (num <= 1) return false;
  for (let i = 2; i < num; i++) {
    if (num % i === 0) return false;
  }
  return true;
}
```
💡 Mặc dù phiên bản đầu tiên tối ưu hơn, nhưng phiên bản thứ hai dễ hiểu hơn và phù hợp với nguyên tắc KISS.

---

#### **3. Nguyên tắc YAGNI (You Aren't Gonna Need It)** 🚫
Nguyên tắc YAGNI khuyên bạn **không thêm tính năng hoặc mã mà bạn không cần ngay bây giờ**. Điều này giúp tránh over-engineering.

Ví dụ:
```typescript
// ❌ Vi phạm YAGNI
interface User {
  name: string;
  age: number;
  address?: string; // Thêm trường không cần thiết
}

// ✅ Tuân thủ YAGNI
interface User {
  name: string;
  age: number;
}
```
📌 Chỉ thêm `address` khi bạn thực sự cần nó.

---

#### **4. Nguyên tắc Single Responsibility (SRP)** 🛠️
Mỗi hàm hoặc lớp chỉ nên có **một trách nhiệm duy nhất**. Điều này giúp mã dễ hiểu và dễ bảo trì hơn.

Ví dụ:
```typescript
// ❌ Vi phạm SRP
class User {
  constructor(public name: string, public email: string) {}

  saveToDatabase() {
    // Lưu user vào database
  }

  sendEmail() {
    // Gửi email cho user
  }
}

// ✅ Tuân thủ SRP
class User {
  constructor(public name: string, public email: string) {}
}

class UserRepository {
  saveToDatabase(user: User) {
    // Lưu user vào database
  }
}

class EmailService {
  sendEmail(user: User) {
    // Gửi email cho user
  }
}
```

---

#### **5. Nguyên tắc Open/Closed (OCP)** 🔓
Mã nên **mở để mở rộng** nhưng **đóng để sửa đổi**. Điều này giúp bạn thêm tính năng mới mà không cần thay đổi mã hiện có.

Ví dụ:
```typescript
// ❌ Vi phạm OCP
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

// ✅ Tuân thủ OCP
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

### **Bài tập thực hành** 📝
Hãy áp dụng các nguyên tắc trên để refactor đoạn mã TypeScript sau:
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
    // Lưu report vào location
  }
}
```

💡 **Gợi ý**:
- Tách `generateReport` thành các lớp riêng biệt cho từng định dạng.
- Tách `saveReport` thành một lớp riêng.

---

**[Section 3: Code Smells](section3.md)** ➡️