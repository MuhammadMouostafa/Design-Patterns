# 🧠 Singleton Pattern

## 📖 Category
**Creational Design Pattern**

---

## 🎯 Intent
Ensure that a class has **only one instance** and provide a **global point of access** to it.

---

## ⚙️ Problem
In some cases, you need exactly one instance of a class — for example:
- Managing a **shared resource** (e.g., database connection, configuration).
- Logging system.
- Thread pool manager.

Creating multiple instances could lead to **inconsistency**, **high memory use**, or **resource conflicts**.

---

## 💡 Solution
- Make the class **responsible for its own instance creation**.
- Restrict external instantiation using a **private constructor**.
- Provide a **static method** (e.g., `GetInstance()`) to access the single instance.

---

## 🧩 Implementation in C++

```cpp
#include <iostream>

class Singleton {
private:
    static Singleton* instance;

    // Private constructor to prevent direct instantiation
    Singleton() {
        std::cout << "Singleton instance created\n";
    }

public:
    // Delete copy and move operations
    Singleton(const Singleton&) = delete;
    Singleton& operator=(const Singleton&) = delete;
    Singleton(Singleton&&) = delete;
    Singleton& operator=(Singleton&&) = delete;

    static Singleton* GetInstance() {
        if (instance == nullptr) {
            instance = new Singleton();
        }
        return instance;
    }

    void ShowMessage() {
        std::cout << "Hello from Singleton!\n";
    }
};

// Initialize static member
Singleton* Singleton::instance = nullptr;

// Example usage
int main() {
    Singleton* s1 = Singleton::GetInstance();
    Singleton* s2 = Singleton::GetInstance();

    s1->ShowMessage();
    std::cout << (s1 == s2 ? "Same instance\n" : "Different instances\n");
    return 0;
}
```
### ⚠️ Issue in Multithreaded Environments

If two threads call GetInstance() simultaneously while instance is nullptr,
they might create two different instances, breaking the Singleton rule.

## 🧱 Thread-Safe Implementation (Using std::mutex)

```cpp
#include <iostream>
#include <mutex>

class Singleton {
private:
    static Singleton* instance;
    static std::mutex mtx;

    // Private constructor prevents external instantiation
    Singleton() {
        std::cout << "Singleton instance created\n";
    }

public:
    // Delete copy and move operations to ensure a single instance
    Singleton(const Singleton&) = delete;
    Singleton& operator=(const Singleton&) = delete;
    Singleton(Singleton&&) = delete;
    Singleton& operator=(Singleton&&) = delete;

    static Singleton* GetInstance() {
        if (instance == nullptr) {
            std::lock_guard<std::mutex> lock(mtx);
            if (instance == nullptr)
                instance = new Singleton();
        }
        return instance;
    }

    void ShowMessage() {
        std::cout << "Hello from Singleton!\n";
    }
};

// Initialize static members
Singleton* Singleton::instance = nullptr;
std::mutex Singleton::mtx;

// Example usage
int main() {
    Singleton* s1 = Singleton::GetInstance();
    Singleton* s2 = Singleton::GetInstance();

    s1->ShowMessage();
    std::cout << (s1 == s2 ? "Same instance\n" : "Different instances\n");
    return 0;
}
```

## 🧠 When to Use
- When only one instance should exist
- When global access is required
- When controlling access to shared resources

## ✅ Pros
- Controlled access to the sole instance
- Reduces global variable clutter
- Lazy initialization (only created when needed)

## ❌ Cons
- Difficult to unit test (global state)
- Violates Single Responsibility Principle
- May cause issues in multithreaded environments if not implemented carefully

## 🌍 Real-World Analogy
A government: only one exists, and everyone refers to it to make official decisions.

## 📘 Related Patterns

- Factory Method – sometimes used inside Singleton to manage object creation.

- Facade – may use a Singleton to provide a single access point to subsystems.