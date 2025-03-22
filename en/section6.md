We will move on to **Section 6: Refactoring and Software Architecture**. In this section, we will explore how refactoring improves software architecture and how to use design patterns to solve complex problems. All examples use **TypeScript**. ✨

---

### **Section 6: Refactoring and Software Architecture** 🏗️

#### **1. Refactoring to Improve Architecture** 🔧
Refactoring is not just about cleaning up "dirty" code — it also helps to gradually and safely change the software architecture. 

**Problem**: A monolithic application needs to transition to a **microservices** architecture.

**Approach**:
- Split modules into independent services using **Dependency Injection**. 🧩
- Use an **API Gateway** to manage the services. 🌐

**Example in TypeScript**:
```typescript
// ❌ Before refactoring: Monolithic
class MonolithicApp {
  processOrder(order: Order) {
    const payment = new PaymentService().process(order);
    const inventory = new InventoryService().update(order);
    return { payment, inventory };
  }
}

// ✅ After refactoring: Microservices
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

#### **2. Using Design Patterns in Refactoring** 🧩
Design patterns help solve recurring architectural problems. Here are some examples:

##### **a. Factory Method Pattern** 🏭
Helps create objects without specifying their concrete classes.

**Example**:
```typescript
// ❌ Before refactoring
class Logger {
  private type: "file" | "database";

  constructor(type: "file" | "database") {
    this.type = type;
  }

  log(message: string) {
    if (this.type === "file") {
      // Log to file
    } else if (this.type === "database") {
      // Log to database
    }
  }
}

// ✅ After refactoring
interface Logger {
  log(message: string): void;
}

class FileLogger implements Logger {
  log(message: string) { /* Log to file */ }
}

class DatabaseLogger implements Logger {
  log(message: string) { /* Log to database */ }
}

class LoggerFactory {
  static createLogger(type: "file" | "database"): Logger {
    return type === "file" ? new FileLogger() : new DatabaseLogger();
  }
}
```

##### **b. Observer Pattern** 👀
Allows objects to observe and react to events from other objects.

**Example**:
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

#### **3. Refactoring to Reduce Coupling** 🔗
**Tight coupling** is the enemy of good architecture. Refactoring helps reduce coupling by using **Dependency Injection** or **Interfaces**.

**Example**:
```typescript
// ❌ Before refactoring: Tight coupling
class UserService {
  private database = new MySQLDatabase(); // Direct dependency on MySQL
}

// ✅ After refactoring: Dependency Injection
interface Database {
  save(data: any): void;
}

class MySQLDatabase implements Database {
  save(data: any) { /* ... */ }
}

class UserService {
  constructor(private database: Database) {} // Depends on an interface
}
```

---

#### **4. Refactoring and Testing** 🧪
A good architecture should be testable. Refactoring helps separate business logic from dependencies (e.g., database, API).

**Example**:
```typescript
// ❌ Before refactoring: Hard to test due to database dependency
class ProductService {
  calculateTotalPrice(productId: string) {
    const product = new Database().getProduct(productId); // Direct dependency
    return product.price * product.quantity;
  }
}

// ✅ After refactoring: Easier to test with Mock
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

// Using Mock in tests
class MockRepository implements ProductRepository {
  getProduct(productId: string) {
    return { price: 100, quantity: 2 }; // Mock data
  }
}

// Test
const service = new ProductService(new MockRepository());
console.log(service.calculateTotalPrice("1")); // 200
```

---

### **Practice Exercise** 📝
Refactor the following code to improve its architecture:
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

💡 **Hints**:
- Use **Dependency Injection** to decouple services. 🔗
- Apply the **Facade** or **Command** pattern to encapsulate the processing workflow. 🧩

---

### **Expected Result** 🎉
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

// Usage
const processor = new OrderProcessingFacade([
  new OrderValidator(),
  new PaymentProcessor(),
  // ...
]);
```

---

 **[Section 7: Refactoring Tools](section7.md)** ➡️ 😊