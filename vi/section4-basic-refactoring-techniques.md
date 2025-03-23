Chúng ta sẽ chuyển sang **Section 4: Các Kỹ thuật Refactoring Cơ bản**. Trong phần này, chúng ta sẽ tìm hiểu các kỹ thuật refactoring đơn giản nhưng mạnh mẽ, giúp cải thiện mã nguồn một cách hiệu quả. Tất cả các ví dụ sẽ được minh họa bằng **TypeScript**. ✨

---

### **Section 4: Các Kỹ thuật Refactoring Cơ bản** 🚀

#### **1. Extract Method (Tách phương thức)** ✂️
Khi một hàm quá dài hoặc có một phần logic có thể tách riêng, hãy tách nó thành một hàm mới.

Ví dụ:
```typescript
// ❌ Trước khi refactor
function printOrderSummary(order: { items: number[]; discount: number }): void {
  let total = 0;
  for (let i = 0; i < order.items.length; i++) {
    total += order.items[i];
  }
  total = total - order.discount;
  console.log(`Total: ${total}`);
}

// ✅ Sau khi refactor
function calculateTotal(items: number[], discount: number): number {
  const subtotal = items.reduce((sum, item) => sum + item, 0);
  return subtotal - discount;
}

function printOrderSummary(order: { items: number[]; discount: number }): void {
  const total = calculateTotal(order.items, order.discount);
  console.log(`Total: ${total}`);
}
```

---

#### **2. Inline Method (Nhúng phương thức)** 🔄
Nếu một hàm quá đơn giản và chỉ được gọi một lần, hãy nhúng nó vào nơi gọi để giảm độ phức tạp.

Ví dụ:
```typescript
// ❌ Trước khi refactor
function getRating(driver: { numberOfLateDeliveries: number }): number {
  return moreThanFiveLateDeliveries(driver) ? 2 : 1;
}

function moreThanFiveLateDeliveries(driver: { numberOfLateDeliveries: number }): boolean {
  return driver.numberOfLateDeliveries > 5;
}

// ✅ Sau khi refactor
function getRating(driver: { numberOfLateDeliveries: number }): number {
  return driver.numberOfLateDeliveries > 5 ? 2 : 1;
}
```

---

#### **3. Extract Variable (Tách biến)** 🧩
Nếu một biểu thức phức tạp, hãy tách nó thành một biến riêng để làm cho mã dễ đọc hơn.

Ví dụ:
```typescript
// ❌ Trước khi refactor
function calculateTotal(order: { items: number[]; discount: number; tax: number }): number {
  return order.items.reduce((sum, item) => sum + item, 0) - order.discount + (order.items.reduce((sum, item) => sum + item, 0) * order.tax);
}

// ✅ Sau khi refactor
function calculateTotal(order: { items: number[]; discount: number; tax: number }): number {
  const subtotal = order.items.reduce((sum, item) => sum + item, 0);
  const discountedTotal = subtotal - order.discount;
  const taxAmount = subtotal * order.tax;
  return discountedTotal + taxAmount;
}
```

---

#### **4. Rename Method/Variable (Đổi tên phương thức/biến)** ✏️
Đặt tên rõ ràng và có ý nghĩa giúp mã dễ hiểu hơn.

Ví dụ:
```typescript
// ❌ Trước khi refactor
function calc(a: number, b: number): number {
  return a + b;
}

// ✅ Sau khi refactor
function addNumbers(num1: number, num2: number): number {
  return num1 + num2;
}
```

---

#### **5. Split Temporary Variable (Tách biến tạm thời)** 🔀
Nếu một biến tạm thời được sử dụng cho nhiều mục đích khác nhau, hãy tách nó thành các biến riêng.

Ví dụ:
```typescript
// ❌ Trước khi refactor
function calculateTotal(order: { items: number[]; discount: number }): number {
  let temp = order.items.reduce((sum, item) => sum + item, 0);
  temp = temp - order.discount;
  return temp;
}

// ✅ Sau khi refactor
function calculateTotal(order: { items: number[]; discount: number }): number {
  const subtotal = order.items.reduce((sum, item) => sum + item, 0);
  const total = subtotal - order.discount;
  return total;
}
```

---

#### **6. Replace Magic Number with Symbolic Constant (Thay số ma thuật bằng hằng số)** 🔢
Sử dụng hằng số thay vì số ma thuật để làm cho mã dễ hiểu hơn.

Ví dụ:
```typescript
// ❌ Trước khi refactor
function calculateArea(radius: number): number {
  return 3.14 * radius * radius;
}

// ✅ Sau khi refactor
const PI = 3.14;

function calculateArea(radius: number): number {
  return PI * radius * radius;
}
```

---

### **Bài tập thực hành** 📝
Hãy refactor đoạn mã TypeScript sau bằng các kỹ thuật cơ bản:
```typescript
function processOrder(order: { items: number[]; discount: number; tax: number }): number {
  let temp = order.items.reduce((sum, item) => sum + item, 0);
  temp = temp - order.discount;
  temp = temp + (temp * order.tax);
  return temp;
}
```

💡 **Gợi ý**:
- Sử dụng **Extract Variable** để tách biến `subtotal`, `discountedTotal`, và `taxAmount`.
- Đổi tên biến `temp` thành tên có ý nghĩa hơn.

---

**[Section 5: Advanced Refactoring Techniques](section5-advanced-refactoring-techniques.md)** ➡️ 😊