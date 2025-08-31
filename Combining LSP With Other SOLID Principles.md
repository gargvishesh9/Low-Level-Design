
# 🔗 Combining LSP with Other SOLID Principles

---

## 📖 Introduction
- The **Liskov Substitution Principle (LSP)** ensures that subtypes can **replace base types safely**.  
- On its own, it prevents **unexpected behavior** in inheritance.  
- When combined with **SRP** and **OCP**, it creates scalable and predictable designs.  

---

## 🔑 How LSP Connects with Other Principles

### 1️⃣ LSP + SRP (Single Responsibility Principle)
- SRP says: *One class = One responsibility*.  
- LSP says: *Subtypes must honor the contract of their base class*.  
- If a class violates SRP (handles multiple responsibilities), then its subtypes will also likely violate LSP.  

✅ Example:  
- A `Bird` class handling both "flying" and "walking" → Ostrich will break LSP.  
- Fixing SRP by separating **FlyingBird** and **NonFlyingBird** → also fixes LSP.  

---

### 2️⃣ LSP + OCP (Open-Closed Principle)
- OCP says: *Open for extension, closed for modification*.  
- If subclasses violate LSP, the base class often needs modification to handle exceptions 🚩 breaking OCP.  

✅ Example:  
- A `PaymentProcessor` modified every time a new payment type doesn’t fit → violates OCP.  
- Fix: Define `Payment` interface with consistent behavior → new payment types can extend without breaking LSP or OCP.  

---

## 🎯 Real-World Example (LSP + SRP + OCP Together)

### ❌ Bad Design
```java
class Rectangle {
    int width, height;
    void setWidth(int w) { this.width = w; }
    void setHeight(int h) { this.height = h; }
}

class Square extends Rectangle {
    void setWidth(int w) { this.width = this.height = w; }
    void setHeight(int h) { this.width = this.height = h; }
}
````

* Violates **LSP** → Square breaks Rectangle’s contract.
* Violates **SRP** → Rectangle is trying to handle two shapes at once.
* Violates **OCP** → Must modify Rectangle to support new shapes.

---

### ✅ Good Design

```java
interface Shape {
    int area();
}

class Rectangle implements Shape {
    private int width, height;
    public Rectangle(int w, int h) { this.width = w; this.height = h; }
    public int area() { return width * height; }
}

class Square implements Shape {
    private int side;
    public Square(int s) { this.side = s; }
    public int area() { return side * side; }
}
```

* SRP: Each class models **only one shape**.
* LSP: Both `Square` and `Rectangle` behave correctly as `Shape`.
* OCP: Adding a `Circle` requires **no modification** to existing code.

---

## 🎯 Interview Q\&A

**Q1. How does LSP connect with SRP?**

**A1.** If a class violates SRP, its subclasses are likely to violate LSP by not being able to honor all responsibilities.

---

**Q2. How does LSP reinforce OCP?**

**A2.** By ensuring subclasses can safely replace parents, you can extend behavior (OCP) without modifying existing code.

---

**Q3. Can you give a real-world example combining LSP, SRP, and OCP?**

**A3.** Payment system:

* SRP → Separate `CreditCardPayment`, `PayPalPayment`.
* LSP → Each implements `Payment` correctly.
* OCP → Add `CryptoPayment` without modifying others.

---

```
