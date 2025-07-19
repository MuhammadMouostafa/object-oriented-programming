# 🧬 Inheritance in C++

Inheritance is one of the core principles of object-oriented programming (OOP). It allows a class to inherit properties and behaviors (variables and functions) from another class. This promotes code reuse and logical hierarchy.

---

## 📘 What is Inheritance?

Inheritance is a mechanism that allows one class (the **child** or **derived class**) to inherit features (members and methods) from another class (the **parent** or **base class**).

### ✅ Benefits

* Code reuse
* Logical classification
* Extensibility
* Easy maintenance

---

## 🧱 Syntax of Inheritance

```cpp
class Base {
public:
    void show() {
        std::cout << "Base class function" << std::endl;
    }
};

class Derived : public Base {
    // inherits Base class publicly
};

int main() {
    Derived d;
    d.show(); // Output: Base class function
    return 0;
}
```

---

## 🔐 Access Specifiers in Inheritance

| Base Member | Public Inheritance | Protected Inheritance | Private Inheritance |
|-------------|--------------------|------------------------|---------------------|
| `public`    | `public`           | `protected`            | `private`           |
| `protected` | `protected`        | `protected`            | `private`           |
| `private`   | *not accessible*   | *not accessible*       | *not accessible*    |

> Private members of base class are **not directly accessible** in derived classes, but they are inherited.

---

## 🔁 Overriding Inherited Methods

If the base class has a method that the derived class wants to modify, it can override it:

```cpp
class Animal {
public:
    void sound() {
        std::cout << "Animal makes sound" << std::endl;
    }
};

class Dog : public Animal {
public:
    void sound() { // overrides Animal::sound
        std::cout << "Dog barks" << std::endl;
    }
};

int main() {
    Dog d;
    d.sound(); // Output: Dog barks
    return 0;
}
```

Note: This is **not** polymorphism unless used with pointers/references and `virtual` keyword, which will be covered in a separate lesson.

---

## 🧬 Types of Inheritance

![](/assets/images/types-of-inheritance.png)

### 1. Single Inheritance

```cpp
class A { /*...*/ };
class B : public A { /*...*/ }; // B inherits A
```

### 2. Multilevel Inheritance

```cpp
class A { /*...*/ };
class B : public A { /*...*/ };
class C : public B { /*...*/ }; // C inherits B and A
```

### 3. Multiple Inheritance

```cpp
class A { /*...*/ };
class B { /*...*/ };
class C : public A, public B { /*...*/ }; // C inherits A and B
```

### 4. Hierarchical Inheritance

```cpp
class A { /*...*/ };
class B : public A { /*...*/ };
class C : public A { /*...*/ }; // B and C inherit A
```

### 5. Hypered Inheritance (Diamond Problem & Virtual Inheritance)
**Hypered Inheritance** is a term used to describe complex inheritance structures that combine both **multiple** and **hierarchical** inheritance. This often leads to the **Diamond Problem**, where a class indirectly inherits the same base class through multiple paths, causing ambiguity and duplication.


🔸 The Diamond Problem

```
      A
     / \
    B   C
     \ /
      D
```


* `B` and `C` both inherit from `A`
* `D` inherits from both `B` and `C`
* Without `virtual` inheritance, `D` has two copies of `A`

❌ Without Virtual Inheritance

```cpp
#include <iostream>
using namespace std;

class A {
public:
    void sayHi() { cout << "Hello from A\n"; }
};

class B : public A {};
class C : public A {};
class D : public B, public C {}; // Diamond problem

int main() {
    D obj;
    // obj.sayHi(); ❌ Ambiguous
    obj.B::sayHi(); // ✅ Must specify path
}
```
Without **virtual inheritance**, class `D` would inherit two copies of `A` (one from `B`, one from `C`), causing ambiguity when accessing members of `A`:



🔷 Solution: Virtual Inheritance
Virtual inheritance solves this problem by ensuring that only one shared base class subobject exists, even if inherited multiple times.

✅ With Virtual Inheritance

```cpp
#include <iostream>
using namespace std;

class A {
public:
    void sayHi() { cout << "Hello from A\n"; }
};

class B : virtual public A {};
class C : virtual public A {};
class D : public B, public C {}; // Solves diamond issue

int main() {
    D obj;
    obj.sayHi(); // ✅ Only one A exists
}
```
By using the `virtual` keyword in the inheritance of `B` and `C`, both share the same instance of `A`, solving the ambiguity:

### Key Points:

🧩 **Hypered inheritance** leads to the **diamond problem** in complex inheritance trees.

🔐 **Virtual inheritance** is used to prevent duplication and ambiguity.

⚠️ Use **virtual inheritance** only when necessary, as it adds complexity to object construction and layout.

---


## 🚫 Private Members Are Still Inherited

Even though a derived class cannot access private members directly, they are still inherited. You can access them using **public/protected getters and setters**.

```cpp
class Base {
private:
    int value;
public:
    void setValue(int v) { value = v; }
    int getValue() { return value; }
};

class Derived : public Base {
    // Cannot access value directly, but can use get/set
};
```

---

## 📌 Summary

* Inheritance enables one class to acquire properties of another.
* Use **public inheritance** for "is-a" relationships.
* You can override methods to customize inherited behavior.
* Know the impact of `public`, `protected`, and `private` inheritance.
* Different types include single, multiple, multilevel, and hierarchical.
