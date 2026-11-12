# MSOOP — OOP + C/C++/Java (Microsoft)

> Weightage: ~8–10%. The single most-reported fundamental. "4 pillars" appears in report after report.
> They also ask "differences between C, C++, Java", and some rounds expect you to write the DSA solution as clean classes.

## 1. The 4 pillars of OOP, and the difference between them
**Answer:**
- **Encapsulation:** bundle data with the methods that change it, and hide the internals (private fields, public methods).
- **Abstraction:** expose *what* something does and hide *how* (interfaces, abstract classes).
- **Inheritance:** a class reuses and extends another ("is-a").
- **Polymorphism:** one interface, many behaviors.

**Analogy (a car):** Encapsulation = the engine is under a locked hood. Abstraction = you only see the steering wheel and pedals. Inheritance = an ElectricCar is a Car. Polymorphism = `accelerate()` burns petrol in one car and drains a battery in another.

**The trap they love:** "Encapsulation vs abstraction, aren't they the same?" No. Encapsulation is *hiding data* (an implementation-level mechanism). Abstraction is *hiding complexity* (a design-level idea). You can encapsulate without abstracting.

## 2. Compile-time vs run-time polymorphism
- **Compile-time:** overloading (same name, different parameters). Resolved by the compiler.
- **Run-time:** overriding through virtual functions. Resolved at runtime by the object's actual type.

## 3. How do virtual functions actually work? (separates people who memorized from people who understand)
**Answer:** Each class with virtual functions gets a **vtable** (a table of function pointers). Each object holds a hidden **vptr** pointing to its class's vtable. A call like `base->speak()` follows vptr -> vtable -> the right function.
**Follow-ups:**
- Cost: one extra pointer per object plus one indirect call (which also blocks inlining).
- **Why should a base class destructor be virtual?** If you `delete` a derived object through a base pointer with a non-virtual destructor, only the base destructor runs. Derived resources leak (it's undefined behavior).
- Can a constructor be virtual? No. The vptr isn't set up until the constructor runs.

## 4. Abstract class vs interface
- **Abstract class:** can have state, constructors, and implemented methods. Use it for shared *code* in an "is-a" hierarchy.
- **Interface:** a pure contract (Java/C# allow multiple; in C++ it's a class with only pure virtual functions). Use it for a shared *capability* ("can-do": Comparable, Serializable).
- **Rule of thumb:** inherit an implementation once, implement many interfaces.

## 5. Overloading vs overriding
Overloading = same name, different signature, same class, compile time. Overriding = same signature, subclass, runtime, needs `virtual` in C++.

## 6. Composition vs inheritance
**Answer:** Prefer composition ("has-a"). Inheritance couples the child tightly to the parent's internals; change the parent and children break (the "fragile base class" problem).
**Example:** A Car *has* an Engine (composition), so swapping in an ElectricEngine is easy. If Car *is* a PetrolEngine, you're stuck.

## 7. Diamond problem
B and C both inherit from A, and D inherits from both. Which copy of A does D get? C++ fixes this with `virtual` inheritance. Java avoids it by disallowing multiple class inheritance (interfaces only).

## 8. C vs C++ vs Java (asked in Dec 2025 report)
| | C | C++ | Java |
|---|---|---|---|
| Paradigm | Procedural | Multi (procedural + OOP + generic) | OOP-first |
| Memory | Manual (malloc/free) | Manual + RAII/smart pointers | Garbage collected |
| Runs on | Native machine code | Native machine code | Bytecode on the JVM (JIT) |
| Pointers | Yes | Yes + references | No raw pointers |
| Multiple inheritance | N/A | Yes | Interfaces only |
**Concept worth saying:** **RAII** in C++. Tie a resource's lifetime to an object, and the destructor frees it automatically. That's how `lock_guard` and `unique_ptr` work.

## 9. Stack vs heap memory
Stack: local variables, automatic, fast, limited size, freed when the function returns. Heap: `new`/`malloc`, lives until freed (or until GC), slower, can fragment. Stack overflow = deep recursion. Memory leak = heap allocation that never gets freed.

## 10. Shallow vs deep copy
Shallow copy duplicates the pointer, so both objects point to the same memory, and double-free bugs follow. Deep copy duplicates what the pointer points to. Rule of 3/5 in C++: if you write one of destructor / copy constructor / copy assignment, you probably need all of them.

## 11. SOLID (one line each, plus an example)
- **S**ingle responsibility: a class has one reason to change.
- **O**pen/closed: extend by adding code, not by editing it.
- **L**iskov substitution: a subclass must work anywhere its parent does (Square-extends-Rectangle breaks this).
- **I**nterface segregation: many small interfaces beat one fat one.
- **D**ependency inversion: depend on interfaces, not concrete classes.

## 12. Thread-safe Singleton
```cpp
class Logger {
public:
    static Logger& get() { static Logger inst; return inst; } // C++11: init is thread-safe
private:
    Logger() {}
    Logger(const Logger&) = delete;
    Logger& operator=(const Logger&) = delete;
};
```
Mention double-checked locking only to say the C++11 static-local version replaces it.
**Downside to mention:** singletons are hidden global state, which makes testing hard.

## 13. "Explain OOP to someone non-technical" (reported, SDE1 round 4)
"Think of a blueprint for a house. The blueprint is the class; each actual house built from it is an object. Every house has its own wiring hidden behind walls (encapsulation), you just use the switches (abstraction). A villa design can reuse the basic house plan and add a pool (inheritance). And 'open the door' means a key for one house and a fingerprint for another (polymorphism)."
