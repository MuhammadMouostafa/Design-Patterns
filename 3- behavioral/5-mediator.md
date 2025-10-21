# 🗣️ Mediator Pattern

## 📖 Category
**Behavioral Design Pattern**

---

## 🎯 Intent
Define an object that **encapsulates communication** between multiple objects (colleagues),  
so they **don’t communicate directly** with each other — reducing dependencies and coupling.

---

## ⚙️ Problem
In complex systems, many objects interact with each other.  
If every object directly references and communicates with others:
- The system becomes **tightly coupled**,  
- Changing one class can break many others,  
- Reuse becomes difficult.

Example:
- A chat application where users send messages to each other — without a central mediator,  
  each user must know all others directly.

---

## 💡 Solution
- Create a **Mediator interface** that defines how colleagues communicate.  
- Each **Colleague** only talks to the **Mediator**, not directly to others.  
- The **Concrete Mediator** routes and coordinates messages between colleagues.

---

## 🧩 Implementation in C++
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <memory>
using namespace std;

// Forward declaration
class Mediator;

// ----- Colleague -----
class Colleague {
protected:
    Mediator* mediator;
    string name;
public:
    Colleague(Mediator* m, const string& n) : mediator(m), name(n) {}
    virtual void Send(const string& message) = 0;
    virtual void Receive(const string& message, const string& sender) = 0;
    string GetName() const { return name; }
    virtual ~Colleague() = default;
};

// ----- Mediator Interface -----
class Mediator {
public:
    virtual void SendMessage(const string& message, Colleague* sender) = 0;
    virtual void AddColleague(shared_ptr<Colleague> colleague) = 0;
    virtual ~Mediator() = default;
};

// ----- Concrete Mediator -----
class ChatMediator : public Mediator {
private:
    vector<shared_ptr<Colleague>> colleagues;
public:
    void AddColleague(shared_ptr<Colleague> colleague) override {
        colleagues.push_back(colleague);
    }

    void SendMessage(const string& message, Colleague* sender) override {
        for (auto& c : colleagues) {
            if (c->GetName() != sender->GetName()) {
                c->Receive(message, sender->GetName());
            }
        }
    }
};

// ----- Concrete Colleague -----
class User : public Colleague {
public:
    User(Mediator* m, const string& n) : Colleague(m, n) {}
    void Send(const string& message) override {
        cout << name << " sends: " << message << endl;
        mediator->SendMessage(message, this);
    }
    void Receive(const string& message, const string& sender) override {
        cout << name << " receives from " << sender << ": " << message << endl;
    }
};

// ----- Client Code -----
int main() {
    ChatMediator chat;

    auto user1 = make_shared<User>(&chat, "Alice");
    auto user2 = make_shared<User>(&chat, "Bob");
    auto user3 = make_shared<User>(&chat, "Charlie");

    chat.AddColleague(user1);
    chat.AddColleague(user2);
    chat.AddColleague(user3);

    user1->Send("Hello everyone!");
    user2->Send("Hey Alice!");
    user3->Send("Hi Bob!");

    return 0;
}
```

## 🧠 When to Use
- When multiple objects communicate in complex ways
- When you want to reduce direct dependencies between interacting objects
- When communication logic needs to be centralized for easier maintenance

## ✅ Pros
- Reduces coupling between colleague classes
- Centralizes communication and control logic
- Makes it easier to modify, extend, or reuse individual components

## ❌ Cons
- The Mediator can become too complex if it handles too many responsibilities
- Moves coupling from many classes to one central class (risk of a "God object")

## 🌍 Real-World Analogy
An air traffic controller acts as a mediator between airplanes. Planes don't communicate directly with each other — they coordinate through the control tower.

## 📘 Related Patterns
- **Observer** – can be used together to broadcast updates
- **Facade** – simplifies interface to a subsystem; Mediator coordinates between peers
- Colleague objects are often combined with **Command** or **State** patterns