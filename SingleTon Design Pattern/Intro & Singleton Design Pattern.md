
# Singleton Design Pattern (Complete Guide)

---

## 1. Introduction
The **Singleton Pattern** is a **Creational Design Pattern**.  
Its intent (as defined by the Gang of Four) is:  

> *"Ensure a class has only one instance and provide a global point of access to it."*

It is one of the most popular and most frequently asked design patterns in **Low-Level Design (LLD) interviews**, especially for Java developers.

---

## 2. Motivation
Why do we need Singleton?  

- Imagine if your application **connects to a database**.  
  - If every part of the code creates a new connection object, memory and performance will degrade.  
  - Instead, we want **one object** that manages all DB connections.  

- Similarly, in a **logging service**, multiple objects would create chaos. A single logger ensures all logs are written consistently.  

### 🔑 Core Idea
- Restrict object creation.  
- Give **controlled global access**.  
- Maintain **consistency** across the system.  

---

## 3. Real-Life Analogies
- **Prime Minister of a Country** → Only one at a time, represents a single point of decision-making.  
- **Task Manager in Windows** → You cannot open multiple instances.  
- **Printer Spooler** → Only one spooler manages all print jobs.  

---

## 4. Real-World Software Examples
- **Spring Framework – ApplicationContext** → Only one context manages beans.  
- **Hibernate – SessionFactory** → Single factory instance manages sessions.  
- **JDBC Connection Pool** → Only one pool object controls all DB connections.  
- **Logger (Log4j, SLF4J)** → Single logger instance across the app.  

---

## 5. Rules of Singleton
1. Class must have **only one instance**.  
2. Provide a **global access point**.  
3. Make **constructor private** to prevent outside instantiation.  

---

## 6. Implementations

### (a) Early Initialization (Eager Singleton)
```java
public class Singleton {
    private static final Singleton instance = new Singleton();

    private Singleton() {}  // private constructor

    public static Singleton getInstance() {
        return instance;
    }
}
````

* ✅ Thread-safe (class loading guarantees single object).
* ✅ Simple to implement.
* ❌ Instance created even if never used (memory waste).

---

### (b) Lazy Initialization (Synchronized Method)

```java
public class Singleton {
    private static Singleton instance;

    private Singleton() {}

    public static synchronized Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

* ✅ Instance created only when needed.
* ✅ Thread-safe.
* ❌ Synchronization overhead → performance hit in multi-threaded systems.

---

### (c) Double-Checked Locking

```java
public class Singleton {
    private static volatile Singleton instance;

    private Singleton() {}

    public static Singleton getInstance() {
        if (instance == null) {  // First check
            synchronized (Singleton.class) {
                if (instance == null) {  // Second check
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

* ✅ Thread-safe.
* ✅ More efficient than full synchronization.
* ✅ `volatile` ensures correct object creation across threads.
* ❌ Slightly more complex.

---

### (d) Bill Pugh Singleton (Inner Static Class)

```java
public class Singleton {
    private Singleton() {}

    private static class Holder {
        private static final Singleton INSTANCE = new Singleton();
    }

    public static Singleton getInstance() {
        return Holder.INSTANCE;
    }
}
```

* ✅ Thread-safe.
* ✅ Lazy initialization without synchronization.
* ✅ Clean and efficient.
* ❌ Less well-known, so sometimes surprises junior developers.

---

### (e) Enum Singleton (Best & Simplest)

```java
public enum Singleton {
    INSTANCE;

    public void showMessage() {
        System.out.println("Enum Singleton Working!");
    }
}
```

* ✅ Thread-safe.
* ✅ Protects against reflection, serialization, cloning.
* ✅ Simplest implementation in Java.
* ❌ Cannot extend other classes (enums can’t extend).

---

## 7. Pitfalls of Singleton

### Breaking Singleton

* **Reflection** can bypass private constructor.
* **Serialization** can create new instances.
* **Cloning** can copy the object.

### Fixes

* Override `readResolve()` in Serializable Singleton:

  ```java
  protected Object readResolve() {
      return getInstance();
  }
  ```
* Throw exception in `clone()`:

  ```java
  @Override
  protected Object clone() throws CloneNotSupportedException {
      throw new CloneNotSupportedException();
  }
  ```

---

## 8. Comparison of Implementations

| Approach               | Thread-Safe | Lazy? | Performance | Notes                  |
| ---------------------- | ----------- | ----- | ----------- | ---------------------- |
| Early Initialization   | ✅           | ❌     | Fast        | Memory waste if unused |
| Lazy + Synchronized    | ✅           | ✅     | Slow        | Simple but inefficient |
| Double-Checked Locking | ✅           | ✅     | Good        | Recommended            |
| Bill Pugh Singleton    | ✅           | ✅     | Excellent   | Clean + Modern         |
| Enum Singleton         | ✅           | ✅     | Excellent   | Best for most cases    |

---

## 9. When to Use Singleton

* Logging frameworks (SLF4J, Log4j).
* Database connection pools.
* Caching frameworks.
* Configuration managers.
* Thread pools / Executors.

❌ Avoid Singleton for **business logic classes** → leads to global state (anti-pattern).

---

## 10. Common Interview Traps

* **Q: Is Singleton just a global variable?**
  → No. Global variables don’t control instantiation. Singleton enforces one instance.

* **Q: How to make Singleton thread-safe?**
  → Double-checked locking, Bill Pugh, or Enum.

* **Q: How do you prevent reflection from breaking Singleton?**
  → Throw an exception in the constructor if an instance already exists.

* **Q: Is Singleton an anti-pattern?**
  → Overuse leads to global state (hard to test, tightly coupled code). Use only when logically one instance must exist.

---

## 11. Interview Questions

### Basics

1. What is Singleton Pattern?
2. Why is it needed?
3. Give real-world examples.

### Implementation

4. How to implement Singleton in Java?
5. Difference between **eager** and **lazy** initialization?
6. Explain **double-checked locking** and role of `volatile`.
7. What is Bill Pugh Singleton?

### Advanced

8. How can reflection break Singleton? How to prevent it?
9. How to prevent Singleton from breaking via serialization/cloning?
10. What is Enum Singleton? Why is it best?
11. Is Singleton a design pattern or an anti-pattern?
12. Which Singleton approach do you use in production and why?

---

## 12. Summary

* Singleton ensures **one instance per JVM**.
* Several implementations exist:

  * Eager, Lazy, Double-Checked Locking, Bill Pugh, Enum.
* Best modern approaches:

  * **Bill Pugh** (clean, lazy, thread-safe).
  * **Enum** (safest, prevents reflection/serialization issues).
* Use Singleton wisely. Overuse may cause **tight coupling + testing difficulties**.

---

```


