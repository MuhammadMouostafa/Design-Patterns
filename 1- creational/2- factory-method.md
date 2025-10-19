# 🏭 Factory Method Pattern

## 📖 Category
**Creational Design Pattern**

---

## 🎯 Intent
Define an **interface for creating an object**, but let **subclasses decide which class to instantiate**.  
The Factory Method lets a class **defer instantiation to subclasses**.

---

## ⚙️ Problem
Sometimes a class cannot anticipate the type of objects it needs to create.  
For example:
- A drawing app needs to create different shapes (Circle, Rectangle, etc.).
- A game needs to spawn different types of enemies.

If you use `new` directly inside your main code, you must modify it each time a new class is added — violating the **Open/Closed Principle**.

---

## 💡 Solution
- Create a **factory method** in a base class that returns an abstract product.  
- Subclasses **override** this method to create specific product objects.  
- The client uses the factory method instead of calling `new` directly.

---

## 🧩 Implementation in C++
```cpp
#include <iostream>
#include <memory>

// ----- Product Interface -----
class Shape {
public:
    virtual void Draw() = 0;
    virtual ~Shape() = default;
};

// ----- Concrete Products -----
class Circle : public Shape {
public:
    void Draw() override {
        std::cout << "Drawing a Circle\n";
    }
};

class Rectangle : public Shape {
public:
    void Draw() override {
        std::cout << "Drawing a Rectangle\n";
    }
};

// ----- Creator (Factory) -----
class ShapeFactory {
public:
    virtual std::unique_ptr<Shape> CreateShape() = 0;
    virtual ~ShapeFactory() = default;
};

// ----- Concrete Factories -----
class CircleFactory : public ShapeFactory {
public:
    std::unique_ptr<Shape> CreateShape() override {
        return std::make_unique<Circle>();
    }
};

class RectangleFactory : public ShapeFactory {
public:
    std::unique_ptr<Shape> CreateShape() override {
        return std::make_unique<Rectangle>();
    }
};

// ----- Client Code -----
int main() {
    std::unique_ptr<ShapeFactory> factory;

    factory = std::make_unique<CircleFactory>();
    auto circle = factory->CreateShape();
    circle->Draw();

    factory = std::make_unique<RectangleFactory>();
    auto rectangle = factory->CreateShape();
    rectangle->Draw();

    return 0;
}
```

## 🧠 When to Use
- When the class doesn't know which type of object it must create
- When you want to delegate object creation to subclasses
- When adding new product types frequently

## ✅ Pros
- Follows the Open/Closed Principle — new products can be added without modifying existing code
- Centralizes object creation logic
- Promotes loose coupling between creator and products

## ❌ Cons
- Increases the number of classes in the project
- Slightly more complex code structure

## 🌍 Real-World Analogy
A document editor that supports different file types (.docx, .pdf, .txt). Each format has its own "factory" that knows how to create and handle that file type.

## 📘 Related Patterns
- **Abstract Factory** – uses multiple factory methods to create related objects
- **Prototype** – creates objects by copying an existing instance instead of instantiating new ones
- **Singleton** – sometimes used for a single shared factory instance