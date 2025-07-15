# C++ Constructors

A **constructor** in C++ is a special method that is **automatically called** when an object of a class is created. It is used to initialize the object.

> 🔸 A constructor has the **same name as the class** and **no return type**, not even `void`.

---

# ✅ Types of Constructors in C++

C++ supports multiple constructor types:

- **Default Constructor**
- **Parameterized Constructor**
- **Copy Constructor**
- **Move Constructor**
- **Delegating Constructor**
- **Explicit Constructor**

---

# 🟩 1. Default Constructor

A constructor that takes **no arguments**. Automatically called when you create an object **without any parameters**.

```cpp
#include <iostream>
using namespace std;

class Car {
public:
    Car() {  // Default Constructor
      cout << "Default constructor called!" << endl;
    }
};

int main() {
    Car car1;  // Calls default constructor
}
```

> 📌 **Note**: If no constructor is defined, the compiler generates a default constructor like this: `Car() {}`.

---

# 🟨 2. Parameterized Constructor

Used to initialize an object with specific values by **passing arguments** to the constructor.

```cpp
#include <iostream>
using namespace std;

class Car {
public:
    string type;

    Car(string in_type) {  // Parameterized Constructor
      type = in_type;
      cout << "Car type: " << type << endl;
    }
};

int main() {
    Car car1("Nissan"); // Calls parameterized constructor
    Car car2 = "Volvo"; // ⚠️ Implicit conversion from string to Car, Compiler calls Car(string) constructor automatically
}
```

## 🌀 Constructor Overloading

You can define multiple constructors in one class:

```cpp
#include <iostream>
using namespace std;
class Car {
public:
    string type;

    Car() {
      cout << "Default constructor\n";
    }

    Car(string in_type) {
      type = in_type;
      cout << "Car type: " << type << endl;
    }
};

int main() {
  Car c1;               // Default
  Car c2("Toyota");     // Parameterized
}
```

---

# 🟦 3. Copy Constructor

Creates a **new object by copying** an existing one.

```cpp
#include <iostream>
using namespace std;
class Car {
public:
    string type;

    Car(string in_type) {
      type = in_type;
    }

    Car(const Car& other) { // Copy Constructor
      type = other.type;
      cout << "Copy constructor called\n";
    }
};

int main() {
    Car car1("Ford");
    Car car2 = car1;    // Copy constructor called
}
```

## 🔍 Copy Constructor Triggers:
- Copy initialization (`Car car2 = car1;`)
- Passing by value (`void print(Car car1)`)
- Returning by value (`return car1;`)  ⛔(RVO may skip)
- Explicit Copy Construction(`Car car2(car1);`)

> ⚠️ Default copy constructors do **shallow copies**. To prevent issues with dynamic memory, define your **own copy constructor** for **deep copies**.

---

## 🔍 Shallow vs Deep Copy

| Shallow Copy                          | Deep Copy                               |
|--------------------------------------|------------------------------------------|
| Copies only pointers                 | Allocates new memory and copies values   |
| Multiple objects share the same data | Each object has its own copy             |
| May cause bugs on delete             | Safe and independent                     |

---

# 🟫 4. Move Constructor (C++11+)

A move constructor creates a new object by taking resources from a temporary object instead of copying them.



```cpp
class Car {
public:
    string* type;

    Car(string val) {
        type = new string(val);
        cout << "Constructor: " << *type << endl;
    }

    // Move Constructor
    Car(Car&& other) {
        type = other.type;
        other.type = nullptr;
        cout << "Move constructor called\n";
    }

    ~Car() {
        delete type;
    }
};

int main() {
    Car car1("BMW");
    Car car2 = std::move(car1); // Move constructor called
}
```

# ⚙️ Copy and Move Constructors: Default vs Custom (Pointer Example)

When a class contains **pointers**, using the compiler's **default copy and move constructors** can cause **serious problems** such as **double deletes** and **shallow copies**.

---

## 🚗 Case 1: Default Copy and Move Constructors (Shallow Copy – Problematic)

