# 🪶 Flyweight Pattern

## 📖 Category
**Structural Design Pattern**

---

## 🎯 Intent
Use **sharing** to support large numbers of fine-grained objects efficiently.  
The Flyweight pattern minimizes memory usage by sharing common state between multiple objects.

---

## ⚙️ Problem
Applications like text editors, games, or GUIs can create **millions of similar objects**.  
Each object often stores the same data, leading to **high memory consumption**.

Example:  
- A text editor stores each character as an object.  
- Most characters share the same font, color, and size — storing them repeatedly wastes memory.

---

## 💡 Solution
- Separate object data into:
  - **Intrinsic state** → shared and stored inside the flyweight (e.g., font, color).  
  - **Extrinsic state** → unique and passed from outside (e.g., character position).  
- Use a **Flyweight Factory** to manage and reuse shared objects.

---

## 🧩 Implementation in C++
```cpp
#include <iostream>
#include <string>
#include <unordered_map>
#include <memory>
using namespace std;

// ----- Flyweight -----
class CharacterFlyweight {
private:
    char symbol;
    string font;
public:
    CharacterFlyweight(char s, const string& f) : symbol(s), font(f) {}
    void Display(int x, int y) const {
        cout << "Character '" << symbol << "' with font [" << font 
             << "] at position (" << x << ", " << y << ")\n";
    }
};

// ----- Flyweight Factory -----
class FlyweightFactory {
private:
    unordered_map<string, shared_ptr<CharacterFlyweight>> flyweights;
    string GetKey(char symbol, const string& font) const {
        return string(1, symbol) + "_" + font;
    }
public:
    shared_ptr<CharacterFlyweight> GetFlyweight(char symbol, const string& font) {
        string key = GetKey(symbol, font);
        if (flyweights.find(key) == flyweights.end()) {
            flyweights[key] = make_shared<CharacterFlyweight>(symbol, font);
            cout << "Creating new Flyweight: " << key << endl;
        }
        return flyweights[key];
    }
};

// ----- Client Code -----
class CharacterContext {
private:
    shared_ptr<CharacterFlyweight> flyweight;
    int x, y; // Extrinsic state
public:
    CharacterContext(shared_ptr<CharacterFlyweight> f, int x, int y)
        : flyweight(f), x(x), y(y) {}
    void Display() const {
        flyweight->Display(x, y);
    }
};

int main() {
    FlyweightFactory factory;

    auto a1 = factory.GetFlyweight('A', "Arial");
    auto a2 = factory.GetFlyweight('A', "Arial");
    auto b1 = factory.GetFlyweight('B', "Arial");

    CharacterContext c1(a1, 10, 20);
    CharacterContext c2(a2, 30, 40);
    CharacterContext c3(b1, 50, 60);

    cout << "\n--- Displaying Characters ---\n";
    c1.Display();
    c2.Display();
    c3.Display();
}
```

## 🧠 When to Use
- When your program uses a large number of similar objects
- When object identity is not critical
- When most object state can be shared externally

## ✅ Pros
- Greatly reduces memory usage
- Improves performance with shared data
- Centralized management of shared objects

## ❌ Cons
- Code becomes more complex
- Makes the program dependent on external (extrinsic) state
- May reduce flexibility when state sharing isn't clear-cut

## 🌍 Real-World Analogy
A font glyph in a text editor: Each character on the screen shares the same shape data (intrinsic) but has its own position (extrinsic).

## 📘 Related Patterns
- **Factory Method** – can create flyweights
- **Composite** – Flyweight often used within composites for efficient storage
- **Proxy** – may manage access to shared flyweights