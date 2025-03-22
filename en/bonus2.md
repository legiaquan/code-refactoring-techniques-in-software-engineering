To support **multilingual emails**, we can extend the system using the **Strategy Pattern** combined with the **Factory Pattern** to create email templates suitable for the user's language. Below is a detailed example: ✉️🌐

---

### **1. Requirements** 📋
- Support sending emails in multiple languages (e.g., English, Vietnamese).
- Each type of email (customer, admin, partner) can be sent in multiple languages.

---

### **2. Structures** 🛠️
- **EmailTemplate**: A common interface for email templates.
- **LanguageStrategy**: An interface to define the language.
- **EmailService**: Uses `EmailTemplate` and `LanguageStrategy` to send emails.

---

### **3. Code Implementation** 💻

#### **Step 1: Define Interface and Strategy** 🧩
```typescript
// Interface for Email Template
interface EmailTemplate {
  getSubject(orderId: string): string;
  getContent(orderId: string): string;
  getType(): string;
}

// Interface for Language Strategy
interface LanguageStrategy {
  getLanguage(): string;
}
```

#### **Step 2: Implement Language Strategy** 🌍
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

#### **Step 3: Implement Email Templates for Each Language** ✏️
```typescript
// Email template for customers (English)
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

// Email template for customers (Vietnamese)
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

// Similarly, implement templates for other email types (admin, partner) and languages.
```

#### **Step 4: Create a Factory to Generate Templates by Language** 🏭
```typescript
class EmailTemplateFactory {
  static createTemplate(type: string, language: string): EmailTemplate {
    if (language === "en") {
      switch (type) {
        case "customer":
          return new CustomerEmailTemplateEnglish();
        // Add other cases (admin, partner)
        default:
          throw new Error("Invalid email type!");
      }
    } else if (language === "vi") {
      switch (type) {
        case "customer":
          return new CustomerEmailTemplateVietnamese();
        // Add other cases (admin, partner)
        default:
          throw new Error("Invalid email type!");
      }
    } else {
      throw new Error("Unsupported language!");
    }
  }
}
```

#### **Step 5: Refactor EmailService** ✂️
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

### **4. Usage After Refactoring** 🚀
```typescript
// Send email in English
const englishService = new EmailService(new EnglishLanguageStrategy());
englishService.sendEmail("customer", "user@example.com", "ORDER_123");

// Send email in Vietnamese
const vietnameseService = new EmailService(new VietnameseLanguageStrategy());
vietnameseService.sendEmail("customer", "user@example.com", "ORDER_123");
```

---

### **5. Results** 🎉
- **Send email in English**:
  ```
  Sending email to user@example.com: [Thank you for your order!] Hello, your order #ORDER_123 has been confirmed.
  Sent customer email to user@example.com in en
  ```

- **Send email in Vietnamese**:
  ```
  Sending email to user@example.com: [Cảm ơn bạn đã đặt hàng!] Xin chào, đơn hàng #ORDER_123 của bạn đã được xác nhận.
  Sent customer email to user@example.com in vi
  ```

---

### **6. Benefits** 🌟
- **Flexibility**: Easily add new languages by creating corresponding `LanguageStrategy` and `EmailTemplate`.
- **Separation of Concerns**: Language logic and email content are clearly separated.
- **Maintainability**: Changing email content or language does not affect the email-sending process.

---

### **Practice Exercise for You** 📝
Extend the system to support:
1. The **French** language 🇫🇷.
2. A new email type for **promotional notifications** (promotion) with multilingual content.

**💡 Hint**:
- Create `PromotionEmailTemplateEnglish`, `PromotionEmailTemplateVietnamese`, and `PromotionEmailTemplateFrench`.
- Add logic to the `EmailTemplateFactory`.

---