# 🔐 Access Specifiers in C++

Access specifiers control the visibility and accessibility of class members. C++ provides three primary access levels:

* **`private`**: Accessible only within the same class
* **`public`**: Accessible from outside the class
* **`protected`**: Reserved for inheritance (covered in Lesson 5)

---

### 📦 Example: Basic Access Specifiers

```cpp
class MyClass {
private:
    int privateValue;
    void privateFunction() {
        // Accessible only within this class
    }

public:
    int publicValue;
    void publicFunction() {
        // Can access both private and public members
        privateValue = 5;
        privateFunction();
    }
};
```

---

## 🤔 Why Access Specifiers Matter

Good practice: **Hide everything that is not supposed to be used directly by the outside world.**

---

### 🚗 Example: Poor Encapsulation (Everything is Public)

```cpp
class Car {
public:
    string brand;
    int speed;

    void accelerate() {
        speed++;
    }
};

int main() {
    Car car;
    car.speed = -100;  // Invalid state!
    car.accelerate();
}
```

❌ Anyone can directly set invalid values like negative speed.

---

### ✅ Improved Encapsulation with Private Members

```cpp
class Car {
private:
    string brand;
    int speed;

public:
    Car(string b) : brand(b), speed(0) {}

    void accelerate() {
        speed++;
    }

    int getSpeed() {
        return speed;
    }
};

int main() {
    Car car("Nissan");
    car.accelerate();
    cout << car.getSpeed();  // OK
    // car.speed = -50;      // ❌ Compilation error
}
```

✅ Keeps the object in a valid state by hiding sensitive internals.

---

## 🛠️ Setters and Getters (When Needed)

Setters and getters allow controlled access to private data. But avoid using them if unnecessary.

```cpp
class Car {
private:
    string color;
    int speed;

public:
    Car(string c) : color(c), speed(0) {}

    string getColor() {
        return color;
    }

    void setColor(string newColor) {
        color = newColor;
    }
};
```

> ⚠️ **Note**: Only create setters/getters when there is a real need.

---

## 🔒 Summary

* Use **`private`** for internal data and helper methods.
* Use **`public`** only for operations intended for the outside world.
* We'll cover **`protected`** in the future lesson on inheritance.
