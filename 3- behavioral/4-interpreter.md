# 🧩 Interpreter Pattern

## 📖 Category
**Behavioral Design Pattern**

---

## 🎯 Intent
Define a **grammar** for a simple language and use objects to **interpret sentences** in that language.  
Each rule in the grammar is represented by a **class**, and interpreting a sentence means executing these rules in sequence.

---

## ⚙️ Problem
You need to:
- Interpret expressions defined by a simple grammar (like mathematical or logical expressions),
- Avoid writing a large `if-else` or parser manually.

Example:
- You want to evaluate simple expressions such as `"a b + c *"` (postfix notation),
- Or interpret a rule-based language (e.g., filters, queries, or commands).

---

## 💡 Solution
- Define an **Abstract Expression** interface with an `Interpret()` method.  
- Implement **Terminal Expressions** (simple symbols or numbers).  
- Implement **Non-Terminal Expressions** (operators or combinations).  
- The **Client** builds a syntax tree that represents the sentence and interprets it.

---

## 🧩 Implementation in C++
```cpp
#include <iostream>
#include <memory>
#include <string>
#include <unordered_map>
using namespace std;

// ----- Abstract Expression -----
class Expression {
public:
    virtual int Interpret(unordered_map<string, int>& context) const = 0;
    virtual ~Expression() = default;
};

// ----- Terminal Expression -----
class VariableExpression : public Expression {
private:
    string name;
public:
    VariableExpression(const string& n) : name(n) {}
    int Interpret(unordered_map<string, int>& context) const override {
        return context[name];
    }
};

// ----- Non-Terminal Expressions -----
class AddExpression : public Expression {
private:
    shared_ptr<Expression> left;
    shared_ptr<Expression> right;
public:
    AddExpression(shared_ptr<Expression> l, shared_ptr<Expression> r)
        : left(l), right(r) {}
    int Interpret(unordered_map<string, int>& context) const override {
        return left->Interpret(context) + right->Interpret(context);
    }
};

class SubtractExpression : public Expression {
private:
    shared_ptr<Expression> left;
    shared_ptr<Expression> right;
public:
    SubtractExpression(shared_ptr<Expression> l, shared_ptr<Expression> r)
        : left(l), right(r) {}
    int Interpret(unordered_map<string, int>& context) const override {
        return left->Interpret(context) - right->Interpret(context);
    }
};

// ----- Client Code -----
int main() {
    // Expression: (a + b) - c
    auto a = make_shared<VariableExpression>("a");
    auto b = make_shared<VariableExpression>("b");
    auto c = make_shared<VariableExpression>("c");

    auto addition = make_shared<AddExpression>(a, b);
    auto expression = make_shared<SubtractExpression>(addition, c);

    unordered_map<string, int> context = { {"a", 5}, {"b", 2}, {"c", 3} };

    cout << "(a + b) - c = " << expression->Interpret(context) << endl;
    return 0;
}
```

## 🧠 When to Use
- When you need to evaluate expressions or commands defined by a grammar
- When your grammar is simple and unlikely to change frequently
- When you want to represent and interpret rules dynamically

## ✅ Pros
- Easy to extend grammar by adding new expression types
- Makes parsing and evaluation logic modular and object-oriented
- Provides a clear way to represent and execute simple languages

## ❌ Cons
- Becomes complex and inefficient for large grammars
- Hard to maintain if many grammar rules exist
- Not suitable for complex language parsing (use tools like ANTLR or Boost.Spirit instead)

## 🌍 Real-World Analogy
A calculator that understands simple formulas like (a + b) - c. Each operator and operand is a class that knows how to evaluate itself.

## 📘 Related Patterns
- **Composite** – the syntax tree structure often uses the Composite pattern
- **Iterator** – can be used to traverse the syntax tree
- **Visitor** – can be used to add new operations on the expression tree