# Polymorphism in C++

Polymorphism is a fundamental concept in object-oriented programming that allows objects or functions to take on multiple forms. In C++, polymorphism provides flexibility and reusability by allowing a single interface to be used for different data types or implementations.

There are two main types:

* **Compile-Time Polymorphism** (Static Binding)
* **Runtime Polymorphism** (Dynamic Binding)

---

## 1. Compile-Time Polymorphism

This form of polymorphism is resolved at **compile-time**. It includes:

### 🔄 Function Overloading

Multiple functions with the same name but different parameter types or counts.

```cpp
void print(int i) { std::cout << "Printing int: " << i << std::endl; }
void print(double d) { std::cout << "Printing double: " << d << std::endl; }
void print(std::string s) { std::cout << "Printing string: " << s << std::endl; }
```

### ➕ Operator Overloading

Customizing operators to work with user-defined types.

```cpp
class Point {
public:
    int x, y;
    Point(int x, int y) : x(x), y(y) {}

    Point operator+(const Point& p) {
        return Point(x + p.x, y + p.y);
    }
};
```

---

## 2. Runtime Polymorphism

This type of polymorphism is resolved **during program execution** using inheritance and virtual functions.

### ❌ Problem Without Virtual Functions
When we use base class pointers or references to refer to derived class objects, and we call methods that are redefined in derived classes **without** marking them as virtual in the base class, static binding occurs:

```cpp
class Animal {
public:
    void speak() { std::cout << "Animal speaks" << std::endl; }
};

class Dog : public Animal {
public:
    void speak() { std::cout << "Dog barks" << std::endl; }
};

Animal* a = new Dog();
a->speak(); // Output: Animal speaks
```

The function call is resolved at compile-time based on the type of pointer (Animal*) because speak() is not marked as virtual, resulting in static dispatch.

### ✅ Solution Using Virtual Functions

By marking the base class method as `virtual`, we tell the compiler to enable dynamic dispatch.

### 🔁 Virtual Functions
A virtual function is a member function marked with the virtual keyword in the base class. It allows derived classes to override behavior. When accessed through a base class pointer or reference, the overridden method in the derived class is executed.


```cpp
class Animal {
public:
    virtual void speak() { std::cout << "Animal speaks" << std::endl; }
};

class Dog : public Animal {
public:
    void speak() override { std::cout << "Dog barks" << std::endl; }
};

Animal* a = new Dog();
a->speak(); // Output: Dog barks (dynamic dispatch)
```

Now, the function call is resolved at runtime using the actual object type.

---

# 🔍 Virtual Table (vtable) and Virtual Pointer (vptr)

## 📚 Overview

In C++, polymorphism allows us to call derived class methods through base class pointers or references. This is made possible by two key mechanisms:

* **vtable**: A lookup table containing addresses of virtual functions for a class.
* **vptr**: A hidden pointer in each object pointing to the vtable of the actual class.

---

## 🧱 Class Hierarchy Example

```cpp
#include <iostream>
using namespace std;

class A {
public:
    virtual void f1() { cout << "A::f1\n"; }
    void f2()          { cout << "A::f2\n"; }
    void f3()          { cout << "A::f3\n"; }
};

class B : public A {
public:
    void f1() override { cout << "B::f1\n"; }  // Overrides A::f1 (still virtual)
    virtual void f2()  { cout << "B::f2\n"; }  // Introduces virtual f2
    void f3()          { cout << "B::f3\n"; }
};

class C : public B {
public:
    void f1() override { cout << "C::f1\n"; }  // Overrides B::f1 (virtual)
    void f2() override { cout << "C::f2\n"; }  // Overrides B::f2 (virtual)
    virtual void f3()  { cout << "C::f3\n"; }  // Introduces virtual f3
};
```
## 👇 Usage and Behavior

```cpp
A* obj1 = new C();
obj1->f1();   // ✅ C::f1 (virtual - dynamic dispatch)
obj1->f2();   // ❌ A::f2 (non-virtual - static dispatch)
obj1->f3();   // ❌ A::f3 (non-virtual - static dispatch)

A* obj2 = new B();
obj2->f1();   // ✅ B::f1 (virtual - dynamic dispatch)
obj2->f2();   // ❌ A::f2 (non-virtual - static dispatch)
obj2->f3();   // ❌ A::f3 (non-virtual - static dispatch)

B* obj3 = new C();
obj3->f1();   // ✅ C::f1 (virtual from A - dynamic dispatch)
obj3->f2();   // ✅ C::f2 (virtual from B - dynamic dispatch)
obj3->f3();   // ❌ B::f3 (non-virtual in B - static dispatch)
```

---

## 🧠 Virtual Table (vtable)

* The **vtable** is a table maintained by the compiler.
* It stores addresses of the most derived (runtime) implementations of virtual functions.
* Each class with at least one virtual function has its own vtable.
* It is **shared** by all instances of the same class.

### 📌 vtable for Each Class

| Class | vtable Contents     |
| ----- | ------------------- |
| A     | A::f1               |
| B     | B::f1, B::f2        |
| C     | C::f1, C::f2, C::f3 |

