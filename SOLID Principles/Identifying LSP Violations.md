
# 🚨 Identifying LSP Violations

---

## 📖 What is LSP?
- **Liskov Substitution Principle (LSP):**  
  *Subtypes must be substitutable for their base types without altering the correctness of the program.*  
- Defined by **Barbara Liskov**.  

### In simple words:  
If `B` is a subclass of `A`, then we should be able to use `B` **anywhere** `A` is expected — without breaking functionality.  

---

## ❌ Example of LSP Violation
```java
class Bird {
    void fly() {
        System.out.println("Flying...");
    }
}

class Ostrich extends Bird {
    @Override
    void fly() {
        throw new UnsupportedOperationException("Ostrich can't fly!");
    }
}
````

### 🚩 Problems

* **Ostrich** is a `Bird`, but it **can’t fly**.
* Violates **LSP** because substituting `Ostrich` where `Bird` is expected breaks behavior.

---

## ✅ Solution (Follow LSP)

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
    // Ostrich does not have fly()
}
```

* Now **Ostrich** is still a `Bird` but does not break expectations.
* We separated **behavioral contracts** into appropriate abstractions.

---

## 🎯 Real-World Example

* Suppose you have a `Payment` class.
* If `CryptoPayment` subclass throws an error because "crypto not supported yet", it breaks LSP.
* Instead → design separate abstractions for supported vs. unsupported payments.

---

## 📝 How to Identify LSP Violations

1. Subclass **removes expected behavior** (e.g., method not supported).
2. Subclass **strengthens preconditions** (e.g., requires more inputs than parent).
3. Subclass **weakens postconditions** (e.g., gives incomplete results).
4. Subclass throws **unexpected exceptions**.

---

## 🎯 Interview Q\&A

**Q1. What does LSP state?**

**A1.** Subtypes should be usable **in place of their base types** without breaking functionality.

---

**Q2. How can you detect LSP violations in code?**

**A2.** Look for:

* Methods overridden to throw exceptions
* Subclasses that add unnecessary constraints
* Behavior inconsistent with the parent class

---

**Q3. Give a real-world violation of LSP.**

**A3.**

* A `Square` class extending `Rectangle`.
* 
* Changing width also changes height, which breaks expectations of `Rectangle`.

---

**Q4. How to fix LSP violations?**

**A4.** Use **interfaces and proper abstractions**. Ensure subclasses honor the contract of the base type.

---

**Q5. Which design patterns help avoid LSP violations?**
**A5.**

* **Composition over inheritance**
* **Strategy Pattern**

---

