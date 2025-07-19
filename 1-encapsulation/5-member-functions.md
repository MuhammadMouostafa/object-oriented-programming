# C++ Class Member Functions

## What Are Class Member Functions?

A **member function** is a function that is declared inside a class and operates on the data members of that class.

### Example:

```cpp
#include <iostream>
using namespace std;

class Car {
private:
    string type;
    int max_speed;
    int current_speed;

public:
    void setValues(string t, int max) {
        type = t;
        max_speed = max;
        current_speed = 0;
    }

    void accelerate(int amount) {
        current_speed += amount;
        if (current_speed > max_speed) current_speed = max_speed;
    }

    void printInfo() {
        cout << "Type: " << type << ", Max Speed: " << max_speed << ", Current Speed: " << current_speed << endl;
    }
};

int main() {
    Car car1;
    car1.setValues("Nissan", 280);
    car1.accelerate(50);
    car1.printInfo();
    return 0;
}
```

---

## Member Function Declaration and Definition

In C++, a **member function** is a function that is **declared inside a class**. Its **definition** (the actual implementation) can be provided either **inside** or **outside** the class body.

---

### ✅ Declaration Inside the Class

A member function must be **declared** within the class definition. This indicates that the function is associated with the class.

```cpp
class Car {
public:
    void print(); // Declaration of member function
};
```

---

### ✅ Definition Outside the Class

You can define the function outside the class using the **scope resolution operator `::`** to specify that the function belongs to the class.

```cpp
void Car::print() {
    std::cout << "This is a car.\n";
}
```

Here, `Car::print()` means that the function `print` is part of the `Car` class.

---

### ✅ Alternatively: Definition Inside the Class

If the function body is written inside the class, it's both declared and defined in one place. These functions are treated as `inline` by the compiler.

```cpp
class Car {
public:
    void print() {
        std::cout << "This is a car.\n";
    }
};
```

This approach is common for simple or one-line functions.

---

## Const Member Functions

These functions guarantee not to modify any member data.

```cpp
class Car {
private:
    int max_speed;
public:
    int getMaxSpeed() const {
      // max_speed++; // can't modify any data member inside it
        return max_speed;
    }
};
```

---

## Private vs Public Functions

* **Private** functions: only accessible within the class.
* **Public** functions: accessible from outside the class.

```cpp
class Car {
private:
    void internalFunction() {}

public:
    void publicFunction() {
        internalFunction(); // OK
    }
};

int main() {
    Car c;
    // c.internalFunction(); // ❌ Error
    c.publicFunction(); // ✅
}
```