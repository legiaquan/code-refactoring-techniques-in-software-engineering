Chúng ta sẽ chuyển sang **Section 7: Công cụ Hỗ trợ Refactoring**. Trong phần này, chúng ta sẽ khám phá các công cụ và IDE hỗ trợ refactoring, giúp bạn thực hiện các thay đổi mã nguồn một cách nhanh chóng và an toàn. Đặc biệt, chúng ta sẽ tập trung vào cách sử dụng các công cụ này với **TypeScript**. ✨

---

### **Section 7: Công cụ Hỗ trợ Refactoring** 🛠️

#### **1. Công cụ Refactoring trong IDE** 💻
Các IDE hiện đại như **Visual Studio Code (VS Code)**, **WebStorm**, và **IntelliJ IDEA** cung cấp nhiều tính năng refactoring mạnh mẽ. Dưới đây là một số tính năng phổ biến:

##### **a. Rename Symbol (Đổi tên biến/hàm/lớp)** ✏️
- **Phím tắt**: `F2` (VS Code) hoặc `Shift + F6` (WebStorm/IntelliJ).
- **Ví dụ**: Đổi tên biến hoặc hàm và tự động cập nhật tất cả các tham chiếu.

```typescript
// ❌ Trước khi đổi tên
function calculateTotal(price: number, quantity: number): number {
  return price * quantity;
}

// ✅ Sau khi đổi tên (sử dụng Rename Symbol)
function computeTotal(price: number, quantity: number): number {
  return price * quantity;
}
```

##### **b. Extract Method (Tách phương thức)** ✂️
- **Phím tắt**: `Ctrl + .` (VS Code) hoặc `Ctrl + Alt + M` (WebStorm/IntelliJ).
- **Ví dụ**: Tách một đoạn mã thành một hàm mới.

```typescript
// ❌ Trước khi tách
function processOrder(order: { items: number[]; discount: number }): number {
  const subtotal = order.items.reduce((sum, item) => sum + item, 0);
  return subtotal - order.discount;
}

// ✅ Sau khi tách (sử dụng Extract Method)
function calculateSubtotal(items: number[]): number {
  return items.reduce((sum, item) => sum + item, 0);
}

function processOrder(order: { items: number[]; discount: number }): number {
  const subtotal = calculateSubtotal(order.items);
  return subtotal - order.discount;
}
```

##### **c. Extract Variable (Tách biến)** 🧩
- **Phím tắt**: `Ctrl + .` (VS Code) hoặc `Ctrl + Alt + V` (WebStorm/IntelliJ).
- **Ví dụ**: Tách một biểu thức phức tạp thành một biến riêng.

```typescript
// ❌ Trước khi tách
function calculateTotal(price: number, quantity: number, tax: number): number {
  return price * quantity + (price * quantity * tax);
}

// ✅ Sau khi tách (sử dụng Extract Variable)
function calculateTotal(price: number, quantity: number, tax: number): number {
  const subtotal = price * quantity;
  const taxAmount = subtotal * tax;
  return subtotal + taxAmount;
}
```

---

#### **2. Công cụ Phân tích Mã Tĩnh (Static Analysis Tools)** 🔍
Các công cụ phân tích mã tĩnh giúp phát hiện các vấn đề tiềm ẩn trong mã và đề xuất refactoring.

##### **a. ESLint** 🛡️
- **Công dụng**: Phát hiện lỗi cú pháp, code smells, và vi phạm coding conventions.
- **Cài đặt**:
  ```bash
  npm install eslint --save-dev
  npx eslint --init
  ```
- **Ví dụ cấu hình ESLint**:
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
- **Công dụng**: Phân tích chất lượng mã, phát hiện lỗi, code smells, và bảo mật.
- **Tích hợp với TypeScript**: Sử dụng plugin SonarTS.

---

#### **3. Công cụ Tự động Refactoring** 🤖
Một số công cụ tự động hóa quá trình refactoring, giúp bạn tiết kiệm thời gian.

##### **a. Prettier** 🎨
- **Công dụng**: Tự động định dạng mã để đảm bảo tính nhất quán.
- **Cài đặt**:
  ```bash
  npm install prettier --save-dev
  ```
- **Ví dụ cấu hình Prettier**:
  ```json
  {
    "semi": true,
    "singleQuote": true,
    "tabWidth": 2
  }
  ```

##### **b. TypeScript Refactoring Tools** 🛠️
- **Công dụng**: Các plugin và extension hỗ trợ refactoring TypeScript.
- **Ví dụ**: Extension **TypeScript Hero** cho VS Code giúp tự động import và sắp xếp các module.

---

#### **4. Công cụ Kiểm thử Tự động (Automated Testing Tools)** 🧪
Refactoring cần được đảm bảo bằng các bài kiểm thử tự động để tránh phá vỡ chức năng hiện có.

##### **a. Jest** 🧾
- **Công dụng**: Viết và chạy các bài kiểm thử đơn vị (unit tests).
- **Ví dụ**:
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
- **Công dụng**: Kiểm thử tự động giao diện người dùng (UI testing).
- **Ví dụ**:
  ```typescript
  describe('My First Test', () => {
    it('Visits the app', () => {
      cy.visit('/');
      cy.contains('Welcome');
    });
  });
  ```

---

### **Bài tập Thực hành** 📝
Hãy sử dụng các công cụ refactoring trong IDE của bạn để cải thiện đoạn mã sau:
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

**💡 Gợi ý**:
- Sử dụng **Extract Method** để tách logic tính toán.
- Sử dụng **Rename Symbol** để đặt tên biến rõ ràng hơn.

---

### **Kết quả Mong đợi**: 🎉
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

**[Bonus 1: Send emails to multiple user types](bonus1-send-emails-to-multiple-user-types.md)** ➡️ ✉️
