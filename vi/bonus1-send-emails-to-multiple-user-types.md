Giả sử bạn có một hệ thống gửi email cho nhiều đối tượng (khách hàng, admin, đối tác) với nội dung đa dạng. Code ban đầu có thể trông như thế này: ✉️

---

### **1. Mã Gốc (Trước khi Refactor)** 🛠️
```typescript
class EmailService {
  // Gửi email cho khách hàng
  sendCustomerEmail(customerEmail: string, orderId: string): void {
    const subject = "Cảm ơn bạn đã đặt hàng!";
    const content = `Xin chào, đơn hàng #${orderId} của bạn đã được xác nhận.`;
    this.validateEmail(customerEmail);
    this.send(customerEmail, subject, content);
    this.logEmail("customer", customerEmail);
  }

  // Gửi email cho admin
  sendAdminEmail(adminEmail: string, orderId: string): void {
    const subject = "Thông báo đơn hàng mới";
    const content = `Admin ơi, có đơn hàng #${orderId} cần xử lý.`;
    this.validateEmail(adminEmail);
    this.send(adminEmail, subject, content);
    this.logEmail("admin", adminEmail);
  }

  // Gửi email cho đối tác
  sendPartnerEmail(partnerEmail: string, orderId: string): void {
    const subject = "Hợp tác vận chuyển";
    const content = `Kính gửi đối tác, đơn hàng #${orderId} đã sẵn sàng.`;
    this.validateEmail(partnerEmail);
    this.send(partnerEmail, subject, content);
    this.logEmail("partner", partnerEmail);
  }

  private validateEmail(email: string): void {
    if (!email.includes("@")) throw new Error("Invalid email!");
  }

  private send(to: string, subject: string, content: string): void {
    console.log(`Gửi email đến ${to}: [${subject}] ${content}`);
  }

  private logEmail(type: string, email: string): void {
    console.log(`Đã gửi email ${type} đến ${email}`);
  }
}
```

---

### **2. Phân tích Mã Gốc** 🔍
- **Code Smells**:
  - **Duplicated Code**: Các phương thức `sendCustomerEmail`, `sendAdminEmail`, `sendPartnerEmail` có cấu trúc giống nhau, chỉ khác phần `subject` và `content`.
  - **Primitive Obsession**: Sử dụng tham số `type` (ví dụ: "customer") dạng chuỗi, dễ gây lỗi.
  - **Khó mở rộng**: Thêm loại email mới đòi hỏi sao chép code.

---

### **3. Refactoring với Strategy Pattern và Composition** 🧩
Mục tiêu: Tách biệt logic tạo nội dung email và quy trình gửi email.

#### **Bước 1: Định nghĩa Template Email (Strategy Interface)** 📝
```typescript
interface EmailTemplate {
  getSubject(orderId: string): string;
  getContent(orderId: string): string;
  getType(): string;
}
```

#### **Bước 2: Triển khai Template cho Từng Đối tượng** 🛠️
```typescript
// Composition
class CustomerEmailTemplate implements EmailTemplate {
  getSubject(orderId: string): string {
    return "Cảm ơn bạn đã đặt hàng!";
  }
  getContent(orderId: string): string {
    return `Xin chào, đơn hàng #${orderId} của bạn đã được xác nhận.`;
  }
  getType(): string {
    return "customer";
  }
}

class AdminEmailTemplate implements EmailTemplate {
  getSubject(orderId: string): string {
    return "Thông báo đơn hàng mới";
  }
  getContent(orderId: string): string {
    return `Admin ơi, có đơn hàng #${orderId} cần xử lý.`;
  }
  getType(): string {
    return "admin";
  }
}

class PartnerEmailTemplate implements EmailTemplate {
  getSubject(orderId: string): string {
    return "Hợp tác vận chuyển";
  }
  getContent(orderId: string): string {
    return `Kính gửi đối tác, đơn hàng #${orderId} đã sẵn sàng.`;
  }
  getType(): string {
    return "partner";
  }
}
```

#### **Bước 3: Tái cấu trúc EmailService** ✂️
```typescript
class EmailService {
  constructor(
    // Composition email templates
    private emailTemplates: Record<string, EmailTemplate> = {
      customer: new CustomerEmailTemplate(),
      admin: new AdminEmailTemplate(),
      partner: new PartnerEmailTemplate(),
    }
  ) {}

