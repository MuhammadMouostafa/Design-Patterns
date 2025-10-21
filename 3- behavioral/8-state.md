# 🔄 State Pattern (Behavioral)

## 📖 Category
**Behavioral Design Pattern**

---

## 🎯 Intent
Allow an object to **change its behavior dynamically** when its internal state changes.  
The object will appear to change its class at runtime.

---

## ⚙️ Problem
In many systems, objects behave differently depending on their internal state.  
A naïve solution is to use long chains of `if` or `switch` statements scattered throughout the code.

Issues:
- Adding new states requires modifying many places.  
- The logic for different states becomes **tangled** and hard to maintain.  
- Violates the **Open/Closed Principle** — every new state change modifies existing code.

Example:  
- A media player behaves differently when **Playing**, **Paused**, or **Stopped**.  
  Using `if` conditions everywhere creates messy code.

---

## 💡 Solution
- Create a **State interface** that defines the behavior associated with a particular state.  
- Implement multiple **Concrete States** for each distinct behavior.  
- The **Context** (main object) holds a reference to the current State and delegates behavior to it.  
- The State objects can change the Context’s state dynamically.

This removes conditionals and makes state transitions **explicit and extensible**.

---

## 🧩 Implementation in C++
```cpp
#include <iostream>
#include <memory>
using namespace std;

// Forward declaration
class State;

// ----- Context -----
class Player {
    unique_ptr<State> state;
public:
    Player(unique_ptr<State> initialState) : state(move(initialState)) {}
    void setState(unique_ptr<State> newState) { state = move(newState); }
    void pressPlay() { state->pressPlay(*this); }
};

// ----- State Interface -----
class State {
public:
    virtual void pressPlay(Player& player) = 0;
    virtual ~State() = default;
};

// ----- Concrete States -----
class PlayingState : public State {
public:
    void pressPlay(Player& player) override {
        cout << "⏸️  Pausing the music...\n";
        player.setState(make_unique<PausedState>());
    }
};

class PausedState : public State {
public:
    void pressPlay(Player& player) override {
        cout << "▶️  Resuming the music...\n";
        player.setState(make_unique<PlayingState>());
    }
};

// ----- Client Code -----
int main() {
    Player player(make_unique<PausedState>());

    player.pressPlay(); // Play
    player.pressPlay(); // Pause
    player.pressPlay(); // Play again
}
```

## 🧠 When to Use
- When an object's behavior depends on its current state, and it must change behavior at runtime
- When you want to avoid massive conditional statements for state transitions
- When you want each state to be independent and easily extendable

## ✅ Pros
- Eliminates complex if/switch logic scattered across methods
- Makes adding new states easy — no need to modify existing code
- Promotes Single Responsibility and Open/Closed principles
- Encapsulates state-specific behavior in separate classes

## ❌ Cons
- Increases the number of classes (one per state)
- Requires the Context to manage and coordinate state transitions
- May add overhead for simple state-based behavior

## 🌍 Real-World Analogy
- Think of a media player:
- When in Playing state → "Play" button acts as "Pause".
- When in Paused state → "Play" button resumes playback.
- The button's behavior changes automatically based on the player's current state.

## 📘 Related Patterns
- **Strategy** – similar structure but focuses on interchangeable algorithms, not state transitions
- **Memento** – can be used to save/restore states
- **Observer** – can notify external components when the state changes