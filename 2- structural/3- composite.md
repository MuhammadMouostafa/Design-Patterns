# 🌿 Composite Pattern

## 📖 Category
Structural Design Pattern

## 🎯 Intent
Compose objects into tree structures to represent part-whole hierarchies. The Composite pattern lets clients treat individual objects and groups of objects uniformly.

## ⚙️ Problem
Sometimes you want to represent complex hierarchies like:

- Files and folders
- GUI elements (buttons, panels, windows)
- Company employees (managers, developers, etc.)

You need a way to treat both simple and composite objects through a common interface.

## 💡 Solution
- Define a Component interface that declares common operations.

- Implement Leaf classes for simple elements.

- Implement Composite classes that store child components and delegate work to them.

This way, the client can use a single interface for both individual objects and compositions.

## 🧩 Implementation in C++

```cpp
#include <iostream>
#include <vector>
#include <memory>
using namespace std;

// ----- Component Interface -----
class Graphic {
public:
    virtual void Draw() const = 0;
    virtual ~Graphic() = default;
};

// ----- Leaf -----
class Circle : public Graphic {
public:
    void Draw() const override {
        cout << "Drawing a Circle\n";
    }
};

class Square : public Graphic {
public:
    void Draw() const override {
        cout << "Drawing a Square\n";
    }
};

// ----- Composite -----
class Picture : public Graphic {
private:
    vector<shared_ptr<Graphic>> children;
public:
    void Add(shared_ptr<Graphic> g) {
        children.push_back(g);
    }

    void Draw() const override {
        cout << "Drawing a Picture with:\n";
        for (const auto& child : children) {
            child->Draw();
        }
    }
};

// ----- Client Code -----
int main() {
    auto circle = make_shared<Circle>();
    auto square = make_shared<Square>();
    auto picture = make_shared<Picture>();

    picture->Add(circle);
    picture->Add(square);

    cout << "Client draws everything through a common interface:\n";
    picture->Draw();
}
```

## 🧠 When to Use
- When you want to represent part–whole hierarchies (trees)
- When clients should treat simple and composite objects the same
- When recursive structures simplify your code

## ✅ Pros
- Simplifies client code by using a uniform interface
- Easy to add new component types
- Supports recursive composition naturally

## ❌ Cons
- Can make design more complex when the hierarchy is simple
- Harder to restrict which components can be in a composite

## 🌍 Real-World Analogy
A folder can contain both files and other folders. You can open any folder and perform the same operation (like listing contents) — even though what's inside may vary.

## 📘 Related Patterns
- **Decorator** – adds behavior dynamically, while Composite groups objects hierarchically
- **Flyweight** – optimizes memory when many similar leaf objects exist
- **Iterator** – can traverse composites uniformly