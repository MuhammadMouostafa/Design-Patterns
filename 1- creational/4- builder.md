# 🧱 Builder Pattern

## 📖 Category
**Creational Design Pattern**

---

## 🎯 Intent
Separate the **construction of a complex object** from its **representation**,  
so that the same construction process can create different representations.

---

## ⚙️ Problem
Sometimes creating an object is **too complex** because:
- It has many optional and required parts.
- Construction requires **multiple steps**.
- The same steps can produce **different results**.

Example: building a `House` object that can have walls, doors, windows, a garage, and a swimming pool.  
Using a long constructor or many setters makes code messy and error-prone.

---

## 💡 Solution
- Use a **Builder interface** to define the steps of building an object.  
- Implement **Concrete Builders** for different product variations.  
- Use a **Director** to manage the construction order.  
- The **client** configures the builder and retrieves the final product.

---

## 🧩 Implementation in C++
```cpp
#include <iostream>
#include <string>
#include <memory>

// ----- Product -----
class House {
private:
    std::string walls;
    std::string roof;
    std::string windows;
public:
    void SetWalls(const std::string& w) { walls = w; }
    void SetRoof(const std::string& r) { roof = r; }
    void SetWindows(const std::string& win) { windows = win; }

    void Show() const {
        std::cout << "House with " << walls << ", " << roof << " and " << windows << "\n";
    }
};

// ----- Builder Interface -----
class HouseBuilder {
public:
    virtual void BuildWalls() = 0;
    virtual void BuildRoof() = 0;
    virtual void BuildWindows() = 0;
    virtual std::unique_ptr<House> GetResult() = 0;
    virtual ~HouseBuilder() = default;
};

// ----- Concrete Builders -----
class WoodenHouseBuilder : public HouseBuilder {
private:
    std::unique_ptr<House> house;
public:
    WoodenHouseBuilder() { house = std::make_unique<House>(); }

    void BuildWalls() override { house->SetWalls("wooden walls"); }
    void BuildRoof() override { house->SetRoof("wooden roof"); }
    void BuildWindows() override { house->SetWindows("glass windows"); }

    std::unique_ptr<House> GetResult() override { return std::move(house); }
};

class BrickHouseBuilder : public HouseBuilder {
private:
    std::unique_ptr<House> house;
public:
    BrickHouseBuilder() { house = std::make_unique<House>(); }

    void BuildWalls() override { house->SetWalls("brick walls"); }
    void BuildRoof() override { house->SetRoof("concrete roof"); }
    void BuildWindows() override { house->SetWindows("double-glazed windows"); }

    std::unique_ptr<House> GetResult() override { return std::move(house); }
};

// ----- Director -----
class ConstructionEngineer {
private:
    HouseBuilder& builder;
public:
    ConstructionEngineer(HouseBuilder& b) : builder(b) {}
    std::unique_ptr<House> ConstructHouse() {
        builder.BuildWalls();
        builder.BuildRoof();
        builder.BuildWindows();
        return builder.GetResult();
    }
};

// ----- Client Code -----
int main() {
    WoodenHouseBuilder woodenBuilder;
    ConstructionEngineer engineer1(woodenBuilder);
    auto woodenHouse = engineer1.ConstructHouse();
    woodenHouse->Show();

    BrickHouseBuilder brickBuilder;
    ConstructionEngineer engineer2(brickBuilder);
    auto brickHouse = engineer2.ConstructHouse();
    brickHouse->Show();

    return 0;
}
```

## 🧠 When to Use
- The object needs complex step-by-step construction
- Different configurations or representations of the same product are required
- You want to reuse the same construction process for different object types

## ✅ Pros
- Builds complex objects step-by-step
- Same construction code can produce different products
- Improves readability and flexibility
- Separates construction logic from the product's representation

## ❌ Cons
- Adds extra classes and structure
- Not ideal for simple object creation

## 🌍 Real-World Analogy
A restaurant kitchen: The chef (Director) follows the same steps to make a meal, but depending on the recipe (Builder), the final dish differs.

## 📘 Related Patterns
- **Abstract Factory** – creates families of products; Builder focuses on one complex object
- **Prototype** – creates objects by copying an existing one
- **Composite** – can use Builder to construct complex object hierarchies