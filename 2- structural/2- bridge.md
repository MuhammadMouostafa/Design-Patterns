# 🌉 Bridge Pattern

## 📖 Category
Structural Design Pattern

## 🎯 Intent
Decouple an abstraction from its implementation so that both can evolve independently.

## ⚙️ Problem
When a class hierarchy grows in multiple dimensions — for example:

- Different Shapes (Circle, Square, …)
- Different Rendering APIs (OpenGL, DirectX, …)

Using inheritance for each combination (OpenGLCircle, DirectXSquare, …) leads to a class explosion and tight coupling.

## 💡 Solution
Split the hierarchy into two independent parts:

- **Abstraction** (e.g., Shape)
- **Implementation** (e.g., Renderer)

The abstraction delegates work to the implementation interface.

Both can change independently without affecting the other.


## 🧱 Structure
- **Abstraction** – defines the high-level interface and maintains a reference to the implementor.
- **RefinedAbstraction** – extends the abstraction with additional functionality.
- **Implementor** – defines the low-level interface.
- **ConcreteImplementor** – provides specific implementation details.

---

## 🧩 Implementation in C++

```cpp
#include <iostream>
#include <memory>
using namespace std;

// ----- Implementor -----
class Renderer {
public:
    virtual void RenderCircle(float x, float y, float radius) = 0;
    virtual ~Renderer() = default;
};

// ----- Concrete Implementors -----
class OpenGLRenderer : public Renderer {
public:
    void RenderCircle(float x, float y, float radius) override {
        cout << "OpenGL: Drawing circle at (" << x << ", " << y
             << ") with radius " << radius << endl;
    }
};

class DirectXRenderer : public Renderer {
public:
    void RenderCircle(float x, float y, float radius) override {
        cout << "DirectX: Drawing circle at (" << x << ", " << y
             << ") with radius " << radius << endl;
    }
};

// ----- Abstraction -----
class Shape {
protected:
    shared_ptr<Renderer> renderer;
public:
    Shape(shared_ptr<Renderer> r) : renderer(r) {}
    virtual void Draw() = 0;
    virtual ~Shape() = default;
};

// ----- Refined Abstraction -----
class Circle : public Shape {
    float x, y, radius;
public:
    Circle(shared_ptr<Renderer> r, float x, float y, float radius)
        : Shape(r), x(x), y(y), radius(radius) {}
    void Draw() override {
        renderer->RenderCircle(x, y, radius);
    }
};

// ----- Client Code -----
int main() {
    auto opengl = make_shared<OpenGLRenderer>();
    auto directx = make_shared<DirectXRenderer>();

    Circle c1(opengl, 5, 10, 3);
    Circle c2(directx, 2, 4, 6);

    c1.Draw();
    c2.Draw();
}
```

## 🧠 When to Use
- When abstraction and implementation should evolve independently
- When you want to switch implementations at runtime
- When using inheritance creates too many subclasses

## ✅ Pros
- Reduces code duplication and coupling
- Both sides can vary independently
- Improves scalability and maintainability

## ❌ Cons
- Adds an extra layer of abstraction
- Can increase complexity for small systems

## 🌍 Real-World Analogy
A remote control (abstraction) can work with different TV brands (implementations). The remote doesn't care about the internal details — it just sends commands to whichever TV it's connected to.

## 📘 Related Patterns
- **Adapter** – makes incompatible interfaces work together, while Bridge separates abstraction from implementation
- **Abstract Factory** – can be used to create implementation objects for the Bridge
- **Strategy** – focuses on interchangeable algorithms, while Bridge focuses on abstraction–implementation separation