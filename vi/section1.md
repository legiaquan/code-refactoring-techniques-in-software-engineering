**Section 1: Giới thiệu về Refactoring** và sử dụng **TypeScript** để minh họa. TypeScript là một ngôn ngữ tuyệt vời để refactoring vì nó có hệ thống kiểu mạnh, giúp phát hiện lỗi sớm và làm cho mã dễ đọc hơn. ✨

---

### **Section 1: Giới thiệu về Refactoring** 🚀

#### **1. Refactoring là gì?** 🤔
Refactoring là quá trình cải thiện cấu trúc mã nguồn mà **không thay đổi hành vi bên ngoài** của nó. Mục tiêu là làm cho mã dễ đọc, dễ bảo trì và mở rộng hơn.

Ví dụ trong TypeScript:
```typescript
// Trước khi refactor
function calculateTotal(price: number, quantity: number): number {
  return price * quantity;
}

// Sau khi refactor
function calculateTotal(price: number, quantity: number): number {
  const total = price * quantity;
  return total;
}
```
📌 Ở đây, chúng ta thêm một biến `total` để làm cho mã dễ đọc hơn mà không thay đổi logic.

---

#### **2. Tại sao cần Refactoring?** 💡
- **Cải thiện khả năng đọc mã**: Mã dễ đọc giúp đồng đội và chính bạn hiểu nhanh hơn. 👀
- **Dễ bảo trì**: Khi mã được tổ chức tốt, việc sửa lỗi hoặc thêm tính năng sẽ dễ dàng hơn. 🛠️
- **Giảm rủi ro**: Refactoring giúp phát hiện và sửa lỗi tiềm ẩn. ⚠️
- **Tối ưu hiệu suất**: Đôi khi refactoring giúp tối ưu hóa hiệu suất. 🚀

Ví dụ:
```typescript
// Trước khi refactor
function getUserInfo(user: { name: string; age: number }): string {
  return user.name + " is " + user.age + " years old.";
}

// Sau khi refactor
function getUserInfo(user: { name: string; age: number }): string {
  return `${user.name} is ${user.age} years old.`;
}
```
✅ Sử dụng template string giúp mã ngắn gọn và dễ đọc hơn.

---

#### **3. Khi nào nên và không nên Refactoring?** ⏰
- **Nên refactor khi**: ✔️
  - Bạn cần thêm tính năng mới và mã hiện tại khó hiểu.
  - Bạn phát hiện mã trùng lặp (duplicated code). 🔁
  - Bạn muốn cải thiện hiệu suất hoặc khả năng mở rộng. 📈

- **Không nên refactor khi**: ❌
  - Bạn đang trong giai đoạn deadline căng thẳng. ⏳
  - Bạn không có đủ test coverage để đảm bảo không phá vỡ chức năng hiện có. 🧪

Ví dụ:
```typescript
// Trước khi refactor
function isEligible(age: number): boolean {
  if (age >= 18) {
    return true;
  } else {
    return false;
  }
}

// Sau khi refactor
function isEligible(age: number): boolean {
  return age >= 18;
}
```
✨ Refactoring giúp mã ngắn gọn hơn mà không thay đổi logic.

---

### **Bài tập thực hành** 📝
Hãy refactor đoạn mã TypeScript sau để làm cho nó dễ đọc hơn:
```typescript
function processOrder(order: { items: number[]; discount: number }): number {
  let total = 0;
  for (let i = 0; i < order.items.length; i++) {
    total += order.items[i];
  }
  return total - order.discount;
}
```

💡 **Gợi ý**:
- Sử dụng `reduce` để tính tổng.
- Đặt tên biến rõ ràng hơn.

---

**[Section 2: Refactoring Principles](section2.md)** ➡️