# 🔌 Adapter Pattern

## 📖 Category
**Structural Design Pattern**

---

## 🎯 Intent
Convert the **interface of a class** into another interface that clients expect.  
The Adapter lets classes with **incompatible interfaces** work together.

---

## ⚙️ Problem
You have existing code (a class or library) that you can’t modify,  
but it doesn’t match the interface your application expects.

Example:
- You have a class that provides data in XML format,  
  but your new system expects JSON.  
- You can’t change the XML class — it’s from a third-party library.

---

## 💡 Solution
- Create an **Adapter class** that wraps the existing class.  
- Implement the interface expected by the client.  
- Inside, the Adapter **translates calls** to the wrapped class.

---

## 🧩 Implementation in C++
```cpp
#include <iostream>
#include <string>

// ----- Target Interface -----
class ITarget {
public:
    virtual std::string Request() const = 0;
    virtual ~ITarget() = default;
};

// ----- Adaptee (Incompatible Interface) -----
class LegacyPrinter {
public:
    std::string OldRequest() const {
        return "LegacyPrinter: Printing using old system.";
    }
};

// ----- Adapter -----
class PrinterAdapter : public ITarget {
private:
    LegacyPrinter* legacyPrinter;
public:
    PrinterAdapter(LegacyPrinter* printer) : legacyPrinter(printer) {}
    std::string Request() const override {
        // Translate new interface to old method
        return "Adapter: (Translated) " + legacyPrinter->OldRequest();
    }
};

// ----- Client Code -----
void ClientCode(const ITarget& target) {
    std::cout << target.Request() << "\n";
}

int main() {
    LegacyPrinter* oldPrinter = new LegacyPrinter();
    PrinterAdapter adapter(oldPrinter);

    std::cout << "Client works with Adapter instead of old class:\n";
    ClientCode(adapter);

    delete oldPrinter;
    return 0;
}
```

## 🧠 When to Use
- You want to reuse an existing class that doesn't match the required interface
- You need to make incompatible interfaces cooperate
- You want to integrate legacy code without modifying it

## ✅ Pros
- Increases code reusability and compatibility
- Allows integration with legacy or third-party code
- Promotes Single Responsibility Principle — converts interface separately

## ❌ Cons
- Adds an extra layer of complexity and indirection
- Can make the code harder to trace or debug if overused

## 🌍 Real-World Analogy
A power adapter converts a plug type (e.g., EU to US) so your device can work with a different socket standard.

## 📘 Related Patterns
- **Bridge** – separates abstraction from implementation, while Adapter makes them compatible
- **Decorator** – adds behavior without changing the interface
- **Facade** – defines a simpler interface, while Adapter makes existing ones compatible