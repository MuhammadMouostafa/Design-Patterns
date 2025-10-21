# 🔗 Chain of Responsibility Pattern

## 📖 Category
**Behavioral Design Pattern**

---

## 🎯 Intent
Allow multiple objects a chance to handle a request **without coupling** the sender to any specific receiver.  
The request is passed along a **chain of handlers**, and each handler decides either to **process** the request or **pass it** to the next one.

---

## ⚙️ Problem
You want to **process requests in different ways**, depending on their type or context,  
but you don’t want to hardcode multiple `if-else` or `switch` statements.

Example:
- Different levels of **support staff** handle different types of user issues.  
- If one can’t handle it, they escalate it to the next level.

---

## 💡 Solution
- Create a **Handler interface** that defines a method for processing requests.  
- Each **Concrete Handler** either handles the request or forwards it to the next handler in the chain.

---

## 🧩 Implementation in C++
```cpp
#include <iostream>
#include <memory>
#include <string>
using namespace std;

// ----- Handler Interface -----
class Handler {
protected:
    shared_ptr<Handler> next;
public:
    virtual ~Handler() = default;

    void SetNext(shared_ptr<Handler> handler) {
        next = handler;
    }

    virtual void HandleRequest(const string& request) {
        if (next) next->HandleRequest(request);
    }
};

// ----- Concrete Handlers -----
class LowLevelSupport : public Handler {
public:
    void HandleRequest(const string& request) override {
        if (request == "low") {
            cout << "LowLevelSupport: Handling low-level issue.\n";
        } else if (next) {
            cout << "LowLevelSupport: Passing to next handler.\n";
            next->HandleRequest(request);
        }
    }
};

class MidLevelSupport : public Handler {
public:
    void HandleRequest(const string& request) override {
        if (request == "medium") {
            cout << "MidLevelSupport: Handling medium-level issue.\n";
        } else if (next) {
            cout << "MidLevelSupport: Passing to next handler.\n";
            next->HandleRequest(request);
        }
    }
};

class HighLevelSupport : public Handler {
public:
    void HandleRequest(const string& request) override {
        if (request == "high") {
            cout << "HighLevelSupport: Handling high-level issue.\n";
        } else {
            cout << "HighLevelSupport: No handler available for this request.\n";
        }
    }
};

// ----- Client Code -----
int main() {
    auto low = make_shared<LowLevelSupport>();
    auto mid = make_shared<MidLevelSupport>();
    auto high = make_shared<HighLevelSupport>();

    low->SetNext(mid);
    mid->SetNext(high);

    cout << "--- Sending 'low' request ---\n";
    low->HandleRequest("low");

    cout << "\n--- Sending 'medium' request ---\n";
    low->HandleRequest("medium");

    cout << "\n--- Sending 'high' request ---\n";
    low->HandleRequest("high");

    cout << "\n--- Sending 'unknown' request ---\n";
    low->HandleRequest("unknown");

    return 0;
}
```

## 🧠 When to Use
- When multiple objects may handle a request and the exact handler isn't known in advance
- When you want to avoid coupling the sender to the receiver
- When you want to process requests sequentially or conditionally

## ✅ Pros
- Reduces coupling between sender and receivers
- Simplifies request handling logic
- Enables flexible and dynamic assignment of responsibilities

## ❌ Cons
- Request might go unhandled if no handler takes responsibility
- Can be hard to debug if the chain is long or dynamic

## 🌍 Real-World Analogy
A customer service system: Your issue starts with the first-level agent. If they can't solve it, it's escalated to the next support level, and so on.

## 📘 Related Patterns
- **Command** – encapsulates a request as an object; can be used together
- **Mediator** – centralizes communication instead of passing along a chain
- **Observer** – broadcasts events to many objects instead of passing sequentially