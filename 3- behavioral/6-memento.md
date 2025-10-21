# 🧠 Memento Pattern (Behavioral)

## 📘 Overview
The **Memento Pattern** captures and externalizes an object’s internal state so that it can be restored later, **without violating encapsulation**.  
It’s commonly used to implement **undo/redo** functionality.

---

## 🎯 Intent
- Save and restore an object’s previous state.  
- Keep state history without exposing internal details.

---

## ⚙️ Problem
In applications that modify data frequently (like text editors, drawing tools, or IDEs), users often need to **undo or redo** actions.  
However, if we directly store and expose an object’s internal state:
- It **breaks encapsulation**,  
- External classes gain access to private data,  
- The system becomes harder to maintain and error-prone.

Example:  
- A text editor wants to support “Undo,” but its internal content is private.  
  Exposing it to other classes would violate encapsulation.

## 💡 Example Without Memento (Tightly Coupled) in C++
```cpp
#include <iostream>
#include <string>
using namespace std;

class Editor {
    string content;
public:
    void type(const string& words) { content += words; }
    string getContent() const { return content; }
};

int main() {
    Editor editor;
    editor.type("Hello ");
    editor.type("World!");

    cout << "Current Content: " << editor.getContent() << endl;
    // No way to restore an old state!
}
```

---

## 💡 Solution
- Introduce a **Memento** object that can **store the internal state** of another object safely.  
- The **Originator** creates and restores Mementos.  
- The **Caretaker** manages these Mementos (for example, keeps a stack of them for undo/redo) but never inspects their contents.  

This way, we can save and restore states **without exposing private fields** or breaking encapsulation.

## ✅ Solution With Memento Pattern in C++

```cpp
#include <iostream>
#include <string>
#include <stack>
using namespace std;

class Memento {
    string state;
public:
    Memento(string s) : state(move(s)) {}
    string getState() const { return state; }
};

class Editor {
    string content;
public:
    void type(const string& words) { content += words; }
    Memento save() const { return Memento(content); }
    void restore(const Memento& memento) { content = memento.getState(); }
    string getContent() const { return content; }
};

int main() {
    Editor editor;
    stack<Memento> history;

    editor.type("Hello ");
    history.push(editor.save());

    editor.type("World!");
    history.push(editor.save());

    cout << "Current Content: " << editor.getContent() << endl;

    // Undo last change
    editor.restore(history.top());
    history.pop();

    cout << "After Undo: " << editor.getContent() << endl;
}
```

## 🧠 When to Use
- When you need to **save and restore** an object’s state (undo/redo, checkpoints).
- When you want to **preserve encapsulation** — the object’s internal state should not be exposed.
- When the state of an object changes frequently and you want **version control** or rollback functionality.
- When implementing **non-destructive edits** in applications like editors or simulations.

---

## ✅ Pros
- **Preserves encapsulation** — internal details remain hidden from external classes.  
- Enables **undo/redo** and **rollback** mechanisms easily.  
- Provides **state snapshots** for debugging or auditing.  
- Promotes **Single Responsibility Principle** — Caretaker handles history, Originator focuses on logic.

---

## ❌ Cons
- Can cause **high memory usage** if many states are stored.  
- **Managing multiple mementos** (e.g., in deep history stacks) can be complex.  
- If object states are large, saving snapshots may **impact performance**.

---

## 🌍 Real-World Analogy
Think of the **"undo" feature in a text editor** — every time you make a change, the editor secretly takes a snapshot of the document (a Memento).  
When you press “Undo,” it restores the document to the previous snapshot.

---

## 📘 Related Patterns
- **Command** – often used together for undo/redo functionality.  
- **Prototype** – can clone objects instead of saving state snapshots.  
- **State** – focuses on changing behavior based on state, while Memento focuses on saving/restoring state.