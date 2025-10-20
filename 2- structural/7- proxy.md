# 🧱 Proxy Pattern

## 📖 Category
**Structural Design Pattern**

---

## 🎯 Intent
Provide a **substitute** or **placeholder** for another object to control access to it.  
The Proxy acts as an intermediary that adds additional behavior — such as access control, lazy initialization, or logging — before delegating requests to the real object.

---

## ⚙️ Problem
Sometimes, direct access to an object is:
- **Expensive** (e.g., heavy object creation or remote access),
- **Unsafe** (e.g., needs access control),
- **Unnecessary** (e.g., object not needed until a specific point).

You need a way to **delay**, **control**, or **monitor** access without changing the original object.

---

## 💡 Solution
- Create an interface that both the **Real Object** and the **Proxy** implement.
- The **Proxy** holds a reference to the real object and controls its access.
- The client interacts with the proxy instead of the real object.

---

## 🧩 Implementation in C++
```cpp
#include <iostream>
#include <memory>
#include <string>
using namespace std;

// ----- Subject Interface -----
class Image {
public:
    virtual void Display() const = 0;
    virtual ~Image() = default;
};

// ----- Real Subject -----
class RealImage : public Image {
private:
    string filename;
public:
    RealImage(const string& fname) : filename(fname) {
        cout << "Loading image from disk: " << filename << endl;
    }
    void Display() const override {
        cout << "Displaying image: " << filename << endl;
    }
};

// ----- Proxy -----
class ProxyImage : public Image {
private:
    string filename;
    mutable shared_ptr<RealImage> realImage; // lazy-loaded
public:
    ProxyImage(const string& fname) : filename(fname) {}

    void Display() const override {
        if (!realImage) {
            cout << "Proxy: Instantiating RealImage only when needed.\n";
            realImage = make_shared<RealImage>(filename);
        }
        realImage->Display();
    }
};

// ----- Client Code -----
int main() {
    cout << "--- Creating Proxy Object ---\n";
    ProxyImage img("photo.png");

    cout << "\n--- First Display (loads image) ---\n";
    img.Display();

    cout << "\n--- Second Display (uses cached image) ---\n";
    img.Display();
}
```

## 🧠 When to Use

- When creating a heavy object is resource-intensive and can be delayed.

- When you need to control or log access to an object.

- When accessing a remote or restricted object.

## 🧩 Common Proxy Types

| Type                 | Purpose                                           |
| -------------------- | ------------------------------------------------- |
| **Virtual Proxy**    | Lazily creates heavy objects when needed          |
| **Protection Proxy** | Controls access based on permissions              |
| **Remote Proxy**     | Represents an object in a different address space |
| **Logging Proxy**    | Logs access and requests to an object             |
| **Caching Proxy**    | Stores results to reuse them later                |

## ✅ Pros
- Adds control and flexibility to object access
- Supports lazy initialization (saves resources)
- Helps implement security and logging transparently

## ❌ Cons
- Adds complexity and one extra layer of indirection
- May slow performance slightly due to delegation

## 🌍 Real-World Analogy
A credit card acts as a proxy for cash. You don't always pay with real money directly — the proxy (card) manages access and verification before completing the transaction.

## 📘 Related Patterns
- **Decorator** – similar structure but adds behavior, not control
- **Adapter** – changes interface, while Proxy keeps the same one
- **Flyweight** – also reduces cost, but via object sharing, not control
