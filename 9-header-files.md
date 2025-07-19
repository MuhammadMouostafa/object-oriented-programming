# 📄 Header Files in C++

In C++, **header files** help organize code by separating *declarations* (what something is) from *definitions* (how it works). This makes your programs easier to read, maintain, and reuse.

---

## 📌 Why Use Header Files?

Header files are used to:

* Separate interface (what) from implementation (how)
* Avoid code duplication
* Improve readability and maintainability
* Enable modular programming

---

## 🔍 What vs How

| Concept                                                       | Placed In                        | Example          |
| ------------------------------------------------------------- | -------------------------------- | ---------------- |
| **What** – class declarations, function prototypes, constants | **Header File** (`.h` or `.hpp`) | `math_utils.h`   |
| **How** – function definitions, logic                         | **Source File** (`.cpp`)         | `math_utils.cpp` |

---

## 🧱 Example: Splitting a Class

### 1. `Person.h` – Declaration (What)

```cpp
#ifndef PERSON_H
#define PERSON_H

class Person {
private:
    int age;
    std::string name;

public:
    Person(std::string n, int a);
    void greet();
};

#endif
```

> This file declares the class and its methods, but doesn't define their logic.

---

### 2. `Person.cpp` – Definition (How)

```cpp
#include "Person.h"
#include <iostream>

Person::Person(std::string n, int a) : name(n), age(a) {}

void Person::greet() {
    std::cout << "Hello, I'm " << name << " and I'm " << age << " years old.\n";
}
```

---

### 3. `main.cpp` – Using the Class

```cpp
#include "Person.h"

int main() {
    Person p("Alice", 25);
    p.greet();
    return 0;
}
```

---

## 🛡️ Include Guards

To prevent a header file from being included multiple times (which causes errors), we use **include guards**.

### Traditional Way

```cpp
#ifndef PERSON_H
#define PERSON_H
// code here
#endif
```

### Modern Way – `#pragma once`

```cpp
#pragma once
// code here
```

> ✅ `#pragma once` is simpler and widely supported. It tells the compiler to include the file only once.

---

## ✅ Summary

* Use header files to declare *what* a class or function is.
* Use source files to define *how* they work.
* Use `#pragma once` to avoid multiple inclusion errors.
