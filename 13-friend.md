## 📘 `friend` Keyword (Function and Class)

### 🔹 What is `friend` in C++?

In C++, the `friend` keyword allows a function or another class to access **private** and **protected** members of a class, **breaking encapsulation intentionally** for specific trusted components.

---

### 🔸 1. Friend Function

#### ✅ Definition

A **non-member** function can be declared as a friend inside a class to allow it access to private/protected data.

#### 🧪 Example:

```cpp
#include <iostream>
using namespace std;

class Box {
private:
    int length;

public:
    Box(int l) : length(l) {}

    // Declare friend function
    friend void printLength(Box b);
};

// Can access private members of Box
void printLength(Box b) {
    cout << "Length is: " << b.length << endl;
}
```

#### 📝 Key Points

* `printLength()` is **not a member** of `Box`, but it can access `length`.
* Friendship is **not mutual** or **inherited**.

---

### 🔸 2. Friend Function (Member of Another Class)

You can make a member function of another class a friend:

```cpp
class B;  // Forward declaration

class A {
private:
    int x = 10;
    friend void B::showA(A&); // Grant access to a specific method in B
};

class B {
public:
    void showA(A& a) {
        cout << "A::x = " << a.x << endl;
    }
};
```

---

### 🔸 3. Friend Class

A whole class can be declared as a friend, giving **all its methods** access to private/protected members.

#### 🧪 Example:

```cpp
class Engine;

class Car {
private:
    int fuel = 50;

    // Declare entire class as a friend
    friend class Engine;
};

class Engine {
public:
    void checkFuel(Car c) {
        cout << "Car fuel: " << c.fuel << endl;
    }
};
```

#### 📝 Key Points

* All members of `Engine` can access `Car`'s private/protected data.
* Friendship is one-way: `Car` doesn't get access to `Engine`.

---

### 🔹 Summary Table

| Type                   | Who gets access?                | One-way? | Inheritable? | Use case                            |
| ---------------------- | ------------------------------- | -------- | ------------ | ----------------------------------- |
| Friend Function        | Specific function               | ✅ Yes    | ❌ No         | Utility/helper functions            |
| Friend Member Function | One method from another class   | ✅ Yes    | ❌ No         | Selective access between classes    |
| Friend Class           | All methods in the friend class | ✅ Yes    | ❌ No         | Tight collaboration between classes |

---

### ⚠️ Best Practices

* Use `friend` **sparingly** — it breaks encapsulation.
* Prefer public APIs when possible.
* Use for tightly coupled classes (e.g., operator overloading, internal helper classes, performance-sensitive access).
