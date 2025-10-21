# 🧩 Strategy Pattern (Behavioral)

## 📖 Category
**Behavioral Design Pattern**

---

## 🎯 Intent
Define a family of algorithms, encapsulate each one, and make them **interchangeable**.  
The Strategy pattern lets the algorithm vary independently from the clients that use it.

---

## ⚙️ Problem
Sometimes, an object needs to perform a specific operation, but the **exact behavior or algorithm** may vary depending on the situation.

If you handle this using conditionals (`if`/`switch`):
- The code becomes **hard to maintain** and **violates Open/Closed Principle**.  
- Adding or changing algorithms requires modifying existing code.  
- The logic of algorithm selection gets mixed with business logic.

Example:  
A payment system supports multiple methods — **Credit Card**, **PayPal**, **Crypto**.  
Hardcoding conditions inside a single `pay()` method makes it rigid and messy.

---

## 💡 Solution
- Define a **Strategy interface** that declares a common algorithm method.  
- Create multiple **Concrete Strategies**, each implementing a specific algorithm.  
- The **Context** class keeps a reference to a Strategy object and delegates work to it.  
- The client can change the algorithm (Strategy) **at runtime**.

This allows algorithms to vary independently of the rest of the system.

---

## 🧩 Implementation in C++
```cpp
#include <iostream>
#include <memory>
#include <string>
using namespace std;

// ----- Strategy Interface -----
class PaymentStrategy {
public:
    virtual void pay(float amount) = 0;
    virtual ~PaymentStrategy() = default;
};

// ----- Concrete Strategies -----
class CreditCardPayment : public PaymentStrategy {
public:
    void pay(float amount) override {
        cout << "💳 Paying " << amount << " using Credit Card.\n";
    }
};

class PayPalPayment : public PaymentStrategy {
public:
    void pay(float amount) override {
        cout << "💻 Paying " << amount << " using PayPal.\n";
    }
};

class CryptoPayment : public PaymentStrategy {
public:
    void pay(float amount) override {
        cout << "🪙 Paying " << amount << " using Cryptocurrency.\n";
    }
};

// ----- Context -----
class PaymentContext {
    unique_ptr<PaymentStrategy> strategy;
public:
    void setStrategy(unique_ptr<PaymentStrategy> newStrategy) {
        strategy = move(newStrategy);
    }
    void checkout(float amount) {
        if (strategy)
            strategy->pay(amount);
        else
            cout << "❌ No payment strategy selected.\n";
    }
};

// ----- Client Code -----
int main() {
    PaymentContext context;

    context.setStrategy(make_unique<CreditCardPayment>());
    context.checkout(100);

    context.setStrategy(make_unique<PayPalPayment>());
    context.checkout(200);

    context.setStrategy(make_unique<CryptoPayment>());
    context.checkout(300);
}
```

## 🧠 When to Use
- When you have multiple algorithms for a specific task
- When you want to avoid conditional logic for algorithm selection
- When you need to swap behaviors dynamically at runtime
- When different variants of an operation should be easily interchangeable

## ✅ Pros
- Promotes Open/Closed Principle — add new algorithms without changing existing code
- Simplifies maintenance by separating algorithm logic from business logic
- Allows runtime flexibility — the algorithm can change dynamically
- Encourages composition over inheritance

## ❌ Cons
- Increases the number of classes (one per strategy)
- The client must be aware of different strategies to choose appropriately
- Context and Strategy interaction adds a small level of indirection

## 🌍 Real-World Analogy
- Think of a navigation app:

- You can choose between driving, walking, or cycling routes.

- Each route uses a different algorithm to calculate directions, but the app's interface stays the same — you just pick a strategy.

## 📘 Related Patterns
- **State** – similar structure; however, State changes automatically based on internal conditions, while Strategy is chosen explicitly by the client
- **Factory Method** – can be used to create the right Strategy object
- **Decorator** – can wrap a strategy to add extra behavior dynamically