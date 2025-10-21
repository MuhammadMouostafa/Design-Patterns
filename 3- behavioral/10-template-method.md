# 🧰 Template Method Pattern (Behavioral)

## 📖 Category
**Behavioral Design Pattern**

---

## 🎯 Intent
Define the **skeleton of an algorithm** in a base class but let **subclasses override specific steps** without changing the algorithm’s overall structure.

---

## ⚙️ Problem
In many systems, multiple classes share a **common algorithm structure** but differ in certain details.  
If each class implements the entire algorithm separately:
- You duplicate code across classes.  
- Changes to the shared structure require modifying every subclass.  
- It becomes difficult to maintain and extend.

Example:  
You have different types of data processors — for CSV, JSON, and XML files.  
Each processor follows the same steps (open → read → parse → close), but their parsing logic differs.

---

## 💡 Solution
- Define a **base class** that contains the common algorithm steps in a **template method**.  
- The template method calls **abstract (or virtual)** methods for steps that subclasses should customize.  
- Subclasses **override only the variable parts**, leaving the structure intact.

This enforces consistency while allowing flexibility in individual steps.

---

## 🧩 Implementation in C++
```cpp
#include <iostream>
using namespace std;

// ----- Abstract Base Class -----
class DataParser {
public:
    // Template Method (defines the skeleton)
    void parseData() {
        openFile();
        readData();
        parseContent();
        closeFile();
    }

protected:
    void openFile() { cout << "📂 Opening file...\n"; }
    void readData() { cout << "📖 Reading data from file...\n"; }

    // Step to be customized by subclasses
    virtual void parseContent() = 0;

    void closeFile() { cout << "✅ Closing file.\n"; }
    virtual ~DataParser() = default;
};

// ----- Concrete Subclass: CSV Parser -----
class CSVParser : public DataParser {
protected:
    void parseContent() override {
        cout << "🔍 Parsing CSV data...\n";
    }
};

// ----- Concrete Subclass: JSON Parser -----
class JSONParser : public DataParser {
protected:
    void parseContent() override {
        cout << "🧩 Parsing JSON data...\n";
    }
};

// ----- Client Code -----
int main() {
    cout << "--- CSV Parsing ---\n";
    CSVParser csv;
    csv.parseData();

    cout << "\n--- JSON Parsing ---\n";
    JSONParser json;
    json.parseData();
}
```

## 🧠 When to Use
- When multiple classes share the same algorithm structure but differ in some steps
- When you want to avoid code duplication by keeping common behavior in a base class
- When you need to enforce a specific sequence of operations
- When you want to allow subclasses to redefine specific steps without changing the algorithm's flow

## ✅ Pros
- Promotes code reuse — common logic is centralized in the base class
- Makes algorithms consistent across subclasses
- Easy to extend — just override necessary steps
- Supports inversion of control — base class controls algorithm flow, subclasses provide details

## ❌ Cons
- Tight coupling between base and subclasses
- Difficult to change the algorithm structure without affecting all subclasses
- Can lead to inheritance rigidity — all behavior extensions must go through subclassing

## 🌍 Real-World Analogy
- Think of a coffee machine: The machine follows a fixed process — boil water → brew coffee → pour into cup → add extras. You can customize certain steps (like "add extras") but the overall process remains the same.

## 📘 Related Patterns
- **Strategy** – encapsulates entire algorithms and lets you swap them; Template Method keeps the algorithm fixed but allows overriding certain steps
- **Factory Method** – often used inside Template Methods to delegate object creation
- **Hook Methods** – optional steps in Template Method that subclasses may override