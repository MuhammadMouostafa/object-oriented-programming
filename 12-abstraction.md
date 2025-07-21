# Abstraction in C++

Abstraction is a **core principle of object-oriented programming (OOP)** that allows developers to manage complexity by **modeling real-world entities** and exposing only relevant details to the outside world. It helps in building systems that are **easier to understand, extend, and maintain**.

---

## ✅ What is Abstraction?

**Abstraction** is the process of hiding implementation details and exposing only the necessary interface to the user.

In simpler terms:

> **"Show what an object does, not how it does it."**

---

## 🎯 Key Goals of Abstraction

* Hide complexity
* Protect internal object state
* Provide clear and minimal interfaces
* Reduce code duplication
* Improve modularity and reusability

---

## 🧱 How Abstraction is Achieved in C++

C++ supports abstraction using two main tools:

1. **Abstract Classes**
2. **Access Modifiers (public, protected, private)**

---

## 1. 🔷 Abstract Classes

An **abstract class** is a class that contains **at least one pure virtual function**. It **cannot be instantiated directly**, and its purpose is to be inherited.

### 📌 Syntax:

```cpp
class AbstractClass {
public:
    virtual void doSomething() = 0; // Pure virtual function
};
```

### 🧪 Example:

```cpp
#include <iostream>
using namespace std;

class Shape {
public:
    virtual void draw() = 0;  // Pure virtual function
};

class Circle : public Shape {
public:
    void draw() override {
        cout << "Drawing Circle\n";
    }
};

class Rectangle : public Shape {
public:
    void draw() override {
        cout << "Drawing Rectangle\n";
    }
};

int main() {
    Shape* s1 = new Circle();
    Shape* s2 = new Rectangle();

    s1->draw();  // Output: Drawing Circle
    s2->draw();  // Output: Drawing Rectangle

    delete s1;
    delete s2;
}
```

---

## 2. 🔒 Access Specifiers in Abstraction

Abstraction is also supported using **access control**:

| Access Specifier | Who Can Access?                               |
| ---------------- | --------------------------------------------- |
| `public`         | Anyone                                        |
| `protected`      | Derived classes and members of the same class |
| `private`        | Only members of the same class                |

By making implementation details `private` or `protected`, and exposing only necessary functionality as `public`, we enforce abstraction.

---

## 📚 Full vs Partial Abstraction

| Type                    | Description                                    | Example                          |
| ----------------------- | ---------------------------------------------- | -------------------------------- |
| **Full Abstraction**    | Only pure virtual functions, no implementation | Interface-style abstract class   |
| **Partial Abstraction** | Mix of implemented and pure virtual methods    | Base classes with reusable logic |

```cpp
class FullAbstract {
public:
    virtual void f1() = 0;
    virtual void f2() = 0;
};

class PartialAbstract {
public:
    void commonLogic() {
        // Shared implementation
    }
    virtual void specificTask() = 0;
};
```

---

## 🔄 Abstraction vs Encapsulation

| Feature     | Abstraction                          | Encapsulation                   |
| ----------- | ------------------------------------ | ------------------------------- |
| Purpose     | Hides implementation complexity      | Protects internal object state  |
| Achieved by | Abstract classes, interfaces         | Private fields + public methods |
| Focus       | **What** the object does             | **How** the object stores data  |
| Visibility  | Hides unwanted details from the user | Restricts direct access to data |

*Both work together to improve software design.*

---

## 🌍 Real-World Analogy

Think of a **TV remote**:

* You can press a button to turn on/off the TV — that’s the *interface*.
* You **don’t need to know** how the remote sends infrared signals to the TV — that’s the *implementation*, which is **abstracted** away.

---

## 👷 Best Practices for Abstraction in C++

* Favor composition over inheritance where possible
* Use interfaces (pure abstract classes) to define **contracts**
* Expose only what's necessary in the public API
* Keep implementation details hidden and loosely coupled
* Document abstract interfaces for better maintainability

---

## 📌 When to Use Abstraction

* Designing frameworks or libraries
* Building large applications with reusable components
* Working with systems where behavior must vary between components (e.g., rendering different shapes, processing different payment methods)

---

## ✅ Summary

| Term                | Meaning                                                           |
| ------------------- | ----------------------------------------------------------------- |
| Abstraction         | Hide internal implementation and show only behavior               |
| Abstract Class      | A class with at least one pure virtual method                     |
| Pure Virtual Method | `virtual void foo() = 0;`                                         |
| Interface           | Fully abstract class (C++ doesn't have true interfaces like Java) |
| Access Modifiers    | Control visibility and support data hiding                        |

---

## 📘 Final Thought

Abstraction helps you **focus on the "what", not the "how"** — enabling better design, testability, and flexibility in complex systems.
