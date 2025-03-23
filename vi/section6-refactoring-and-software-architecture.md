Chúng ta sẽ chuyển sang **Section 6: Refactoring và Kiến trúc Phần mềm**. Trong phần này, chúng ta sẽ khám phá cách refactoring giúp cải thiện kiến trúc phần mềm và cách sử dụng các mẫu thiết kế (design patterns) để giải quyết các vấn đề phức tạp. Tất cả ví dụ đều sử dụng **TypeScript**. ✨

---

### **Section 6: Refactoring và Kiến trúc Phần mềm** 🏗️

#### **1. Refactoring để Cải thiện Kiến trúc** 🔧
Refactoring không chỉ dừng lại ở việc sửa mã "bẩn" — nó còn giúp thay đổi kiến trúc phần mềm một cách **từ từ** và **an toàn**. 

**Bài toán**: Một ứng dụng có kiến trúc **monolithic** (khối lớn) cần chuyển sang **microservices**.

**Cách tiếp cận**:
- Tách các module thành các service độc lập bằng cách sử dụng **Dependency Injection**.
- Sử dụng **API Gateway** để quản lý các service.

**Ví dụ trong TypeScript**:
```typescript
// ❌ Trước khi refactor: Monolithic
class MonolithicApp {
  processOrder(order: Order) {
    const payment = new PaymentService().process(order);
    const inventory = new InventoryService().update(order);
    return { payment, inventory };
  }
}

// ✅ Sau khi refactor: Microservices
interface Service {
  process(order: Order): Promise<any>;
}

class PaymentService implements Service {
  async process(order: Order) { /* ... */ }
}

class InventoryService implements Service {
  async process(order: Order) { /* ... */ }
}

class ApiGateway {
  constructor(private services: Service[]) {}

  async processOrder(order: Order) {
    return Promise.all(this.services.map(service => service.process(order)));
  }
}
```

---

#### **2. Sử dụng Design Patterns trong Refactoring** 🧩
Các mẫu thiết kế giúp giải quyết các vấn đề kiến trúc lặp lại. Dưới đây là một số ví dụ:

##### **a. Factory Method Pattern** 🏭
Giúp tạo đối tượng mà không cần chỉ định lớp cụ thể.

**Ví dụ**:
```typescript
// ❌ Trước khi refactor
class Logger {
  private type: "file" | "database";

  constructor(type: "file" | "database") {
    this.type = type;
  }

  log(message: string) {
    if (this.type === "file") {
      // Ghi vào file
    } else if (this.type === "database") {
      // Ghi vào database
    }
  }
}

// ✅ Sau khi refactor
interface Logger {
  log(message: string): void;
}

class FileLogger implements Logger {
  log(message: string) { /* Ghi vào file */ }
}

class DatabaseLogger implements Logger {
  log(message: string) { /* Ghi vào database */ }
}

class LoggerFactory {
  static createLogger(type: "file" | "database"): Logger {
    return type === "file" ? new FileLogger() : new DatabaseLogger();
  }
}
```

##### **b. Observer Pattern** 👀
Giúp các đối tượng theo dõi và phản ứng với sự kiện từ đối tượng khác.

**Ví dụ**:
```typescript
// Publisher (Subject)
class NewsPublisher {
  private subscribers: Subscriber[] = [];

  subscribe(subscriber: Subscriber) {
    this.subscribers.push(subscriber);
  }

  notify(news: string) {
    this.subscribers.forEach(subscriber => subscriber.update(news));
  }
}

// Subscriber
interface Subscriber {
  update(news: string): void;
}

class EmailSubscriber implements Subscriber {
  update(news: string) {
    console.log(`Sending email: ${news}`);
  }
}
```

---

#### **3. Refactoring để Giảm Sự Phụ thuộc (Coupling)** 🔗
**Tight coupling** (sự phụ thuộc chặt chẽ) là kẻ thù của kiến trúc tốt. Refactoring giúp giảm coupling bằng cách sử dụng **Dependency Injection** hoặc **Interface**.

**Ví dụ**:
```typescript
// ❌ Trước khi refactor: Tight coupling
class UserService {
  private database = new MySQLDatabase(); // Phụ thuộc trực tiếp vào MySQL
}

// ✅ Sau khi refactor: Dependency Injection
interface Database {
  save(data: any): void;
}

class MySQLDatabase implements Database {
  save(data: any) { /* ... */ }
}

class UserService {
  constructor(private database: Database) {} // Phụ thuộc vào interface
}
```

---

#### **4. Refactoring và Kiểm thử (Testing)** 🧪
Một kiến trúc tốt phải dễ kiểm thử. Refactoring giúp tách biệt logic nghiệp vụ và các thành phần phụ thuộc (ví dụ: database, API).

**Ví dụ**:
```typescript
// ❌ Trước khi refactor: Khó test vì phụ thuộc vào database
class ProductService {
  calculateTotalPrice(productId: string) {
    const product = new Database().getProduct(productId); // Phụ thuộc trực tiếp
    return product.price * product.quantity;
  }
}

// ✅ Sau khi refactor: Dễ test với Mock
interface ProductRepository {
  getProduct(productId: string): Product;
}

class ProductService {
  constructor(private repository: ProductRepository) {}

  calculateTotalPrice(productId: string) {
    const product = this.repository.getProduct(productId);
    return product.price * product.quantity;
  }
}

// Sử dụng Mock trong test
class MockRepository implements ProductRepository {
  getProduct(productId: string) {
    return { price: 100, quantity: 2 }; // Dữ liệu giả
  }
}

// Test
const service = new ProductService(new MockRepository());
console.log(service.calculateTotalPrice("1")); // 200
```

---

### **Bài tập Thực hành** 📝
Hãy refactor đoạn mã sau để cải thiện kiến trúc:
```typescript
class OrderProcessor {
  process(order: Order) {
    const validator = new OrderValidator();
    if (!validator.validate(order)) throw new Error("Invalid order");

    const payment = new PaymentService().process(order);
    new InventoryService().update(order);
    new EmailService().sendConfirmation(order.userEmail);
  }
}
```

💡 **Gợi ý**:
- Sử dụng **Dependency Injection** để tách các service.
- Áp dụng mẫu **Facade** hoặc **Command** để đóng gói quy trình xử lý.

---

### **Kết quả Mong đợi**: 🎉
```typescript
interface OrderHandler {
  handle(order: Order): void;
}

class OrderValidator implements OrderHandler {
  handle(order: Order) {
    if (!this.validate(order)) throw new Error("Invalid order");
  }
  // ...
}

class PaymentProcessor implements OrderHandler {
  handle(order: Order) { /* ... */ }
}

class OrderProcessingFacade {
  constructor(private handlers: OrderHandler[]) {}

  process(order: Order) {
    this.handlers.forEach(handler => handler.handle(order));
  }
}

// Sử dụng
const processor = new OrderProcessingFacade([
  new OrderValidator(),
  new PaymentProcessor(),
  // ...
]);
```

---

 **[Section 7: Refactoring Tools](section7-refactoring-tools.md)** ➡️ 😊