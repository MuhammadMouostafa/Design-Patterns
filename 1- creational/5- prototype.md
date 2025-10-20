# 🧬 Prototype Pattern

## 📖 Category
**Creational Design Pattern**

---

## 🎯 Intent
Create new objects by **cloning existing ones**,  
instead of creating them from scratch using `new`.

---

## ⚙️ Problem
Sometimes object creation is **costly or complex**:
- The object has expensive initialization (e.g., database or file I/O).
- Many similar objects differ only slightly.
- The code shouldn’t depend on concrete classes for instantiation.

Using `new` for each instance can be inefficient or lead to tight coupling.

---

## 💡 Solution
- Define a **Prototype interface** with a `Clone()` method.  
- Each class implements `Clone()` to return a copy of itself.  
- The client uses existing instances as prototypes and clones them as needed.

---

## 🧩 Implementation in C++
```cpp
#include <iostream>
#include <memory>
#include <string>

// ----- Prototype Interface -----
class Shape {
public:
    virtual std::unique_ptr<Shape> Clone() const = 0;
    virtual void Draw() const = 0;
    virtual ~Shape() = default;
};

// ----- Concrete Prototypes -----
class Circle : public Shape {
private:
    int radius;
public:
    Circle(int r) : radius(r) {}
    std::unique_ptr<Shape> Clone() const override {
        return std::make_unique<Circle>(*this); // copy constructor
    }
    void Draw() const override {
        std::cout << "Drawing a Circle with radius " << radius << "\n";
    }
};

class Rectangle : public Shape {
private:
    int width, height;
public:
    Rectangle(int w, int h) : width(w), height(h) {}
    std::unique_ptr<Shape> Clone() const override {
        return std::make_unique<Rectangle>(*this);
    }
    void Draw() const override {
        std::cout << "Drawing a Rectangle " << width << "x" << height << "\n";
    }
};

// ----- Client Code -----
int main() {
    // Create original prototypes
    std::unique_ptr<Shape> circlePrototype = std::make_unique<Circle>(10);
    std::unique_ptr<Shape> rectPrototype = std::make_unique<Rectangle>(4, 8);

    // Clone them instead of creating new
    auto circle1 = circlePrototype->Clone();
    auto circle2 = circlePrototype->Clone();

    auto rect1 = rectPrototype->Clone();

    // Draw all
    circle1->Draw();
    circle2->Draw();
    rect1->Draw();

    return 0;
}
```

## 🧠 When to Use
- When object creation is expensive or complicated
- When you need to avoid subclassing factories for new types
- When you want to decouple code from concrete classes

## ✅ Pros
- Avoids costly initialization — copies existing objects
- Simplifies adding new object types at runtime
- Reduces dependency on concrete classes
- Enables dynamic configuration of objects

## ❌ Cons
- Requires implementing cloning logic for each class
- Deep vs shallow copying must be handled carefully
- Cloning complex object graphs can be tricky

## 🌍 Real-World Analogy
A 3D modeling tool: You create one tree model, then clone it multiple times with minor adjustments instead of rebuilding it each time.

## 📘 Related Patterns
- **Factory Method** – creates objects through inheritance, while Prototype uses cloning
- **Abstract Factory** – may use prototypes to produce product families
- **Builder** – constructs objects step by step, whereas Prototype copies an existing one