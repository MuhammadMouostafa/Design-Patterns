# 🔁 Iterator Pattern

## 📖 Category
**Behavioral Design Pattern**

---

## 🎯 Intent
Provide a way to **access elements of a collection sequentially**  
**without exposing** its underlying representation (e.g., array, list, tree).

---

## ⚙️ Problem
You have a collection of elements, but:
- You don’t want clients to know how it’s implemented internally.
- You want to **traverse** it in a consistent and flexible way.

Example:
- A social network’s friend list might be stored differently (array, DB, tree),
  but you still want to iterate over friends uniformly.

---

## 💡 Solution
- Define an **Iterator interface** with methods like `Next()`, `HasNext()`, etc.  
- Each **Concrete Iterator** keeps track of the current position in the collection.  
- The **Collection** provides a factory method that returns an iterator.

---

## 🧩 Implementation in C++
```cpp
#include <iostream>
#include <vector>
#include <memory>
using namespace std;

// ----- Iterator Interface -----
template <typename T>
class Iterator {
public:
    virtual bool HasNext() const = 0;
    virtual T Next() = 0;
    virtual ~Iterator() = default;
};

// ----- Aggregate Interface -----
template <typename T>
class IterableCollection {
public:
    virtual shared_ptr<Iterator<T>> CreateIterator() const = 0;
    virtual ~IterableCollection() = default;
};

// ----- Concrete Collection -----
class NumberCollection : public IterableCollection<int> {
private:
    vector<int> numbers;
public:
    void Add(int num) { numbers.push_back(num); }

    class NumberIterator : public Iterator<int> {
    private:
        const vector<int>& data;
        size_t index;
    public:
        NumberIterator(const vector<int>& nums) : data(nums), index(0) {}
        bool HasNext() const override { return index < data.size(); }
        int Next() override { return data[index++]; }
    };

    shared_ptr<Iterator<int>> CreateIterator() const override {
        return make_shared<NumberIterator>(numbers);
    }
};

// ----- Client Code -----
int main() {
    NumberCollection collection;
    collection.Add(10);
    collection.Add(20);
    collection.Add(30);

    auto it = collection.CreateIterator();
    while (it->HasNext()) {
        cout << it->Next() << " ";
    }
    cout << endl;

    return 0;
}
```

## 🧠 When to Use
- When you want to traverse a collection without exposing its internal structure
- When you need multiple traversal algorithms for the same collection
- When you want uniform iteration across different types of collections

## ✅ Pros
- Promotes Single Responsibility Principle — traversal logic is separated from collection logic
- Makes it easy to add new types of iterators
- Provides uniform access to different data structures

## ❌ Cons
- Adds extra classes and complexity for simple collections
- External iterators can break encapsulation if misused

## 🌍 Real-World Analogy
A TV remote channel button: You can change channels one by one without knowing how the TV stores or manages them internally.

## 📘 Related Patterns
- **Composite** – often used with Iterator to traverse hierarchical structures
- **Factory Method** – can be used to create iterator instances
- **Memento** – can store iterator state for resuming traversal later