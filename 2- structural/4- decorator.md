# 🎀 Decorator Pattern
## 📖 Category

Structural Design Pattern

## 🎯 Intent

Attach additional responsibilities to an object dynamically.
Decorators provide a flexible alternative to subclassing for extending functionality.

## ⚙️ Problem

Sometimes you want to add features to objects (like logging, encryption, compression)
without modifying their code or creating endless subclasses.

Example:

- You have a DataSource interface with a writeData() method.

- You want to add encryption or compression to it only when needed,
without touching the base class.

## 💡 Solution

- Define a Component interface that defines the core behavior.

- Create Concrete Components that implement it.

- Create a Decorator base class that implements the same interface and wraps a component.

- Concrete decorators add extra behavior before or after delegating calls to the wrapped object.

## 🧩 Implementation in C++

```cpp
#include <iostream>
#include <memory>
#include <string>
using namespace std;

// ----- Component Interface -----
class DataSource {
public:
    virtual void WriteData(const string& data) = 0;
    virtual string ReadData() = 0;
    virtual ~DataSource() = default;
};

// ----- Concrete Component -----
class FileDataSource : public DataSource {
private:
    string filename;
    string content;
public:
    FileDataSource(const string& file) : filename(file) {}
    void WriteData(const string& data) override {
        content = data;
        cout << "Writing to file " << filename << ": " << content << endl;
    }
    string ReadData() override {
        cout << "Reading from file " << filename << endl;
        return content;
    }
};

// ----- Base Decorator -----
class DataSourceDecorator : public DataSource {
protected:
    shared_ptr<DataSource> wrappee;
public:
    DataSourceDecorator(shared_ptr<DataSource> source) : wrappee(source) {}
    void WriteData(const string& data) override {
        wrappee->WriteData(data);
    }
    string ReadData() override {
        return wrappee->ReadData();
    }
};

// ----- Concrete Decorators -----
class EncryptionDecorator : public DataSourceDecorator {
public:
    EncryptionDecorator(shared_ptr<DataSource> source) : DataSourceDecorator(source) {}
    void WriteData(const string& data) override {
        string encrypted = "[Encrypted]" + data;
        cout << "Encrypting data...\n";
        wrappee->WriteData(encrypted);
    }
    string ReadData() override {
        string data = wrappee->ReadData();
        cout << "Decrypting data...\n";
        return data.substr(11); // remove "[Encrypted]"
    }
};

class CompressionDecorator : public DataSourceDecorator {
public:
    CompressionDecorator(shared_ptr<DataSource> source) : DataSourceDecorator(source) {}
    void WriteData(const string& data) override {
        string compressed = "[Compressed]" + data;
        cout << "Compressing data...\n";
        wrappee->WriteData(compressed);
    }
    string ReadData() override {
        string data = wrappee->ReadData();
        cout << "Decompressing data...\n";
        return data.substr(12); // remove "[Compressed]"
    }
};

// ----- Client Code -----
int main() {
    auto file = make_shared<FileDataSource>("data.txt");
    auto encrypted = make_shared<EncryptionDecorator>(file);
    auto compressed = make_shared<CompressionDecorator>(encrypted);

    cout << "\n--- Writing Process ---\n";
    compressed->WriteData("Hello, World!");

    cout << "\n--- Reading Process ---\n";
    cout << "Final data: " << compressed->ReadData() << endl;
}
```

## 🧠 When to Use

- When you need to add or remove responsibilities at runtime.

- When subclassing would create too many combinations.

- When you want to follow the Open/Closed Principle (open for extension, closed for modification).

## ✅ Pros

- Flexible and reusable extension mechanism.

- Avoids deep inheritance trees.

- Combine multiple decorators dynamically.

## ❌ Cons

- Can make debugging more difficult (many layers).

- Order of decorators matters and may affect behavior.

## 🌍 Real-World Analogy

Think of a coffee order:

- You start with plain coffee (base component).

- Add milk, sugar, or whipped cream (decorators).
Each add-on wraps the original coffee and extends its behavior.

## 📘 Related Patterns

- Composite – both use recursive composition, but Composite combines objects, while Decorator adds behavior.

- Adapter – changes an interface; Decorator enhances functionality.

- Proxy – controls access, not adds behavior.