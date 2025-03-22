Để hỗ trợ **email đa ngôn ngữ**, chúng ta có thể mở rộng hệ thống bằng cách sử dụng **Strategy Pattern** kết hợp với **Factory Pattern** để tạo ra các template email phù hợp với ngôn ngữ của người dùng. Dưới đây là một ví dụ chi tiết: ✉️🌐

---

### **1. Yêu cầu** 📋
- Hỗ trợ gửi email bằng nhiều ngôn ngữ (ví dụ: tiếng Anh, tiếng Việt).
- Mỗi loại email (khách hàng, admin, đối tác) có thể được gửi bằng nhiều ngôn ngữ khác nhau.

---

### **2. Thiết kế cấu trúc** 🛠️
- **EmailTemplate**: Interface chung cho các template email.
- **LanguageStrategy**: Interface để xác định ngôn ngữ.
- **EmailService**: Sử dụng `EmailTemplate` và `LanguageStrategy` để gửi email.

---

### **3. Triển khai Code** 💻

#### **Bước 1: Định nghĩa Interface và Strategy** 🧩
```typescript
// Interface cho Email Template
interface EmailTemplate {
  getSubject(orderId: string): string;
  getContent(orderId: string): string;
  getType(): string;
}

// Interface cho Language Strategy
interface LanguageStrategy {
  getLanguage(): string;
}
```

#### **Bước 2: Triển khai Language Strategy** 🌍
```typescript
class EnglishLanguageStrategy implements LanguageStrategy {
  getLanguage(): string {
    return "en";
  }
}

class VietnameseLanguageStrategy implements LanguageStrategy {
  getLanguage(): string {
    return "vi";
  }
}
```

#### **Bước 3: Triển khai Email Template cho Từng Ngôn ngữ** ✏️
```typescript
// Template email cho khách hàng (tiếng Anh)
class CustomerEmailTemplateEnglish implements EmailTemplate {
  getSubject(orderId: string): string {
    return "Thank you for your order!";
  }
  getContent(orderId: string): string {
    return `Hello, your order #${orderId} has been confirmed.`;
  }
  getType(): string {
    return "customer";
  }
}

// Template email cho khách hàng (tiếng Việt)
class CustomerEmailTemplateVietnamese implements EmailTemplate {
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

// Tương tự cho các loại email khác (admin, partner) và ngôn ngữ khác.
```

#### **Bước 4: Tạo Factory để Tạo Template theo Ngôn ngữ** 🏭
```typescript
class EmailTemplateFactory {
  static createTemplate(type: string, language: string): EmailTemplate {
    if (language === "en") {
      switch (type) {
        case "customer":
          return new CustomerEmailTemplateEnglish();
        // Thêm các trường hợp khác (admin, partner)
        default:
          throw new Error("Invalid email type!");
      }
    } else if (language === "vi") {
      switch (type) {
        case "customer":
          return new CustomerEmailTemplateVietnamese();
        // Thêm các trường hợp khác (admin, partner)
        default:
          throw new Error("Invalid email type!");
      }
    } else {
      throw new Error("Unsupported language!");
    }
  }
}
```

#### **Bước 5: Tái cấu trúc EmailService** ✂️
```typescript
class EmailService {
  constructor(private languageStrategy: LanguageStrategy) {}

  sendEmail(type: string, email: string, orderId: string): void {
    const language = this.languageStrategy.getLanguage();
    const template = EmailTemplateFactory.createTemplate(type, language);

    this.validateEmail(email);
    this.send(email, template.getSubject(orderId), template.getContent(orderId));
    this.logEmail(template.getType(), email, language);
  }

  private validateEmail(email: string): void {
    if (!email.includes("@")) throw new Error("Invalid email!");
  }

  private send(to: string, subject: string, content: string): void {
    console.log(`Sending email to ${to}: [${subject}] ${content}`);
  }

  private logEmail(type: string, email: string, language: string): void {
    console.log(`Sent ${type} email to ${email} in ${language}`);
  }
}
```

---

### **4. Sử dụng Sau khi Refactor** 🚀
```typescript
// Gửi email bằng tiếng Anh
const englishService = new EmailService(new EnglishLanguageStrategy());
englishService.sendEmail("customer", "user@example.com", "ORDER_123");

// Gửi email bằng tiếng Việt
const vietnameseService = new EmailService(new VietnameseLanguageStrategy());
vietnameseService.sendEmail("customer", "user@example.com", "ORDER_123");
```

---

### **5. Kết quả** 🎉
- **Gửi email tiếng Anh**:
  ```
  Sending email to user@example.com: [Thank you for your order!] Hello, your order #ORDER_123 has been confirmed.
  Sent customer email to user@example.com in en
  ```

- **Gửi email tiếng Việt**:
  ```
  Sending email to user@example.com: [Cảm ơn bạn đã đặt hàng!] Xin chào, đơn hàng #ORDER_123 của bạn đã được xác nhận.
  Sent customer email to user@example.com in vi
  ```

---

### **6. Lợi ích** 🌟
- **Linh hoạt**: Dễ dàng thêm ngôn ngữ mới bằng cách tạo `LanguageStrategy` và `EmailTemplate` tương ứng.
- **Tách biệt trách nhiệm**: Logic ngôn ngữ và nội dung email được tách biệt rõ ràng.
- **Dễ bảo trì**: Thay đổi nội dung email hoặc ngôn ngữ không ảnh hưởng đến quy trình gửi email.

---

### **Bài tập Thực hành cho Bạn** 📝
Hãy mở rộng hệ thống để hỗ trợ thêm:
1. Ngôn ngữ **tiếng Pháp** 🇫🇷.
2. Loại email **thông báo khuyến mãi** (promotion) với nội dung đa ngôn ngữ.

**💡 Gợi ý**:
- Tạo `PromotionEmailTemplateEnglish`, `PromotionEmailTemplateVietnamese`, và `PromotionEmailTemplateFrench`.
- Thêm logic vào `EmailTemplateFactory`.

---