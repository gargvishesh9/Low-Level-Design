
# 🦅 Liskov Substitution Principle (LSP)

---

## 📖 Definition
- Proposed by **Barbara Liskov** in 1987.  
- **Formal definition:**  
  *If S is a subtype of T, then objects of type T can be replaced with objects of type S without altering the correctness of the program.*  

---

## 🔑 Key Idea
- Subtypes must **honor the contract** of their parent type.  
- A derived class should **enhance or extend** the behavior, not **break it**.  

---

## ❌ Common Violations of LSP
1. **Throwing unexpected exceptions**  
   ```java
   class Bird {
       void fly() { System.out.println("Flying..."); }
   }

   class Penguin extends Bird {
       @Override
       void fly() { throw new UnsupportedOperationException("Penguins can’t fly"); }
   }
   ```

🚩 Violates LSP because `Penguin` cannot behave like a `Bird` that flies.

---

2. **Changing method expectations**

   ```java
   class Rectangle {
       protected int width;
       protected int height;

       void setWidth(int w) { this.width = w; }
       void setHeight(int h) { this.height = h; }

       int area() { return width * height; }
   }

   class Square extends Rectangle {
       @Override
       void setWidth(int w) { this.width = this.height = w; }
       @Override
       void setHeight(int h) { this.width = this.height = h; }
   }
   ```

   🚩 Violates LSP because `Square` breaks the expected independent behavior of `Rectangle`.

---

## ✅ Correct Design

* Use **interfaces** and proper **abstractions**.
* Don’t force a subclass into behaviors it cannot fulfill.

```java
interface Bird {}

interface FlyingBird extends Bird {
    void fly();
}

class Sparrow implements FlyingBird {
    public void fly() {
        System.out.println("Sparrow flying...");
    }
}

class Ostrich implements Bird {
    // Ostrich does not fly, but still a bird
}
```

---

## 🎯 Real-World Examples

* **Vehicles Example:**

  * A `Car` class with `refuel()` method.
  * A `Tesla` subclass that doesn’t use fuel → violates LSP if forced to implement `refuel()`.
  * Fix: Separate `FuelVehicle` and `ElectricVehicle` interfaces.

* **Payments Example:**

  * `Payment` base class.
  * If `CryptoPayment` subclass throws "Unsupported" exception, it breaks LSP.
  * Fix: Separate supported payment types into different abstractions.

---

## 📝 Why LSP Matters

1. Prevents **unexpected bugs** when using polymorphism.
2. Makes code **predictable and reliable**.
3. Promotes **reusability** and **extensibility**.
4. Essential for **clean OOP design**.

---

## 🎯 Interview Q\&A

**Q1. Define the Liskov Substitution Principle.**

**A1.** Subtypes must be usable in place of their base types without breaking correctness.

---

**Q2. Give a classic example of LSP violation.**

**A2.** `Square` subclassing `Rectangle` because it changes the behavior of width/height independently.

---

**Q3. How do you avoid LSP violations?**

**A3.**

* Design clear **contracts**
* Use **interfaces** wisely
* Prefer **composition over inheritance**

---

**Q4. Which design patterns rely on LSP?**

**A4.**

* Strategy Pattern
* Template Method Pattern
* Bridge Pattern

---

**Q5. Why is LSP important in real-world applications?**

**A5.** It ensures **polymorphism works correctly**, so new subclasses don’t break existing functionality.

---


```
