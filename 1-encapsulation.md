# 📦 Encapsulation in C++

Encapsulation is a fundamental principle in object-oriented programming (OOP). It means bundling data (attributes) and the functions (methods) that operate on that data into a single unit — the class.

🔒 It helps protect the internal state of an object and exposes only what’s necessary to the outside world.

![](/assets/images/encapsulation.png)

---

🔧 **Class and Object Basics**

### 🔹 Class

A class is a blueprint for creating objects. It defines attributes (data members) and actions (member functions).

```cpp
class Car {
public:
    string type;
    int max_speed;
    int current_speed;

    void accelerate();  // member function declaration
};
```

### 🔹 Object

An object is an instance of a class.

![](/assets/images/class-vs-object.png)

```cpp
Car myCar;  // object of class Car
```

---

🧱 **Why Encapsulation?**

Suppose you're writing a program to manage cars. Each car has:

* **Attributes**: `type`, `max_speed`, `current_speed`
* **Behaviors**: `accelerate`, `brake`

You could do this without classes, but as your program grows, managing many cars becomes messy.

### ✅ Approach 1: Without Class

```cpp
#include <iostream>
using namespace std;

int main() {
    string type1 = "Nissan";
    int max_speed1 = 280;
    int current_speed1 = 0;

    current_speed1++;  // accelerate
    current_speed1++;

    cout << "Speed: " << current_speed1 << endl;

    current_speed1--;  // brake

    cout << "Speed: " << current_speed1 << endl;
}
```

❌ **Drawbacks:**

* Difficult to manage many cars
* No structure or code reuse

---

### ✅ Approach 2: Class With Data Members Only

```cpp
#include <iostream>
using namespace std;

class Car {
public:
    string type;
    int max_speed;
    int current_speed;
};

int main() {
    Car car1;
    car1.type = "Nissan";
    car1.max_speed = 280;
    car1.current_speed = 0;

    car1.current_speed++;  // accelerate

    cout << "Speed: " << car1.current_speed << endl;
}
```

🟡 **Better structure**, but we still manually modify `current_speed`.

---

### ✅ Approach 3: Class With Data and Member Functions

```cpp
#include <iostream>
using namespace std;

class Car {
public:
    string type;
    int max_speed;
    int current_speed;

    void accelerate() {
        if (current_speed < max_speed) {
            current_speed++;
        }
    }

    void brake() {
        if (current_speed > 0) {
            current_speed--;
        }
    }

    void stop() {
        current_speed = 0;
    }

    void print() {
        cout << "Speed: " << current_speed << endl;
    }
};

int main() {
    Car car1;
    car1.type = "Nissan";
    car1.max_speed = 280;
    car1.current_speed = 0;

    car1.accelerate();
    car1.accelerate();
    car1.print();       // Speed: 2

    car1.brake();
    car1.print();       // Speed: 1

    car1.stop();
    car1.print();       // Speed: 0
}
```

✅ **Clean and reusable**
✅ **Functions hide how speed is changed**
✅ **Easy to manage many cars**

---

📝 **Key Points**

* Use **classes** to group related data and behaviors.
* Use **member functions** to manipulate internal state safely.
* **Encapsulation** keeps your code **modular**, **maintainable**, and **secure**.
