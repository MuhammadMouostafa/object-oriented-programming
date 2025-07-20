# Polymorphism in C++

Polymorphism, a core concept in object-oriented programming, allows entities such as functions or objects to take multiple forms. In C++, polymorphism enables us to perform a single action in different ways. There are two main types:

* **Compile-Time Polymorphism** (Static Binding)
* **Runtime Polymorphism** (Dynamic Binding)

---

## 1. Compile-Time Polymorphism

This form of polymorphism is resolved during compilation. It includes:

### Function Overloading

Using the same function name with different parameters.

```cpp
void print(int i) { std::cout << "Printing int: " << i << std::endl; }
void print(double d) { std::cout << "Printing double: " << d << std::endl; }
void print(std::string s) { std::cout << "Printing string: " << s << std::endl; }
```

### Operator Overloading

Redefining operators to work with user-defined types.

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

This type of polymorphism is resolved during program execution. It is achieved through **inheritance** and **method overriding**.

### The Problem Without Dynamic Binding

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

This is because the function call is resolved at compile-time based on the type of pointer (`Animal*`).

### Solving It with Virtual Functions

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
a->speak(); // Output: Dog barks
```

Now, the function call is resolved at runtime using the actual object type.


# 🔍 Virtual Table (vtable) and Virtual Pointer (vptr)

## 📚 Overview

In C++, polymorphism allows us to call derived class methods through base class pointers or references. This is made possible by two key mechanisms:

* **Virtual Table (vtable)**: A lookup table for virtual functions.
* **Virtual Pointer (vptr)**: A hidden pointer in each object pointing to the vtable of its actual (runtime) class.

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
### 👇 Sample Usage

```cpp
A* obj1 = new C();
obj1->f1();   // ✅ virtual → dynamic dispatch → C::f1
obj1->f2();   // ❌ non-virtual in A → static dispatch → A::f2
obj1->f3();   // ❌ non-virtual in A → static dispatch → A::f3

A* obj2 = new B();
obj2->f1();   // ✅ virtual in A → dynamic dispatch → B::f1
obj2->f2();   // ❌ non-virtual in A → static dispatch → A::f2
obj2->f3();   // ❌ non-virtual in A → static dispatch → A::f3

B* obj3 = new C();
obj3->f1();   // ✅ virtual in A → dynamic dispatch → C::f1
obj3->f2();   // ✅ virtual in B → dynamic dispatch → C::f2
obj3->f3();   // ❌ non-virtual in B → static dispatch → B::f3
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
+------------------+         +--------------------+
                           | vtable for class C  |
                           +--------------------+
                           | C::f1               |
                           | C::f2               |
                           | C::f3               |
                           +--------------------+
```

---

## 📊 Examble Summary Table

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

### 🔥 The Problem Without Virtual Destructors

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

### ✅ Fix: Use a Virtual Destructor in the Base

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


### Pure Virtual Functions and Abstract Classes

```cpp
class Shape {
public:
    virtual void draw() = 0; // Pure virtual
};

class Circle : public Shape {
public:
    void draw() override {
        std::cout << "Drawing Circle" << std::endl;
    }
};
```
---

## `override`, `final`, `virtual` Keywords

* `virtual`: Declares a function as virtual.
* `override`: Ensures you're correctly overriding a virtual function.
* `final`: Prevents further overriding.

```cpp
class A {
public:
    virtual void func() {}
};

class B : public A {
public:
    void func() override final {}
};
```

---

## Dynamic Cast and RTTI

Allows casting base pointers to derived types safely at runtime:

```cpp
Animal* a = new Dog();
Dog* d = dynamic_cast<Dog*>(a);
```

Use `typeid` to inspect types at runtime:

```cpp
if (typeid(*a) == typeid(Dog)) {
    std::cout << "It is a Dog";
}
```


