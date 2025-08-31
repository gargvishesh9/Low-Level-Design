
# 🗂️ Utility Folder

---

## 📖 What is a Utility Folder?
- A **Utility Folder** is a place where developers dump **unrelated static methods**.  
- Usually named `utils`, `helpers`, or `common`.  
- Becomes a **junk drawer** of methods that don’t belong anywhere else.  

---

## ❌ Example of a Utility Class
```java
class StringUtils {
    public static boolean isNullOrEmpty(String s) {
        return s == null || s.isEmpty();
    }

    public static void sendEmail(String email, String msg) {
        // email sending logic ❌ (doesn’t belong here)
    }

    public static void logToFile(String data) {
        // logging logic ❌ (doesn’t belong here)
    }
}
````

### 🚩 Problems

* **Mixed responsibilities**: validation, communication, logging.
* Violates **Single Responsibility Principle (SRP)**.
* Hard to maintain → every developer keeps adding more random methods.

---

## ✅ Better Approach

Split responsibilities into **focused classes**:

```java
class StringValidator {
    public static boolean isNullOrEmpty(String s) {
        return s == null || s.isEmpty();
    }
}

class EmailService {
    public void sendEmail(String email, String msg) {
        // email logic
    }
}

class Logger {
    public void logToFile(String data) {
        // logging logic
    }
}
```

* Each class has **one clear responsibility**.
* Easier to extend, test, and maintain.

---

## 🎯 Why Utility Folders are Bad

1. Become **dumping grounds** for random code.
2. Contain **unrelated responsibilities**.
3. Lead to **tight coupling** and code scattered everywhere.
4. Difficult to test in isolation.

---

## 📝 Best Practices

* Don’t create a `utils` folder as a default.
* Group logic by **domain** (e.g., `UserService`, `PaymentValidator`).
* Apply **SRP** → one class, one responsibility.
* If you need reusable code, consider **design patterns** (e.g., Strategy, Factory).

---

## 🎯 Interview Q\&A

**Q1. What is a Utility Folder?**

**A1.** A collection of **unrelated static helper methods** that developers put together when they don’t know where to place them.

---

**Q2. Why is it considered a bad practice?**

**A2.** Because it **violates SRP**, mixes unrelated responsibilities, and becomes unmaintainable over time.

---

**Q3. What’s a better alternative to a utility folder?**

**A3.** Create **domain-specific classes** (e.g., `EmailService`, `PaymentValidator`) instead of dumping everything in `Utils`.

---

**Q4. When is a utility class acceptable?**

**A4.** Only for **highly generic, domain-agnostic functions** (like string trimming, date parsing) — but still keep them minimal and cohesive.

---

**Q5. How do you refactor an existing utility folder?**

**A5.**

* Identify methods with **different concerns**
* Move them into **appropriate domain classes**
* Leave only truly **generic helpers** in utility

---

