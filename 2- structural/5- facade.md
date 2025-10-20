# 🏛️ Facade Pattern

## 📖 Category
**Structural Design Pattern**

---

## 🎯 Intent
Provide a **unified, simplified interface** to a complex subsystem.  
The Facade pattern defines a **high-level interface** that makes a system easier to use.

---

## ⚙️ Problem
Large systems often have **multiple complex classes** with many dependencies.  
Clients that use them must understand and interact with all their details.

Example:
- A computer startup involves many subsystems like CPU, Memory, and HardDrive.
- The client should not need to manage each one manually.

---

## 💡 Solution
- Create a **Facade class** that wraps and coordinates the subsystems.  
- The facade exposes **simple methods** for the client to perform common operations.  
- Clients interact only with the facade instead of dealing with the internal details.

---

## 🧩 Implementation in C++
```cpp
#include <iostream>
using namespace std;

// ----- Subsystem Classes -----
class CPU {
public:
    void Freeze() { cout << "CPU: Freezing operations...\n"; }
    void Jump(long position) { cout << "CPU: Jumping to position " << position << endl; }
    void Execute() { cout << "CPU: Executing instructions...\n"; }
};

class Memory {
public:
    void Load(long position, const string& data) {
        cout << "Memory: Loading data '" << data << "' at position " << position << endl;
    }
};

class HardDrive {
public:
    string Read(long lba, int size) {
        cout << "HardDrive: Reading " << size << " bytes from sector " << lba << endl;
        return "Boot data";
    }
};

// ----- Facade -----
class ComputerFacade {
private:
    CPU cpu;
    Memory memory;
    HardDrive hardDrive;
public:
    void Start() {
        cout << "\n=== Starting Computer ===\n";
        cpu.Freeze();
        string data = hardDrive.Read(0, 512);
        memory.Load(0, data);
        cpu.Jump(0);
        cpu.Execute();
        cout << "=== Computer Started ===\n";
    }
};

// ----- Client Code -----
int main() {
    ComputerFacade computer;
    computer.Start();
}
```

# Facade Pattern

## 🧠 When to Use
- When you want to provide a simple interface to a complex subsystem
- When there are many dependencies between clients and subsystems
- When you want to decouple clients from implementation details

## ✅ Pros
- Simplifies usage of complex systems
- Reduces coupling between clients and subsystems
- Promotes cleaner and more maintainable code

## ❌ Cons
- Can hide important functionality of subsystems
- If not designed carefully, it can become an object that does too much

## 🌍 Real-World Analogy
A restaurant waiter acts as a facade: you don't talk to the chef, cashier, or kitchen staff — you just place your order through the waiter, who handles all the details for you.

## 📘 Related Patterns
- **Adapter** – makes interfaces compatible, while Facade simplifies them
- **Mediator** – centralizes communication between objects, while Facade provides a simple entry point
- **Abstract Factory** – often used with Facade to create subsystem components