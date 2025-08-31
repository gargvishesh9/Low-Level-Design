
# ⚖️ Interface Segregation Principle (ISP)

---

## 📖 Definition
- **Interface Segregation Principle (ISP):**  
  *Clients should not be forced to depend on methods they do not use.*  
- Instead of one large, "fat" interface → create **smaller, specific interfaces**.  

---

## ❌ Bad Example (ISP Violation)
```java
interface Worker {
    void work();
    void eat();
}

class HumanWorker implements Worker {
    public void work() { System.out.println("Human working..."); }
    public void eat() { System.out.println("Human eating lunch..."); }
}

class RobotWorker implements Worker {
    public void work() { System.out.println("Robot working..."); }
    public void eat() { throw new UnsupportedOperationException("Robots don’t eat!"); }
}
````

🚩 Problem:

* `RobotWorker` is forced to implement `eat()`, which it cannot do.
* Violates ISP.

---

## ✅ Good Example (ISP Applied)

```java
interface Workable {
    void work();
}

interface Eatable {
    void eat();
}

class HumanWorker implements Workable, Eatable {
    public void work() { System.out.println("Human working..."); }
    public void eat() { System.out.println("Human eating lunch..."); }
}

class RobotWorker implements Workable {
    public void work() { System.out.println("Robot working..."); }
}
```

* Now **Robot** doesn’t depend on methods it cannot use.
* Each interface is **focused and clean**.

---

## 🎯 Real-World Example

* Imagine an **Appliance interface** with methods:

  * `turnOn()`, `turnOff()`, `changeBattery()`.
* A **TV** and **Fan** don’t need `changeBattery()`.
* A **Remote Control** does, but not `turnOn()` in the same way.
* Solution: Separate into **PowerControllable** and **BatteryOperated** interfaces.

---

## 📝 Why ISP Matters

1. Prevents **unnecessary dependencies**.
2. Reduces **bloat** in interfaces.
3. Makes systems **easier to extend**.
4. Improves **testability** and **maintainability**.

---

## 🎯 Best Practices

* Keep interfaces **small and focused**.
* Follow the rule: *“Many client-specific interfaces are better than one general-purpose interface.”*
* Use **role-based interfaces** (like `Eatable`, `Workable`) instead of generic ones.

---

## 🎯 Interview Q\&A

**Q1. What is the Interface Segregation Principle?**

**A1.** ISP states that a client should not be forced to implement methods it does not use.

---

**Q2. How do you identify an ISP violation?**

**A2.** When a class is forced to throw exceptions or leave methods unimplemented because they don’t make sense for it.

---

**Q3. Give a real-world ISP violation example.**

**A3.** A `Printer` interface with methods `print()`, `scan()`, `fax()`.

* A basic printer that only prints would be forced to implement `scan()` and `fax()`.

---

**Q4. How do you fix ISP violations?**

**A4.** Split into smaller, more specific interfaces → `Printable`, `Scannable`, `Faxable`.

---

**Q5. Which design principle works closely with ISP?**

**A5.** LSP (Liskov Substitution Principle) → smaller interfaces ensure subclasses don’t break expected contracts.


```
