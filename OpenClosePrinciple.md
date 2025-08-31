
# 🔒 Open-Closed Principle (OCP)

---

## 📖 Definition
- **Open-Closed Principle (OCP):**  
  *Classes should be **open for extension**, but **closed for modification***.  
- You should be able to add **new functionality** without changing existing code.  

---

## ❌ Bad Example (Violation of OCP)
```java
class Invoice {
    public double calculateTax(String type, double amount) {
        if(type.equals("GST")) {
            return amount * 0.18;
        } else if(type.equals("VAT")) {
            return amount * 0.20;
        }
        return 0;
    }
}
````

### 🚩 Problems

* Every time a new tax type is added → you must **modify the class**.
* Breaks OCP → not scalable.

---

## ✅ Good Example (OCP Applied with Polymorphism)

```java
interface Tax {
    double calculate(double amount);
}

class GST implements Tax {
    public double calculate(double amount) {
        return amount * 0.18;
    }
}

class VAT implements Tax {
    public double calculate(double amount) {
        return amount * 0.20;
    }
}

class Invoice {
    private Tax tax;

    public Invoice(Tax tax) {
        this.tax = tax;
    }

    public double calculateTax(double amount) {
        return tax.calculate(amount);
    }
}
```

* Adding a new tax type → just create a new class (e.g., `LuxuryTax`).
* No need to **modify** existing classes.

---

## 🎯 Real-World Analogy

* Think of a **power socket**:

  * The socket is fixed (closed for modification).
  * You can plug in different devices (open for extension).

---

## 📝 Benefits of OCP

1. Improves **extensibility**
2. Reduces **risk of breaking existing code**
3. Encourages **polymorphism and abstraction**
4. Makes code **scalable and maintainable**

---

## 🎯 Interview Q\&A

**Q1. Explain OCP in simple words.**

**A1.** You should be able to **add new features** without changing existing code.

---

**Q2. How is OCP implemented in Java?**

**A2.** Through **abstraction** (interfaces/abstract classes) and **polymorphism**.

---

**Q3. Give a real-world analogy of OCP.**

**A3.** A phone with a SIM slot → the phone (class) doesn’t change, you just insert a new SIM (extension).

---

**Q4. Why is modifying classes risky?**

**A4.** It may **break existing functionality** and introduce **new bugs**.

---

**Q5. What design patterns support OCP?**

**A5.**

* Strategy Pattern
* Decorator Pattern
* Factory Pattern

---


```


