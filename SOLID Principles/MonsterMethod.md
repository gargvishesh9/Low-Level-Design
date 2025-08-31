

---

# 🐲 Monster Method

---

## 📖 What is a Monster Method?

* A **Monster Method** is a method that:

  * Is **too long**
  * Does **too many things**
  * Handles **multiple responsibilities**
* Violates the **Single Responsibility Principle (SRP)** at the method level.

---

## ❌ Example of a Monster Method

```java
public void processOrder(Order order) {
    // Validate order
    if(order == null || order.items.isEmpty()) {
        throw new IllegalArgumentException("Invalid order");
    }

    // Calculate total price
    double total = 0;
    for(Item i : order.items) {
        total += i.price;
    }

    // Save to database
    Database.save(order);

    // Send confirmation email
    EmailService.send(order.customerEmail, "Order confirmed!");

    // Generate invoice
    PDFGenerator.generate(order);
}
```

### 🚩 Problems

* Validation, calculation, persistence, communication, and reporting **all mixed together**.
* Hard to read, test, and maintain.

---

## ✅ Refactored Example

```java
public void processOrder(Order order) {
    validate(order);
    double total = calculator.calculateTotal(order);
    repository.save(order);
    notifier.sendConfirmation(order);
    reportService.generateInvoice(order);
}

private void validate(Order order) {
    if(order == null || order.items.isEmpty()) {
        throw new IllegalArgumentException("Invalid order");
    }
}
```

* Now responsibilities are **split into smaller methods/classes**.
* Each method/class is **focused and reusable**.

---

## 🎯 Why Avoid Monster Methods?

1. Difficult to **understand**
2. Difficult to **unit test**
3. Increases **bug probability**
4. Harder to **refactor**
5. Violates **clean code principles**

---

## 📝 Best Practices

* Break large methods into **smaller private methods**.
* Follow **SRP at method level**.
* Apply **naming conventions** that reflect responsibility.
* Use **design patterns** (like Strategy, Observer) where suitable.

---

## 🎯 Interview Q\&A

**Q1. What is a Monster Method?**

**A1.** A method that is **too long** and handles **multiple responsibilities** instead of focusing on a single task.

---

**Q2. Why are Monster Methods bad?**

**A2.** They reduce readability, make testing harder, increase coupling, and violate SRP.

---

**Q3. How do you fix a Monster Method?**

**A3.** By **refactoring**:

* Extract smaller methods
* Delegate responsibilities to other classes/services

---

**Q4. Give a real-world example of a Monster Method.**

**A4.** A `processPayment()` method that validates cards, connects to the bank, logs transactions, sends notifications, and updates reports – all in one place.

---

**Q5. Is refactoring Monster Methods risky?**

**A5.** Yes, if not covered by tests. Always **add unit tests first** and then refactor to ensure behavior is preserved.

---

