# 🧩 Abstract Factory Pattern

## 📖 Category
**Creational Design Pattern**

---

## 🎯 Intent
Provide an **interface for creating families of related or dependent objects**  
without specifying their concrete classes.

---

## ⚙️ Problem
Imagine you’re building a **cross-platform UI library** that supports multiple operating systems:  
Windows, macOS, and Linux.  
Each platform has its own style of buttons, checkboxes, and windows.

If you instantiate them directly with `new WindowsButton()` or `new MacButton()`,  
your code becomes tightly coupled to specific classes — difficult to extend or maintain.

---

## 💡 Solution
- Define **abstract interfaces** for products (e.g., `Button`, `Checkbox`).  
- Create an **Abstract Factory** interface that declares creation methods for each product type.  
- Implement **Concrete Factories** that produce families of products that work together.  
- The client code uses only the **abstract interfaces**.

---

## 🧩 Implementation in C++
```cpp
#include <iostream>
#include <memory>

// ----- Abstract Products -----
class Button {
public:
    virtual void Paint() = 0;
    virtual ~Button() = default;
};

class Checkbox {
public:
    virtual void Check() = 0;
    virtual ~Checkbox() = default;
};

// ----- Concrete Products (Windows) -----
class WindowsButton : public Button {
public:
    void Paint() override {
        std::cout << "Rendering a Windows-style Button\n";
    }
};

class WindowsCheckbox : public Checkbox {
public:
    void Check() override {
        std::cout << "Checking a Windows-style Checkbox\n";
    }
};

// ----- Concrete Products (Mac) -----
class MacButton : public Button {
public:
    void Paint() override {
        std::cout << "Rendering a Mac-style Button\n";
    }
};

class MacCheckbox : public Checkbox {
public:
    void Check() override {
        std::cout << "Checking a Mac-style Checkbox\n";
    }
};

// ----- Abstract Factory -----
class GUIFactory {
public:
    virtual std::unique_ptr<Button> CreateButton() = 0;
    virtual std::unique_ptr<Checkbox> CreateCheckbox() = 0;
    virtual ~GUIFactory() = default;
};

// ----- Concrete Factories -----
class WindowsFactory : public GUIFactory {
public:
    std::unique_ptr<Button> CreateButton() override {
        return std::make_unique<WindowsButton>();
    }
    std::unique_ptr<Checkbox> CreateCheckbox() override {
        return std::make_unique<WindowsCheckbox>();
    }
};

class MacFactory : public GUIFactory {
public:
    std::unique_ptr<Button> CreateButton() override {
        return std::make_unique<MacButton>();
    }
    std::unique_ptr<Checkbox> CreateCheckbox() override {
        return std::make_unique<MacCheckbox>();
    }
};

// ----- Client Code -----
void BuildUI(std::unique_ptr<GUIFactory> factory) {
    auto button = factory->CreateButton();
    auto checkbox = factory->CreateCheckbox();

    button->Paint();
    checkbox->Check();
}

int main() {
    std::cout << "Windows UI:\n";
    BuildUI(std::make_unique<WindowsFactory>());

    std::cout << "\nMac UI:\n";
    BuildUI(std::make_unique<MacFactory>());

    return 0;
}
```

## 🧠 When to Use
- The system must be independent of how its products are created
- You need to ensure consistency among related products
- The system should be easily extendable with new product families

## ✅ Pros
- Ensures compatibility among created products
- Encourages the Open/Closed Principle — add new families without changing existing code
- Centralizes object creation logic

## ❌ Cons
- Adding a new product type (e.g., Slider) requires modifying all factories
- More complex structure than simple factories

## 🌍 Real-World Analogy
A furniture company produces consistent sets of products — modern chairs, modern sofas, and modern tables. Another factory produces Victorian-style furniture — the same product types but with a different design.

## 📘 Related Patterns
- **Factory Method** – often used to implement each product creation method
- **Builder** – focuses on step-by-step object construction instead of families
- **Prototype** – creates copies of existing products instead of new instances