```cpp
#include <iostream>
using namespace std;

class Car {
public:
    string* type;

    Car(const string& in_type) {
        type = new string(in_type);
        cout << "Constructor: " << *type << endl;
    }

    ~Car() {
        delete type;
        cout << "Destructor called\n";
    }
};

int main() {
    Car car1("Nissan");

    Car car2 = car1;             // ❗ Uses auto-generated copy constructor → shallow copy
    Car car3 = std::move(car1);  // ❗ Uses auto-generated move constructor → shallow move
}
```

### ❌ What’s the Problem?

- The default copy/move constructors **just copy the pointer** value.
- `car1`, `car2`, and `car3` all end up **sharing the same pointer**.
- When destructors run, they each call `delete` on the same memory → **undefined behavior** (crash or memory corruption).

---

## ✅ Case 2: Manually Defined Copy and Move Constructors (Safe – Deep Copy and Move)

```cpp
#include <iostream>
using namespace std;

class SafeCar {
public:
    string* type;

    SafeCar(const string& in_type) {
        type = new string(in_type);
        cout << "Constructor: " << *type << endl;
    }

    // ✅ Deep Copy Constructor
    SafeCar(const SafeCar& other) {
        type = new string(*other.type);
        cout << "Copy constructor called\n";
    }

    // ✅ Move Constructor
    SafeCar(SafeCar&& other) noexcept {
        type = other.type;
        other.type = nullptr;
        cout << "Move constructor called\n";
    }

    ~SafeCar() {
        delete type;
        cout << "Destructor called\n";
    }
};

int main() {
    SafeCar car1("Toyota");

    SafeCar car2 = car1;             // ✅ Deep copy: allocates new memory
    SafeCar car3 = std::move(car1);  // ✅ Ownership transferred
}
```

---

## 🧠 Explanation

| Feature               | `Car` (Default)        | `SafeCar` (Defined)        |
|----------------------|------------------------|----------------------------|
| Copy constructor      | Shallow (pointer copy) | Deep (new memory)          |
| Move constructor      | Shallow (pointer copy) | Transfers ownership safely |
| Destructor behavior   | ❌ Crashes              | ✅ Safe                     |
| Use with STL/objects  | ❌ Unsafe               | ✅ Safe                     |

---

## 📝 Conclusion

If your class manages **dynamic memory** or **resources**:

- ❗ **Never rely** on the default copy/move constructors.
- ✅ Always define:
  - A custom **copy constructor** (for deep copies)
  - A custom **move constructor** (for ownership transfer)
  - A proper **destructor** (to avoid leaks)

This ensures your class behaves safely and predictably when copied or moved.

---

## 🟪 5. Delegating Constructor (C++11+)

Allows one constructor to **call another** constructor in the **same class** to reuse initialization logic.

```cpp
class Car {
public:
    string type;

    Car() : Car("Default") {
        cout << "Delegating constructor called\n";
    }

    Car(string in_type) {
        type = in_type;
        cout << "Parameterized constructor: " << type << endl;
    }
};

int main() {
    Car car1;              // Calls default → delegated to parameterized
    Car car2("Mazda");     // Calls parameterized
}
```

---

## 🟥 6. Explicit Constructor

Prevents the compiler from performing **implicit conversions** or copy-initializations.

```cpp
class Car {
public:
    string type;

    explicit Car(string in_type) {
        type = in_type;
        cout << "Explicit constructor called: " << type << endl;
    }
};

void display(Car c) {}

int main() {
    Car car1("Volvo");    // OK: direct initialization
    // Car car2 = "Volvo";  // ❌ Error: implicit conversion not allowed
    display(Car("Honda")); // OK
}
```

> ✅ Use `explicit` when you want to prevent unintended type conversions.

---

## 🧠 Characteristics of Constructors

- The name of the constructor is the same as its class name.
- Constructors are mostly declared in the public section.
- They do not have a return type.
- Called automatically when an object is created.
- Constructors can be **overloaded**.
- A constructor **cannot be virtual**.
- A constructor **cannot be inherited**.
- Constructor addresses **cannot be taken**.
- Constructors can call other constructors (**delegation**).
- The compiler provides default copy/move constructors unless you override them.