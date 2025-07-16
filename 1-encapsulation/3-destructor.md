# 💣 Destructor in C++

A **destructor** is a special member function in C++ that is **automatically called** when an object:

- Goes **out of scope**
- Is **explicitly deleted**
- Or is a **global/static object** and the program ends

It is used to **release resources** such as dynamically allocated memory, file handles, or network connections.

---

## 🔧 Syntax

```cpp
class MyClass {
public:
    ~MyClass();  // Destructor declaration
};
```

- Begins with a **tilde (~)** followed by the class name
- Has **no return type**
- Takes **no parameters**
- Cannot be overloaded

---

## 🧠 Why Use a Destructor?

To **clean up resources** that your object acquired during its lifetime.

### Example:

```cpp
class Car {
public:
    string* type;

    Car(const string& in_type) {
        type = new string(in_type);
    }

    ~Car() {
        delete type;
        cout << "Destructor called\n";
    }
};
```

If you don’t release memory manually (`delete`), your program will **leak memory**.

---

## ⏱ When Is the Destructor Called?

| Situation                        | Destructor Called |
|----------------------------------|-------------------|
| Object goes out of scope         | ✅ Yes            |
| Object is deleted via `delete`   | ✅ Yes            |
| Program exits (global/static)    | ✅ Yes            |
| Object is moved                  | ✅ If not null    |

---

## 🔄 Destruction Order

Destructors are called in **reverse order of construction** when leaving scope.

### ✅ Example: Scope and Order

```cpp
#include <iostream>
using namespace std;

class Car {
public:
    string name;

    Car(string name) : name(name) {
        cout << "Constructed: " << name << endl;
    }

    ~Car() {
        cout << "Destroyed: " << name << endl;
    }
};

int main() {
    cout << "Entering scope\n";

    Car car1("BMW");
    {
        Car car2("Audi");
        Car car3("Tesla");
    } // car3 and car2 go out of scope here

    cout << "Leaving scope\n";
}
```

### 🧾 Output:
```
Entering scope
Constructed: BMW
Constructed: Audi
Constructed: Tesla
Destroyed: Tesla
Destroyed: Audi
Leaving scope
Destroyed: BMW
```

---

## 🚧 Destructor in Pointer-Holding Classes

### ❌ Unsafe (shallow copy):

```cpp
~Car() {
    delete type; // May cause double delete
}
```

### ✅ Safe:

```cpp
~Car() {
    if (type) {
        delete type;
    }
}
```

---

## 📏 Rule of Three / Five

If your class defines a **destructor**, you should also define:

- Copy constructor
- Copy assignment operator

And in C++11+:

- Move constructor
- Move assignment operator

This is the **Rule of Five**.

---

## ✅ Summary

| Feature               | Destructor                 |
|-----------------------|----------------------------|
| Syntax                | `~ClassName()`             |
| Parameters            | None                       |
| Return type           | None                       |
| Overload allowed?     | ❌ No                      |
| Automatic call?       | ✅ Yes                     |
| Purpose               | Cleanup resources          |
| Called when           | Object leaves scope or is deleted |

---