  // Phương thức chung để gửi email
  sendEmail(
    type: string,
    email: string,
    orderId: string
  ): void {
    const template = this.emailTemplates[type];
    if (!template) throw new Error("Invalid email type!");

    this.validateEmail(email);
    this.send(
      email,
      template.getSubject(orderId),
      template.getContent(orderId)
    );
    this.logEmail(template.getType(), email);
  }

  private validateEmail(email: string): void { /* ... */ }
  private send(to: string, subject: string, content: string): void { /* ... */ }
  private logEmail(type: string, email: string): void { /* ... */ }
}
```

---

### **4. Sử dụng Sau khi Refactor** 🚀
```typescript
const emailService = new EmailService();

// Gửi email cho khách hàng
emailService.sendEmail("customer", "user@example.com", "ORDER_123");

// Gửi email cho admin
emailService.sendEmail("admin", "admin@company.com", "ORDER_123");

// Gửi email cho đối tác
emailService.sendEmail("partner", "partner@company.com", "ORDER_123");
```

---

### **5. Lợi ích Sau khi Refactor** 🌟
- **Giảm Duplicated Code**: Chỉ còn một phương thức `sendEmail` thay vì nhiều phương thức riêng lẻ.
- **Dễ mở rộng**: Thêm loại email mới chỉ cần tạo một lớp triển khai `EmailTemplate`.
- **Tuân thủ Open/Closed Principle**: Mở rộng mà không cần sửa code có sẵn.
- **Tách biệt trách nhiệm**: Logic tạo nội dung email được tách biệt khỏi quy trình gửi.

---

### **Bài tập Thực hành cho Bạn** 📝
Hãy refactor đoạn mã sau để hỗ trợ **email thông báo khuyến mãi** và **email xác nhận tài khoản**:
```typescript
class PromotionalEmailService {
  sendPromotionEmail(userEmail: string, discountCode: string): void {
    const subject = "Khuyến mãi đặc biệt!";
    const content = `Bạn nhận được mã giảm giá: ${discountCode}`;
    this.validateEmail(userEmail);
    this.send(userEmail, subject, content);
    this.logEmail("promotion", userEmail);
  }

  sendVerificationEmail(userEmail: string, token: string): void {
    const subject = "Xác nhận tài khoản";
    const content = `Vui lòng nhập mã xác nhận: ${token}`;
    this.validateEmail(userEmail);
    this.send(userEmail, subject, content);
    this.logEmail("verification", userEmail);
  }

  private validateEmail(email: string): void {
    if (!email.includes("@")) throw new Error("Invalid email!");
  }

  private send(to: string, subject: string, content: string): void {
    console.log(`Gửi email đến ${to}: [${subject}] ${content}`);
  }

  private logEmail(type: string, email: string): void {
    console.log(`Đã gửi email ${type} đến ${email}`);
  }
}
```

**💡 Gợi ý**:
- Áp dụng **Strategy Pattern** tương tự ví dụ trên.
- Tạo các lớp `PromotionEmailTemplate` và `VerificationEmailTemplate`.

---

### **Kết quả Mong đợi**: 🎯
```typescript
interface EmailTemplate {
  getSubject(data: string): string;
  getContent(data: string): string;
  getType(): string;
}

class PromotionEmailTemplate implements EmailTemplate {
  getSubject(discountCode: string): string {
    return "Khuyến mãi đặc biệt!";
  }
  getContent(discountCode: string): string {
    return `Bạn nhận được mã giảm giá: ${discountCode}`;
  }
  getType(): string {
    return "promotion";
  }
}

class VerificationEmailTemplate implements EmailTemplate {
  getSubject(token: string): string {
    return "Xác nhận tài khoản";
  }
  getContent(token: string): string {
    return `Vui lòng nhập mã xác nhận: ${token}`;
  }
  getType(): string {
    return "verification";
  }
}

class EmailService {
  constructor(
    private templates: Record<string, EmailTemplate> = {
      promotion: new PromotionEmailTemplate(),
      verification: new VerificationEmailTemplate(),
    }
  ) {}

  sendEmail(type: string, email: string, data: string): void {
    const template = this.templates[type];
    // ... logic gửi email
  }
}
```

---

**[Bonus 2: Send multilingual emails](bonus2-send-multilingual-emails.md)** ➡️