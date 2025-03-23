Suppose you have a system that sends emails to multiple recipients (customers, admins, partners) with diverse content. The initial code might look like this: ✉️

---

### **1. Original Code (Before Refactoring)** 🛠️
```typescript
class EmailService {
  // Send email to customers
  sendCustomerEmail(customerEmail: string, orderId: string): void {
    const subject = "Thank you for your order!";
    const content = `Hello, your order #${orderId} has been confirmed.`;
    this.validateEmail(customerEmail);
    this.send(customerEmail, subject, content);
    this.logEmail("customer", customerEmail);
  }

  // Send email to admins
  sendAdminEmail(adminEmail: string, orderId: string): void {
    const subject = "New Order Notification";
    const content = `Admin, there is a new order #${orderId} to process.`;
    this.validateEmail(adminEmail);
    this.send(adminEmail, subject, content);
    this.logEmail("admin", adminEmail);
  }

  // Send email to partners
  sendPartnerEmail(partnerEmail: string, orderId: string): void {
    const subject = "Shipping Collaboration";
    const content = `Dear partner, order #${orderId} is ready.`;
    this.validateEmail(partnerEmail);
    this.send(partnerEmail, subject, content);
    this.logEmail("partner", partnerEmail);
  }

  private validateEmail(email: string): void {
    if (!email.includes("@")) throw new Error("Invalid email!");
  }

  private send(to: string, subject: string, content: string): void {
    console.log(`Sending email to ${to}: [${subject}] ${content}`);
  }

  private logEmail(type: string, email: string): void {
    console.log(`Sent ${type} email to ${email}`);
  }
}
```

---

### **2. Analysis of the Original Code** 🔍
- **Code Smells**:
  - **Duplicated Code**: The methods `sendCustomerEmail`, `sendAdminEmail`, and `sendPartnerEmail` have similar structures, differing only in `subject` and `content`.
  - **Primitive Obsession**: Using the `type` parameter (e.g., "customer") as a string is error-prone.
  - **Hard to Extend**: Adding a new email type requires duplicating code.

---

### **3. Refactoring with Strategy Pattern and Composition** 🧩
**Goal**: Separate the logic for creating email content from the email-sending process.

#### **Step 1: Define Email Template (Strategy Interface)** 📝
```typescript
interface EmailTemplate {
  getSubject(orderId: string): string;
  getContent(orderId: string): string;
  getType(): string;
}
```

#### **Step 2: Implement Templates for Each Recipient** ✂️
```typescript
// Composition
class CustomerEmailTemplate implements EmailTemplate {
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

class AdminEmailTemplate implements EmailTemplate {
  getSubject(orderId: string): string {
    return "New Order Notification";
  }
  getContent(orderId: string): string {
    return `Admin, there is a new order #${orderId} to process.`;
  }
  getType(): string {
    return "admin";
  }
}

class PartnerEmailTemplate implements EmailTemplate {
  getSubject(orderId: string): string {
    return "Shipping Collaboration";
  }
  getContent(orderId: string): string {
    return `Dear partner, order #${orderId} is ready.`;
  }
  getType(): string {
    return "partner";
  }
}
```

#### **Step 3: Refactor EmailService** 🔧
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

  // General method to send emails
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

### **4. Usage After Refactoring** 🚀
```typescript
const emailService = new EmailService();

// Send email to customers
emailService.sendEmail("customer", "user@example.com", "ORDER_123");

// Send email to admins
emailService.sendEmail("admin", "admin@company.com", "ORDER_123");

// Send email to partners
emailService.sendEmail("partner", "partner@company.com", "ORDER_123");
```

---

### **5. Benefits After Refactoring** 🌟
- **Reduced Duplicated Code**: Only one `sendEmail` method instead of multiple individual methods.
- **Easier to Extend**: Adding a new email type only requires creating a new `EmailTemplate` implementation.
- **Adheres to Open/Closed Principle**: Extend functionality without modifying existing code.
- **Separation of Concerns**: Email content creation logic is separated from the email-sending process.

---

### **Practice Exercise for You** 📝
Refactor the following code to support **promotional emails** and **account verification emails**:
```typescript
class PromotionalEmailService {
  sendPromotionEmail(userEmail: string, discountCode: string): void {
    const subject = "Special Promotion!";
    const content = `You have received a discount code: ${discountCode}`;
    this.validateEmail(userEmail);
    this.send(userEmail, subject, content);
    this.logEmail("promotion", userEmail);
  }

  sendVerificationEmail(userEmail: string, token: string): void {
    const subject = "Account Verification";
    const content = `Please enter the verification code: ${token}`;
    this.validateEmail(userEmail);
    this.send(userEmail, subject, content);
    this.logEmail("verification", userEmail);
  }

  private validateEmail(email: string): void {
    if (!email.includes("@")) throw new Error("Invalid email!");
  }

  private send(to: string, subject: string, content: string): void {
    console.log(`Sending email to ${to}: [${subject}] ${content}`);
  }

  private logEmail(type: string, email: string): void {
    console.log(`Sent ${type} email to ${email}`);
  }
}
```

**💡 Hint**:
- Apply the **Strategy Pattern** similar to the example above.
- Create `PromotionEmailTemplate` and `VerificationEmailTemplate` classes.

---

### **Expected Result** 🎯
```typescript
interface EmailTemplate {
  getSubject(data: string): string;
  getContent(data: string): string;
  getType(): string;
}

class PromotionEmailTemplate implements EmailTemplate {
  getSubject(discountCode: string): string {
    return "Special Promotion!";
  }
  getContent(discountCode: string): string {
    return `You have received a discount code: ${discountCode}`;
  }
  getType(): string {
    return "promotion";
  }
}

class VerificationEmailTemplate implements EmailTemplate {
  getSubject(token: string): string {
    return "Account Verification";
  }
  getContent(token: string): string {
    return `Please enter the verification code: ${token}`;
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
    // ... email sending logic
  }
}
```

---

**[Bonus 2: Send multilingual emails](bonus2-send-multilingual-emails.md)** ➡️