## 🧷 Virtual Pointer (vptr)

* The **vptr** is a hidden pointer inside every object of a class with virtual functions.
* It points to the vtable of the actual class of the object (not necessarily the pointer type).
* Automatically set in the constructor of the most derived class.

---

## 🔄 Static vs Dynamic Dispatch

When calling a method through a base class pointer:

1. **Check if the method is `virtual` in the pointer's static type (e.g., `Base*`)**:

   * ✅ If **yes** → Use **vptr → vtable** to dispatch dynamically.
   * ❌ If **no** → Call base method directly (**static dispatch**).

### 👁️ In Our Example

* `obj1->f1()` → `A::f1` is virtual → vptr → C::f1 is called ✅
* `obj1->f2()` → `A::f2` is not virtual → A::f2 is called ❌
* `obj1->f3()` → `A::f3` is not virtual → A::f3 is called ❌

---

## 🧮 Memory Layout Visualization

Let's visualize `A* obj = new C();`

```
+------------------+
| vptr ----------> |-------------------+
|                  |                   |
|  data members    |                   v
+------------------+       +---------------------+
                           | vtable for class C  |
                           +---------------------+
                           | C::f1               |
                           | C::f2               |
                           | C::f3               |
                           +---------------------+
```

---

## 🧠 Virtual Dispatch Table Summary

| Object | Function | Virtual in Base? | Dispatch Type | Function Called | vtable Used |
| ------ | -------- | ---------------- | ------------- | --------------- | ----------- |
| `obj1` | `f1()`   | Yes (in A)       | Dynamic       | `C::f1()`       | `C::vtable` |
| `obj1` | `f2()`   | No (in A)        | Static        | `A::f2()`       | N/A         |
| `obj1` | `f3()`   | No (in A)        | Static        | `A::f3()`       | N/A         |
| `obj2` | `f1()`   | Yes (in A)       | Dynamic       | `B::f1()`       | `B::vtable` |
| `obj2` | `f2()`   | No (in A)        | Static        | `A::f2()`       | N/A         |
| `obj2` | `f3()`   | No (in A)        | Static        | `A::f3()`       | N/A         |
| `obj3` | `f1()`   | Yes (in A)       | Dynamic       | `C::f1()`       | `C::vtable` |
| `obj3` | `f2()`   | Yes (in B)       | Dynamic       | `C::f2()`       | `C::vtable` |
| `obj3` | `f3()`   | No (in B)        | Static        | `B::f3()`       | N/A         |

---

## ✅ Summary

* Virtual functions enable **runtime polymorphism**.
* vtable holds function pointers to the most derived implementations.
* vptr is per-object and set during construction.
* If a function is **not** virtual in the pointer type, it uses **static dispatch**.
* If it's **virtual**, then dynamic dispatch happens via **vtable lookup** using **vptr**.

This mechanism is fundamental to C++'s object-oriented power!

---
## 🧹 Virtual Destructors in C++

### ❌ Problem Without Virtual Destructor

In C++, when you delete an object through a pointer to its **base class**, the destructor that's actually called depends on whether the base class destructor is marked `virtual` or not.

If it's **not virtual**, **only the base class destructor is executed**, even if the object is actually of a derived class. This causes:

* 🔁 Derived class cleanup is **skipped**
* 🧠 Resource **leaks** (memory, file handles, etc.)
* ❌ Violated polymorphic behavior

#### ❌ Example: Memory Leak Without Virtual Destructor

```cpp
#include <iostream>

class Base {
public:
    ~Base() {
        std::cout << "Base destructor\n";
    }
};

class Derived : public Base {
private:
    int* data;
public:
    Derived() {
        data = new int[100];  // allocate memory
        std::cout << "Derived constructor\n";
    }

    ~Derived() {
        delete[] data;  // cleanup memory
        std::cout << "Derived destructor\n";
    }
};

int main() {
    Base* ptr = new Derived();  // actually a Derived object
    delete ptr;  // Only Base destructor is called!
}
```

**Output:**

```
Derived constructor
Base destructor
```

> ❌ `Derived::~Derived()` was **never called**, so the allocated memory is leaked!

---

### ✅ Solution with Virtual Destructor

To fix this, we declare the base destructor as `virtual`. This ensures the **correct destructor chain** is called, starting from the derived class down to the base.

#### ✅ Example: Safe Cleanup with Virtual Destructor

```cpp
#include <iostream>

class Base {
public:
    virtual ~Base() {
        std::cout << "Base destructor\n";
    }
};

class Derived : public Base {
private:
    int* data;
public:
    Derived() {
        data = new int[100];
        std::cout << "Derived constructor\n";
    }

    ~Derived() {
        delete[] data;
        std::cout << "Derived destructor\n";
    }
};

int main() {
    Base* ptr = new Derived();
    delete ptr;  // Correctly calls both destructors
}
```

**Output:**

```
Derived constructor
Derived destructor
Base destructor
```

