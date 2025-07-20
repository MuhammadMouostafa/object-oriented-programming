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


# 🔍 Understanding Virtual Table (vtable) and Virtual Pointer (vptr)

## 🔸 Concept Overview
When a class contains at least one virtual function, the compiler sets up a mechanism to support runtime polymorphism. This mechanism involves: **vtable** (Virtual Table) and **vptr** (Virtual Pointer)

## 🧱 Class Hierarchy Example

```cpp
class A {
public:
    virtual void f1() { std::cout << "A::f1\n"; }
    void f2()         { std::cout << "A::f2\n"; }
    void f3()         { std::cout << "A::f3\n"; }
};

class B : public A {
public:
    void f1() override { std::cout << "B::f1\n"; }
    virtual void f2()  { std::cout << "B::f2\n"; }
    void f3()          { std::cout << "B::f3\n"; }
};

class C : public B {
public:
    void f1() override { std::cout << "C::f1\n"; }
    void f2() override { std::cout << "C::f2\n"; }
    virtual void f3()  { std::cout << "C::f3\n"; }
};

class D : public C {
public:
    void f1() override { std::cout << "D::f1\n"; }
    void f2() override { std::cout << "D::f2\n"; }
    void f3() override { std::cout << "D::f3\n"; }
};

int main() {
    A* obj = new C();
    obj->f1(); // C::f1 dynamic binding (virtual)
    obj->f2(); // A::f2 static binding (non-virtual in A)
    obj->f3(); // A::f3 static binding (non-virtual in A)

    B* obj2 = new D();
    obj2->f1(); // D::f1 dynamic binding (virtual dispatch)
    obj2->f2(); // D::f2 dynamic binding (virtual dispatch)
    obj2->f3(); // B::f3 static binding (non-virtual in B)
}
```

## 🧠 What is a Virtual Table (vtable)?

A **vtable** is a lookup table maintained per class that contains addresses of the virtual functions that objects of that class can call.

* Every class with **at least one virtual function** gets its own vtable.
* Each entry in the vtable is a function pointer to the most derived implementation of a virtual function.

### 🧾 For This Example:

* `A` has a vtable: `[ &A::f1 ]`
* `B` has a vtable: `[ &B::f1, &B::f2 ]`
* `C` has a vtable: `[ &C::f1, &C::f2, &C::f3 ]`
* `D` has a vtable: `[ &D::f1, &D::f2, &D::f3 ]`

Each vtable is generated **at compile time** and shared across all instances of a class at runtime.

## 📌 What is a Virtual Pointer (vptr)?

A **vptr** is a hidden pointer inside each object that points to the vtable of its actual class.

* It is automatically set up by the constructor of the object.
* It ensures that the correct vtable is used based on the actual object type (not the pointer type).

### Example:

```cpp
A* obj1 = new C();
```

Here:

* The actual object is of type `C`, so `obj1->vptr` points to `C`'s vtable.
* This enables `obj1->f1()` to dynamically dispatch to `C::f1()` even though `obj1` is of type `A*`.

### Another Example:

```cpp
B* obj2 = new D();
```

* The actual object is of type `D`, so `obj2->vptr` points to `D`'s vtable.
* This enables dynamic dispatch to `D::f1()` and `D::f2()` because both are virtual in base classes.

## 🧩 Summary

| Object | Static Type | Dynamic Type | vptr Points To | f1()      | f2()      | f3()      |
| ------ | ----------- | ------------ | -------------- | --------- | --------- | --------- |
| `obj1` | `A*`        | `C`          | `C`'s vtable   | `C::f1()` | `A::f2()` | `A::f3()` |
| `obj2` | `B*`        | `D`          | `D`'s vtable   | `D::f1()` | `D::f2()` | `B::f3()` |

✅ Only virtual functions participate in dynamic dispatch via vtable.
❌ Non-virtual functions are resolved at compile time (static binding).





### Virtual Destructors

Ensures proper cleanup when deleting derived objects via base pointers.

```cpp
class Base {
public:
    virtual ~Base() {
        std::cout << "Base destructor" << std::endl;
    }
};
```

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

## 3. vtable and vptr

To support dynamic dispatch, C++ compilers use:

* **vtable**: A table of function pointers created per class with virtual functions.
* **vptr**: A hidden pointer in each object instance pointing to its class's vtable.

When a virtual method is called, the call is resolved through the vptr and vtable at runtime.

---

## 4. `override`, `final`, `virtual` Keywords

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

## 5. Dynamic Cast and RTTI

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

---

## Summary

Polymorphism enables flexible and extensible code. Core components include:

* Overloading (compile-time)
* Virtual functions and inheritance (runtime)
* Abstract classes and pure virtual methods
* vtable and vptr mechanics
* Proper destructor behavior
* Runtime type checks (dynamic\_cast, typeid)

Mastering polymorphism allows you to design reusable, maintainable, and scalable software systems.



