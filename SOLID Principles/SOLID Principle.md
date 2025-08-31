# 🏗️ SOLID Principles – Introduction

---

## 📖 What are SOLID Principles?
- **SOLID** is an acronym representing **five key design principles** in object-oriented programming.  
- Introduced by **Robert C. Martin (Uncle Bob)** to make software:
  - More maintainable  
  - Easier to extend  
  - Flexible and reusable  
  - Less prone to bugs  

---

## 🔑 The 5 Principles
1. **S → Single Responsibility Principle (SRP)**  
   - A class should have **only one reason to change**.  

2. **O → Open/Closed Principle (OCP)**  
   - Software entities should be **open for extension but closed for modification**.  

3. **L → Liskov Substitution Principle (LSP)**  
   - Subtypes must be substitutable for their base types.  

4. **I → Interface Segregation Principle (ISP)**  
   - Clients should not be forced to depend on interfaces they do not use.  

5. **D → Dependency Inversion Principle (DIP)**  
   - High-level modules should not depend on low-level modules. Both should depend on abstractions.  

---

## 🎯 Why SOLID?
- Avoids **spaghetti code**  
- Improves **readability and maintainability**  
- Makes refactoring easier  
- Supports **scalability** in long-term projects  

---

## 📝 Example Analogy
Imagine building a house:
- If all rooms share one **main switch**, changing one feature affects everything (bad design).  
- If each room has its **own switch**, changes are isolated (good design).  

---

## 🎯 Interview Q&A

**Q1. What does SOLID stand for?**  
**A1.**  
- S → Single Responsibility Principle  
- O → Open/Closed Principle  
- L → Liskov Substitution Principle  
- I → Interface Segregation Principle  
- D → Dependency Inversion Principle  

---

**Q2. Why are SOLID principles important in real-world projects?**  
**A2.** They make code **flexible, testable, reusable, and maintainable**. Without SOLID, projects become hard to scale and refactor.  

---

**Q3. Can you give an example where violating SOLID caused issues?**  
**A3.** Example: If a single class handles **database operations, UI rendering, and business logic**, changing one part may break another. This violates SRP and makes code fragile.  

---

**Q4. How do SOLID principles help in writing testable code?**  
**A4.** By separating responsibilities and depending on abstractions, we can write **unit tests easily** with mock dependencies.  

---

**Q5. Which SOLID principle relates to “open for extension, closed for modification”?**  
**A5.** The **Open/Closed Principle (OCP)**.  

---
