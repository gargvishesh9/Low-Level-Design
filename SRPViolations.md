# 🚩 Identifying SRP Violations

---

## 📖 What is SRP?
- **Single Responsibility Principle (SRP)**:  
  *A class should have only one reason to change.*  
- If a class handles multiple concerns, it becomes **fragile** and **hard to maintain**.

---

## ❌ Example of SRP Violation
```java
class Employee {
    void calculateSalary() {
        // business logic
    }

    void saveToDatabase() {
        // database logic
    }

    void generateReport() {
        // report generation logic
    }
}
```

### 🚩 Problems:
- **Multiple responsibilities**: salary calculation, DB operations, reporting.  
- Change in any responsibility forces modification in the same class.  
- Violates **SRP** and increases coupling.  

---

## ✅ Correct Approach (Separation of Concerns)
```java
class Employee {
    void calculateSalary() { /* business logic */ }
}

class EmployeeRepository {
    void save(Employee emp) { /* DB logic */ }
}

class ReportGenerator {
    void generate(Employee emp) { /* reporting logic */ }
}
```

- Now each class has **one clear responsibility**.  
- Follows **SRP**.  

---

## 🎯 How to Identify SRP Violations
1. **Monster Classes** – class doing too many things.  
2. **Utility Classes** – holding unrelated static methods.  
3. **Classes with multiple reasons to change** – e.g., logic, persistence, UI.  

---

## 🎯 Interview Q&A

**Q1. How do you identify an SRP violation?**  
**A1.** If a class has **more than one reason to change** (e.g., handling business logic + persistence), it violates SRP.  

---

**Q2. Why is SRP important?**  
**A2.** It reduces coupling, increases maintainability, and makes code more testable.  

---

**Q3. Give a real-world example of SRP violation.**  
**A3.** A `User` class that manages login, payment processing, and email notifications all together.  

---

**Q4. What problems arise when SRP is violated?**  
**A4.**  
- Harder to test  
- Frequent unintended side effects  
- Increased code complexity  
- Poor scalability  

---

**Q5. How do you fix SRP violations?**  
**A5.**  
- Break down the class into **smaller, cohesive classes**.  
- Each class should focus on **one responsibility only**.  

---
