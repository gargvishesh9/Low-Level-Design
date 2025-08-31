# 🟢 Single Responsibility Principle (SRP)

---

## 📖 Definition
- **Single Responsibility Principle (SRP):**  
  *A class should have only one reason to change.*  
- Each class/module should focus on **one well-defined responsibility**.  

---

## 🔑 Why SRP?
- Increases **readability and maintainability**  
- Easier to **test and debug**  
- Supports **scalability**  
- Avoids **tight coupling**  

---

## ❌ Bad Example (Violation of SRP)
```java
class User {
    void register(String email, String password) {
        // register user
    }

    void sendEmail(String email) {
        // email sending logic
    }

    void generateReport() {
        // reporting logic
    }
}
```

### 🚩 Problems
- **Multiple responsibilities**: registration, email sending, reporting.  
- If email system changes → this class must change.  
- If report format changes → this class must change.  

---

## ✅ Correct Example (SRP Applied)
```java
class User {
    void register(String email, String password) {
        // register user
    }
}

class EmailService {
    void sendEmail(String email) {
        // email sending logic
    }
}

class ReportService {
    void generateReport(User user) {
        // reporting logic
    }
}
```

- Each class has **one clear responsibility**.  
- Easier to maintain and test.  

---

## 🎯 Real-World Example
Imagine a **Restaurant Management System**:  
- `OrderService` → handles orders  
- `PaymentService` → handles payments  
- `NotificationService` → sends notifications  

If everything were in one class → **monster class problem**.  

---

## 📝 Benefits of SRP
1. **Clean separation of concerns**  
2. **Code reuse** improves  
3. **Testing is easier** since classes are smaller  
4. **Fewer bugs** when requirements change  

---

## 🎯 Interview Q&A

**Q1. Define SRP in simple words.**  
**A1.** A class should have **only one reason to change** – meaning it should handle only one responsibility.  

---

**Q2. Why is SRP important?**  
**A2.** It reduces code complexity, improves maintainability, makes code testable, and avoids unintended side effects.  

---

**Q3. Give a real-world analogy of SRP.**  
**A3.** In a company:
- HR handles recruitment,  
- Finance handles payments,  
- IT handles infrastructure.  
If one person did all → inefficiency and confusion.  

---

**Q4. How does SRP improve testability?**  
**A4.** Smaller classes with focused responsibilities are easier to **unit test** without dependencies.  

---

**Q5. Is SRP only about classes?**  
**A5.** No. SRP applies to **methods, modules, microservices** – each should do **only one thing well**.  

---
