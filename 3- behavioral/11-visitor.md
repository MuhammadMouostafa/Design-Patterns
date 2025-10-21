# 🧳 Visitor Pattern

## 📖 Category
Behavioral Design Pattern

## 🎯 Intent
Represent an operation to be performed on elements of an object structure. The Visitor Pattern lets you define a new operation without changing the classes of the elements it operates on.

## ⚙️ Problem
You have a collection of objects of different types (e.g., shapes: circle, rectangle, triangle). You often need to perform new operations on them — like rendering, exporting, or calculating area.

If you add these operations directly inside each class:

- You'll break the Open/Closed Principle (you must modify existing classes)
- Your classes become bloated with unrelated behavior
- Adding new operations becomes error-prone and hard to maintain

**Example:**
Imagine you have a drawing app with several shape classes. Later, you need to implement exporting, measurements, or transformations. If you modify every class for each new operation — it quickly becomes unmanageable.

## 💡 Solution
- Create a Visitor interface that declares visit() methods for each element type
- Each Element class implements an accept() method that calls the corresponding visit() on the visitor
- You can add new operations by creating new visitor classes without modifying the existing element hierarchy
- This separates object structure (data) from operations (behavior)

## 🧩 Implementation in C++
```cpp
#include <iostream>
#include <vector>
using namespace std;

// ----- Forward Declarations -----
class Circle;
class Rectangle;

// ----- Visitor Interface -----
class Visitor {
public:
    virtual void visit(Circle& circle) = 0;
    virtual void visit(Rectangle& rectangle) = 0;
    virtual ~Visitor() = default;
};

// ----- Element Interface -----
class Shape {
public:
    virtual void accept(Visitor& visitor) = 0;
    virtual ~Shape() = default;
};

// ----- Concrete Elements -----
class Circle : public Shape {
public:
    void accept(Visitor& visitor) override {
        visitor.visit(*this);
    }
};

class Rectangle : public Shape {
public:
    void accept(Visitor& visitor) override {
        visitor.visit(*this);
    }
};

// ----- Concrete Visitors -----
class DrawVisitor : public Visitor {
public:
    void visit(Circle&) override {
        cout << "🎨 Drawing a Circle\n";
    }
    void visit(Rectangle&) override {
        cout << "📐 Drawing a Rectangle\n";
    }
};

class AreaVisitor : public Visitor {
public:
    void visit(Circle&) override {
        cout << "📏 Calculating area of Circle\n";
    }
    void visit(Rectangle&) override {
        cout << "📏 Calculating area of Rectangle\n";
    }
};

// ----- Client Code -----
int main() {
    vector<Shape*> shapes = { new Circle(), new Rectangle() };

    DrawVisitor drawVisitor;
    AreaVisitor areaVisitor;

    cout << "--- Drawing Shapes ---\n";
    for (auto shape : shapes)
        shape->accept(drawVisitor);

    cout << "\n--- Calculating Areas ---\n";
    for (auto shape : shapes)
        shape->accept(areaVisitor);

    for (auto shape : shapes) delete shape;
}
```


## 🧠 When to Use
- When you need to perform many unrelated operations on elements of a structure
- When you want to add new operations without modifying existing classes
- When the object structure is stable, but operations change frequently
- When operations need to access the internal state of elements that shouldn't expose data publicly

## ✅ Pros
- Open for extension — add new operations easily
- Keeps element classes simple and focused
- Centralizes related behavior in visitor classes
- Promotes cleaner separation between data and operations

## ❌ Cons
- Adding a new element type requires modifying all visitors
- Tight coupling between visitors and element structure
- Complex to implement when there are many element types

## 🌍 Real-World Analogy
Think of a tax auditor visiting different types of companies — factories, schools, hospitals. Each company accepts the auditor, and the auditor performs specific checks based on the company type. You can introduce new kinds of audits (financial, safety, quality) by creating new visitors, without changing the companies themselves.

## 📘 Related Patterns
- **Composite** – often used with Visitor to traverse hierarchical structures
- **Interpreter** – can use Visitor to process grammar trees
- **Strategy** – encapsulates interchangeable behavior, but unlike Visitor, it doesn't separate data and operation
- **Double Dispatch** – Visitor relies on this mechanism to call the correct visit() based on both visitor and element types