> ✅ Now the memory is safely released. The destruction is correct and complete.

---

### 🧠 When to Use Virtual Destructors?

* Always use a **virtual destructor** in a base class if you expect **polymorphic deletion**.
* It protects against bugs that may not crash your program but can silently cause memory/resource leaks.

---
# `virtual`, `override`, and `final` Keywords in C++

C++ uses the `virtual`, `override`, and `final` keywords to support polymorphism and control function overriding in inheritance hierarchies. Here's a breakdown of what each keyword does, followed by detailed examples.

## `virtual` Keyword

The `virtual` keyword is used in base classes to declare functions that can be overridden in derived classes. It enables **runtime polymorphism** (dynamic dispatch), allowing the correct function to be called based on the actual object type.

## `override` Keyword

The `override` keyword is used in derived classes to indicate that a virtual function from the base class is being overridden. It helps catch mistakes, like mismatched function signatures.

### Example:
```cpp
#include <iostream>
using namespace std;

class Shape {
public:
    virtual void draw() const {
        cout << "Drawing shape" << endl;
    }

    void move() const {
        cout << "Moving shape" << endl;
    }
};

class Circle : public Shape {
public:
    void draw() const override {  // ✅ Correctly overrides
        cout << "Drawing circle" << endl;
    }

    // ❌ Invalid override - 'move' is not virtual in base class
    // void move() override {
    //     cout << "Invalid override attempt" << endl;
    // }
};

int main() {
    Shape* s = new Circle();
    s->draw();  // Output: Drawing circle
    s->move();  // Output: Moving shape
    delete s;
    return 0;
}

```

## `final` Keyword

The `final` keyword in C++ can be used in two ways:

- **With a virtual function**: Prevents the function from being overridden in any derived class.
- **With a class**: Prevents the class from being inherited at all.

### Function-level `final` Example:
```cpp
#include <iostream>
using namespace std;

class Printer {
public:
    virtual void print() final {  // ❌ final here prevents any override
        cout << "Printing from base" << endl;
    }
};

class PDFPrinter : public Printer {
public:
    // ❌ Error: print is marked as final in base class
    // void print() override {
    //     cout << "Trying to override final method" << endl;
    // }
};

```

### Class-level `final` Example:
```cpp
class NonInheritable final {
public:
    void display() {}
};

// class SubClass : public NonInheritable { };  // ❌ Error: class is final
```
---

## 🧠 Dynamic Cast and RTTI (Run-Time Type Information)

When using **polymorphism** in C++, we often use **base class pointers** to point to **derived class objects**. But sometimes we need to safely check or access the actual type of the object. That's where `dynamic_cast` and `typeid` are useful.
> `dynamic_cast` and `typeid` are **Operators**.
### 🧩 What is RTTI?

RTTI (Run-Time Type Information) allows checking the **actual type** of an object at **runtime**.  
It only works if the base class has at least one `virtual` function (i.e., it's polymorphic).

---

### 🔄 `dynamic_cast`: Safe Downcasting

`dynamic_cast` tries to convert a pointer/reference of a base class to a derived class **safely**.

### ✅ Example:

```cpp
#include <iostream>
using namespace std;

class Animal {
public:
    virtual void speak() { cout << "Animal sound\n"; }
};

class Dog : public Animal {
public:
    void speak() override { cout << "Woof!\n"; }
    void fetch() { cout << "Dog fetching ball!\n"; }
};

int main() {
    Animal* a = new Dog();  // Base pointer to derived object

    Dog* d = dynamic_cast<Dog*>(a);  // Try to cast to Dog

    if (d) {
        d->fetch();  // Safe to call Dog-specific method
    } else {
        cout << "Not a Dog\n";
    }

    delete a;
}
```

### ⚠️ If the cast fails

```cpp
Animal* a = new Animal();
Dog* d = dynamic_cast<Dog*>(a);  // Returns nullptr
```

---

### 🕵️ `typeid`: Check the Actual Type

`typeid` gets the **real type** of an object at runtime.

### ✅ Example:

```cpp
#include <iostream>
#include <typeinfo>
using namespace std;

class Animal {
public:
    virtual ~Animal() {}
};

class Dog : public Animal {};

int main() {
    Animal* a = new Dog();

    if (typeid(*a) == typeid(Dog)) {
        cout << "It is a Dog\n";
    } else {
        cout << "It is NOT a Dog\n";
    }

    delete a;
}
```

---

### 🎯 How They Help With Polymorphism

- In polymorphism, we use base pointers to work with derived objects.
- But to access derived-specific methods or check the exact type, we use:
  - `dynamic_cast` to **safely convert** back to derived class.
  - `typeid` to **check the real type** of the object.

---

### ✅ Summary Table

| Feature        | Purpose                              | Requires Virtual? | Returns         |
|----------------|---------------------------------------|-------------------|-----------------|
| `dynamic_cast` | Safely cast base to derived           | ✅ Yes            | Pointer or nullptr |
| `typeid`       | Get the actual object type at runtime | ✅ Yes            | `type_info` object |
