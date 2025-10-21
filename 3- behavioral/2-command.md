# 🕹️ Command Pattern

## 📖 Category
**Behavioral Design Pattern**

---

## 🎯 Intent
Encapsulate a **request as an object**, allowing you to:
- Parameterize methods with different requests,
- Queue or log requests,
- Support **undo/redo** operations.

---

## ⚙️ Problem
You want to issue requests to objects **without knowing anything** about:
- The request’s action,
- The receiver of the request.

Example:
- A text editor supports **undo/redo** — every operation should be stored and executed later.  
- Or, a GUI button performs different actions depending on the assigned command.

---

## 💡 Solution
- Define a **Command interface** with an `Execute()` method.  
- Each **Concrete Command** implements `Execute()` to call specific operations on a **Receiver**.  
- The **Invoker** (e.g., a button or remote control) calls `Execute()` on the command object.

---

## 🧩 Implementation in C++
```cpp
#include <iostream>
#include <memory>
#include <vector>
#include <string>
using namespace std;

// ----- Command Interface -----
class ICommand {
public:
    virtual void Execute() = 0;
    virtual ~ICommand() = default;
};

// ----- Receiver -----
class Light {
public:
    void TurnOn() { cout << "Light is ON\n"; }
    void TurnOff() { cout << "Light is OFF\n"; }
};

// ----- Concrete Commands -----
class TurnOnCommand : public ICommand {
private:
    Light* light;
public:
    TurnOnCommand(Light* l) : light(l) {}
    void Execute() override {
        light->TurnOn();
    }
};

class TurnOffCommand : public ICommand {
private:
    Light* light;
public:
    TurnOffCommand(Light* l) : light(l) {}
    void Execute() override {
        light->TurnOff();
    }
};

// ----- Invoker -----
class RemoteControl {
private:
    vector<shared_ptr<ICommand>> history;
public:
    void Submit(shared_ptr<ICommand> command) {
        command->Execute();
        history.push_back(command);
    }
};

// ----- Client Code -----
int main() {
    Light livingRoomLight;

    auto turnOn = make_shared<TurnOnCommand>(&livingRoomLight);
    auto turnOff = make_shared<TurnOffCommand>(&livingRoomLight);

    RemoteControl remote;
    remote.Submit(turnOn);
    remote.Submit(turnOff);

    return 0;
}
```

## 🧠 When to Use
- To decouple objects that issue requests from those that execute them
- When you want to queue, log, or undo/redo operations
- To support macro commands (grouping multiple commands together)

## ✅ Pros
- Decouples sender and receiver
- Makes it easy to add new commands without changing existing code
- Enables logging, queuing, and undo functionality
- Supports composite (macro) commands

## ❌ Cons
- Increases the number of classes (one per command)
- May add unnecessary complexity for simple operations

## 🌍 Real-World Analogy
A remote control is a classic example: Each button (Invoker) is assigned a command (Concrete Command) that operates on a device (Receiver). You can reassign commands to buttons dynamically.

## 📘 Related Patterns
- **Chain of Responsibility** – passes requests along a chain instead of executing directly
- **Memento** – can store state for undoable commands
- **Strategy** – encapsulates algorithms; Command encapsulates requests