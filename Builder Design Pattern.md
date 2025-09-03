

## Goal / problem statement

You need to create objects of a class (`Student`) that has **many attributes** (some mandatory, many optional).
Requirements:

* Attributes should be **immutable** (once created, object state can't change).
* Construction should be **readable, safe, and maintainable**.
* Avoid constructor explosion (telescoping) and runtime errors from ad-hoc maps.

Typical fields (from example): `firstName`, `lastName`, `age`, `weight`, `college`, `salary`, …

---

## 1) Attributes are immutable — why it matters

Immutable objects are easier to reason about and thread-safe by default.

To make fields immutable in Java:

* Mark fields `private final`.
* Initialize them only once (via constructor or builder).
* Provide only getter methods (no setters).

Example:

```java
public final class Student {
    private final String firstName;
    private final String lastName;
    private final Integer age;
    private final Integer weight;
    private final String college;
    private final Double salary;

    // constructor-only initialization; no setters
}
```

**Benefits**: predictable state, thread-safety, safer equals/hashCode, easier caching.

**Problem**: How to construct objects with many optional parameters without ugly constructors?

---

## 2) Issues with mutable setters (bad starting point)

A common first approach: create class with default constructor + setters.

```java
public class StudentMutable {
    private String firstName;
    private String lastName;
    private Integer age;
    private Integer weight;
    private String college;
    private Double salary;

    public StudentMutable() {}

    public void setFirstName(String f) { this.firstName = f; }
    public void setLastName(String l) { this.lastName = l; }
    // ... other setters
}
```

**Drawbacks**

* Object is mutable — state can change after creation.
* No guarantee all required fields set before use → invalid state.
* Not thread-safe.
* Verbose client code:

  ```java
  StudentMutable s = new StudentMutable();
  s.setFirstName("Subhash");
  s.setAge(24);
  // maybe forgot weight or set wrong field name
  ```

**Conclusion**: mutable setters violate immutability and cause bugs.

---

## 3) Parameterized constructor (single full ctor)

Make all fields final and require them via one full constructor:

```java
public class Student {
    private final String firstName;
    private final String lastName;
    private final Integer age;
    private final Integer weight;
    private final String college;
    private final Double salary;

    public Student(String firstName, String lastName,
                   Integer age, Integer weight,
                   String college, Double salary) {
        this.firstName = firstName;
        this.lastName  = lastName;
        this.age       = age;
        this.weight    = weight;
        this.college   = college;
        this.salary    = salary;
    }
}
```

**Drawbacks**

* For many optional fields, constructor signature becomes huge and error-prone.
* Hard to remember parameter order; code readability suffers.
* Passing nulls for optional fields is messy.

---

## 4) Telescoping constructors (attempted improvement)

Create multiple constructors for different combinations:

```java
public Student(String firstName, Integer age) { ... }
public Student(String firstName, Integer age, Integer weight) { ... }
public Student(String firstName, String lastName, Integer age, Integer weight, String college) { ... }
// ... many variants
```

**Problems**

* Combinatorial explosion of constructors — maintenance nightmare.
* Duplicate code and confusing overloads.
* Clients still struggle to pick the right constructor.

This is the classic *telescoping constructor* problem described in your notes.&#x20;

---

## 5) Single constructor + data structure approach (Map) — why it’s tempting but dangerous

Some try to pass a `Map<String, Object>` or a helper `Map` to a single constructor to allow arbitrary keys:

```java
Map<String,Object> values = new HashMap<>();
values.put("firstName", "Subhash");
values.put("age", 24);
Student s = new Student(values);
```

**Drawbacks**

* Loses compile-time type safety — runtime casting exceptions possible.
* Typos in key strings (e.g., `"fname"` vs `"firstName"`) produce runtime errors — exactly the kind of bug shown in your notes where a key mismatch caused issues.&#x20;
* Hard to document and validate required fields.

Conclusion: flexible but unsafe.

---

## 6) Helper class approach (intermediate) — from the PDF

Your notes suggest a `Helper` (or DTO) with mutable setters, and a `Student` that accepts the helper:

```java
public class StudentHelper {
    public String firstName;
    public Integer age;
    public Integer weight;
    public String college;
    public Double salary;
    // builder-style setters or public fields used to populate
}

public class Student {
    private final String firstName;
    private final Integer age;
    // ...

    public Student(StudentHelper h) {
        this.firstName = h.firstName;
        this.age = h.age;
        this.weight = h.weight;
        // ...
    }
}
```

**Pros**

* Avoids huge constructor signatures.
* Centralizes population logic.

**Cons**

* Helper is mutable — still possible to create inconsistent helper state before passing.
* Not idiomatic fluent API; still some verbosity.
* You're left with two classes where the helper must be documented.

Your notes progressed to this helper-based solution before landing on Builder.&#x20;

---

## 7) Builder Pattern — concept & when to use

**Intent**: Separate the construction of a complex object from its representation. The Builder lets you construct objects step-by-step with readable code and produces immutable objects.

**When to use**

* Class with many parameters (especially optional ones).
* Fields should be immutable.
* You want a readable, fluent client API.
* Validation or final consistency checks are required during construction.

---

## 8) Implement BPD 1 — Basic Builder (simple, clear)

This is the straightforward static inner `Builder` approach.

### Student.java (basic builder)

```java
public final class Student {
    private final String firstName;
    private final String lastName;
    private final Integer age;
    private final Integer weight;
    private final String college;
    private final Double salary;

    private Student(Builder builder) {
        this.firstName = builder.firstName;
        this.lastName  = builder.lastName;
        this.age       = builder.age;
        this.weight    = builder.weight;
        this.college   = builder.college;
        this.salary    = builder.salary;
    }

    public static class Builder {
        // same fields but not final
        private String firstName;
        private String lastName;
        private Integer age;
        private Integer weight;
        private String college;
        private Double salary;

        public Builder() {}

        public Builder firstName(String firstName) {
            this.firstName = firstName;
            return this;
        }
        public Builder lastName(String lastName) {
            this.lastName = lastName;
            return this;
        }
        public Builder age(Integer age) {
            this.age = age;
            return this;
        }
        // ... other setter-like builder methods

        public Student build() {
            return new Student(this);
        }
    }

    // getters only (no setters)
}
```

### Usage

```java
Student s = new Student.Builder()
                    .firstName("Subhash")
                    .lastName("Kumar")
                    .age(24)
                    .weight(80)
                    .college("IIT")
                    .salary(7_40_000.0)
                    .build();
```

**Why this is good**

* Readable, labeled parameters (no confusion about order).
* Immutable `Student` object.
* Optional fields can be omitted easily.
* Compile-time type safety.

---

## 9) Implement BPD 2 — Final, production-ready Builder

A more robust implementation adds:

* Validation in `build()` (required fields check).
* Default values for optional fields.
* Defensive copying for mutable collections.
* `toBuilder()` for creating modified copies.
* Make `Builder` support chaining nicely.
* Make `Student` `Serializable` safely or support `readResolve()` if needed.

### Student (final) — full example

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;
import java.util.Objects;

public final class Student {
    private final String firstName;
    private final String lastName;
    private final Integer age;
    private final Integer weight;
    private final String college;
    private final Double salary;
    private final List<String> skills; // example of a collection field

    private Student(Builder b) {
        this.firstName = b.firstName;
        this.lastName  = b.lastName;
        this.age       = b.age;
        this.weight    = b.weight;
        this.college   = b.college;
        this.salary    = b.salary;
        // defensive copy
        this.skills    = b.skills == null
                         ? Collections.emptyList()
                         : Collections.unmodifiableList(new ArrayList<>(b.skills));
    }

    public static Builder builder(String firstName) {
        return new Builder(firstName);
    }

    // getters only
    public String getFirstName() { return firstName; }
    public String getLastName()  { return lastName; }
    public Integer getAge()      { return age; }
    public Integer getWeight()   { return weight; }
    public String getCollege()   { return college; }
    public Double getSalary()    { return salary; }
    public List<String> getSkills() { return skills; }

    public Builder toBuilder() {
        Builder b = new Builder(this.firstName);
        b.lastName(this.lastName)
         .age(this.age)
         .weight(this.weight)
         .college(this.college)
         .salary(this.salary)
         .skills(new ArrayList<>(this.skills));
        return b;
    }

    public static final class Builder {
        // required field
        private final String firstName;

        // optional
        private String lastName;
        private Integer age;
        private Integer weight;
        private String college;
        private Double salary;
        private List<String> skills;

        public Builder(String firstName) {
            this.firstName = Objects.requireNonNull(firstName, "firstName is required");
        }

        public Builder lastName(String lastName) {
            this.lastName = lastName;
            return this;
        }
        public Builder age(Integer age) {
            this.age = age;
            return this;
        }
        public Builder weight(Integer weight) {
            this.weight = weight;
            return this;
        }
        public Builder college(String college) {
            this.college = college;
            return this;
        }
        public Builder salary(Double salary) {
            this.salary = salary;
            return this;
        }
        public Builder skills(List<String> skills) {
            this.skills = skills == null ? null : new ArrayList<>(skills);
            return this;
        }
        public Builder addSkill(String skill) {
            if (this.skills == null) this.skills = new ArrayList<>();
            this.skills.add(skill);
            return this;
        }

        public Student build() {
            // validations: example => age must be non-negative if provided
            if (age != null && age < 0) {
                throw new IllegalArgumentException("age must be non-negative");
            }
            // more validations can be added here
            return new Student(this);
        }
    }
}
```

### Usage

```java
Student s = Student.builder("Subhash")
                   .lastName("Kumar")
                   .age(24)
                   .weight(80)
                   .college("IIT")
                   .salary(740000.0)
                   .addSkill("Java")
                   .addSkill("Data Structures")
                   .build();
```

**Notes**

* Builder enforces required fields via constructor.
* `build()` centralizes validation and invariants.
* Object returned is immutable and safe to share.

---

## 10) Comparison — evolution summary

| Approach                        | Readability | Type-safety | Immutability |   Validation | Recommended                                   |
| ------------------------------- | ----------: | ----------: | -----------: | -----------: | :-------------------------------------------- |
| Mutable setters                 |         Low |        Good |           No |           No | **Not recommended**                           |
| Full constructor (many args)    |        Poor |        Good |          Yes |          Yes | Rarely — error-prone                          |
| Telescoping constructors        |        Poor |        Good |          Yes |      Limited | **Bad** for many optional fields              |
| Map-based                       |      Medium |      **No** |      Depends | Runtime only | **Unsafe**                                    |
| Helper DTO + ctor               |      Medium |        Good | Yes (target) |         Some | Intermediate                                  |
| Builder (basic)                 |        High |        Good |          Yes |          Yes | Good                                          |
| Builder (final with validation) |        High |        Good |          Yes | Yes (strong) | **Recommended** for complex immutable objects |

---

## 11) When to use Builder Pattern (concise)

* Class has **many parameters**, especially optional ones.
* You want **immutable** objects.
* You want **clear, self-documenting client code**.
* You need **validation** and final consistency checks at construction time.

Do **not** use Builder for tiny classes with 2–3 fields — overhead isn't worth it.

---

## 12) Do’s and Don’ts

**Do**

* Use Builder for complex construction or many optional fields.
* Keep the immutable target class `final` and fields `private final`.
* Validate required fields in `build()`.
* Use defensive copies for collections.

**Don’t**

* Use Map/String keys to approximate named parameters — this loses type-safety.
* Expose setters on the final immutable class.
* Overuse Builder for trivial beans.

---

## 13) Interview Questions (Builder pattern) — with suggested answers

**Q1. What problem does the Builder pattern solve?**
**A:** It solves construction of complex objects (many parameters, optional parameters) by providing a readable, step-by-step fluent API, producing immutable objects and centralizing validation.

**Q2. How does Builder compare to telescoping constructors?**
**A:** Telescoping constructors require many overloads for different parameter sets and become unmaintainable. Builder gives a single flexible API with named parameter methods, avoiding explosion.

**Q3. When would you prefer Factory over Builder?**
**A:** Factory is used when you want to decide which concrete class to instantiate (polymorphic creation). Builder is for constructing a single complex object step by step.

**Q4. How do you enforce required fields?**
**A:** Make required fields parameters of the Builder's constructor (or separate `builder(required1, required2)` factory method). Validate in `build()`.

**Q5. How do you design a Builder for inheritance?**
**A:** Use covariant return types and recursive generics (the *self type* trick) so subclasses' builder methods return the correct builder type. Or prefer composition over inheritance.

**Q6. How to make builder thread-safe?**
**A:** Builders are typically not thread-safe; they are used per-thread (per construction). If needed, synchronize or create fresh builders per thread.

**Q7. Why is final/immutable Student preferred?**
**A:** Final/immutable makes objects thread-safe, simpler to reason about, and less error-prone. Builders allow immutability while keeping construction readable.

---

## 14) Quick checklist / template to implement Builder in your repo

1. Create immutable target class with `private final` fields and private constructor receiving `Builder`.
2. Create a public static `Builder` inner class.
3. Provide fluent setter-like methods in `Builder` returning `this`.
4. Validate required fields and invariants in `build()`.
5. Return new target object from `build()`.
6. Add `toBuilder()` if object modification/copying is needed.
7. Defensive copy collections and return unmodifiable collections in getters.

---

## 15) Final words (practical tips)

* Use the Builder pattern when the class has many optional params and immutability is desired.
* For small POJOs, prefer constructors or static factory methods.
* In Java, prefer the **static inner class Builder** (Bill Pugh style), and add validation in `build()`.
* If you need absolute protection vs. reflection & serialization, consider additional safeguards (but Builder + immutable class already addresses most needs).

---
