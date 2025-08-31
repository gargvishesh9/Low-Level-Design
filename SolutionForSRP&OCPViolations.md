
# 🛠️ Solutions for SRP & OCP Violations

---

## 📖 Introduction
- **SRP (Single Responsibility Principle)** → A class should have only **one reason to change**.  
- **OCP (Open-Closed Principle)** → Classes should be **open for extension** but **closed for modification**.  

Violations of these principles lead to **rigid, fragile, and unmaintainable code**.  
This section covers **common violations** and **how to fix them**.  

---

## ❌ Example: SRP + OCP Violation
```java
class PaymentProcessor {
    public void processPayment(String type, double amount) {
        if(type.equals("CreditCard")) {
            // credit card logic
        } else if(type.equals("PayPal")) {
            // PayPal logic
        } else if(type.equals("UPI")) {
            // UPI logic
        }
    }
}
````

### 🚩 Problems

* **SRP Violation** → Handles logic for multiple payment types.
* **OCP Violation** → Adding a new payment type requires **modifying this class**.

---

## ✅ Solution (Apply SRP + OCP with Polymorphism)

```java
interface Payment {
    void pay(double amount);
}

class CreditCardPayment implements Payment {
    public void pay(double amount) {
        System.out.println("Paid " + amount + " using Credit Card");
    }
}

class PayPalPayment implements Payment {
    public void pay(double amount) {
        System.out.println("Paid " + amount + " using PayPal");
    }
}

class UpiPayment implements Payment {
    public void pay(double amount) {
        System.out.println("Paid " + amount + " using UPI");
    }
}

class PaymentProcessor {
    private Payment payment;

    public PaymentProcessor(Payment payment) {
        this.payment = payment;
    }

    public void process(double amount) {
        payment.pay(amount);
    }
}
```

### ✅ Benefits

* **SRP Applied** → Each payment method has its own class.
* **OCP Applied** → Adding a new payment type (e.g., CryptoPayment) requires **no modification** to existing code.

---

## 🎯 Best Practices

1. Identify **classes with multiple responsibilities** → split them.
2. Use **interfaces and abstraction** for extending behavior.
3. Prefer **composition over inheritance**.
4. Apply **design patterns** like:

   * Strategy Pattern
   * Factory Pattern
   * Dependency Injection

---

## 🎯 Interview Q\&A

**Q1. How can one class violate both SRP and OCP?**

**A1.** If a class has **multiple reasons to change** and requires modification when adding new features → it violates both SRP and OCP.

---

**Q2. How do interfaces help in solving these violations?**

**A2.** Interfaces allow us to define **contracts**. New behavior can be added via **new classes** without modifying existing ones.

---

**Q3. Which design pattern is most commonly used to fix OCP violations?**

**A3.** The **Strategy Pattern**. It lets us define a family of algorithms (behaviors) and makes them interchangeable.

---

**Q4. Give a real-world analogy of SRP + OCP solution.**

**A4.** Think of a **payment gateway**:

* Each payment method (CreditCard, PayPal, UPI) is a separate **module**.
* The gateway doesn’t need to change when a new method is added.

---

**Q5. What happens if we ignore SRP & OCP?**

**A5.** Code becomes **rigid, fragile, unscalable**, and difficult to test. Any new change may break existing features.

---

