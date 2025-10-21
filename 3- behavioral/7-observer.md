# 👀 Observer Pattern (Behavioral)

## 📖 Category
**Behavioral Design Pattern**

---

## 🎯 Intent
Define a **one-to-many dependency** between objects so that when one object (the **Subject**) changes its state, all its **Observers** are notified and updated automatically.

---

## ⚙️ Problem
In many systems, multiple objects need to stay in sync with the state of another object.  
If we tightly couple them:
- Every time the main object changes, we must manually update all dependents.  
- Adding new dependent classes requires modifying the main class.  
- The code becomes **rigid and hard to maintain**.

Example:  
- A stock price tracker needs to notify multiple displays and loggers whenever a price changes.  
  Hard-coding all dependencies inside the Stock class breaks flexibility.

---

## 💡 Solution
- Create a **Subject** interface that maintains a list of Observers and notifies them of any changes.  
- Observers register or unregister themselves with the Subject.  
- When the Subject’s state changes, it **notifies all observers** automatically.  

This makes communication **loosely coupled** and **extensible** — new observers can be added without modifying existing code.

---

## 🧩 Implementation in C++
```cpp
#include <iostream>
#include <vector>
#include <string>
#include <algorithm>
using namespace std;

// ----- Observer Interface -----
class IObserver {
public:
    virtual void update(float price) = 0;
    virtual ~IObserver() = default;
};

// ----- Subject Interface -----
class ISubject {
public:
    virtual void attach(IObserver* observer) = 0;
    virtual void detach(IObserver* observer) = 0;
    virtual void notify() = 0;
    virtual ~ISubject() = default;
};

// ----- Concrete Subject -----
class Stock : public ISubject {
    vector<IObserver*> observers;
    float price;
public:
    void setPrice(float newPrice) {
        price = newPrice;
        notify();
    }
    void attach(IObserver* observer) override {
        observers.push_back(observer);
    }
    void detach(IObserver* observer) override {
        observers.erase(remove(observers.begin(), observers.end(), observer), observers.end());
    }
    void notify() override {
        for (auto* observer : observers)
            observer->update(price);
    }
};

// ----- Concrete Observers -----
class MobileAppDisplay : public IObserver {
public:
    void update(float price) override {
        cout << "📱 Mobile App: Stock price updated to " << price << endl;
    }
};

class WebDashboard : public IObserver {
public:
    void update(float price) override {
        cout << "💻 Web Dashboard: Stock price updated to " << price << endl;
    }
};

// ----- Client Code -----
int main() {
    Stock stock;

    MobileAppDisplay mobile;
    WebDashboard web;

    stock.attach(&mobile);
    stock.attach(&web);

    stock.setPrice(120.5f);
    stock.setPrice(125.0f);

    stock.detach(&web);
    stock.setPrice(130.0f);
}
```

## 🧠 When to Use
- When multiple objects need to be notified about state changes in another object
- When you want to decouple the Subject from its dependents
- When adding or removing observers should not require code changes in the Subject

## ✅ Pros
- Promotes loose coupling between Subject and Observers
- Makes it easy to add or remove observers dynamically
- Ensures synchronized state updates across dependent objects
- Promotes Open/Closed Principle — new observers can be added without modifying existing code

## ❌ Cons
- Notifications may cause unexpected update cascades or performance overhead if many observers exist
- Difficult to debug due to indirect communication
- No guaranteed order of notification (depends on implementation)

## 🌍 Real-World Analogy
A YouTube channel acts as the Subject. When it uploads a new video, all subscribed users (Observers) receive a notification automatically.

## 📘 Related Patterns
- **Mediator** – centralizes communication; Observer distributes it automatically
- **Singleton** – often used to manage global event managers or Subjects
- **Event-driven architecture** – uses similar principles for system-wide notifications