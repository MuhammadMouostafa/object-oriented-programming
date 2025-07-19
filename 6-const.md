# C++ `const` Keyword Lesson

The `const` keyword in C++ is essential for writing safer, more predictable, and maintainable code. It can be applied to functions, variables, objects, and pointers. This lesson covers all use cases with examples.

---

## 📌 Const Member Variable

A `const` member variable must be initialized either:

* At declaration
* In the constructor's initializer list

### ✅ Example:

```cpp
class Car {
private:
    const string type = "Nissan";
    const string engine_type;

public:
    Car(string engine_type) : engine_type(engine_type) {}
};
```

---

## 📌 Const Member Function

A member function that doesn't modify any data members should be declared as `const`.

### ✅ Syntax

```cpp
void functionName() const;
```

### ✅ Why use it?

* Prevents unintended changes to the object
* Allows calling the function on const objects

### ✅ Example:

```cpp
class Car {
public:
    string type, color;
    int current_speed;

    Car(string type, string color) : type(type), color(color), current_speed(0) {}

    void print() const {
        cout << type << ", " << color << " " << current_speed << endl;
        // current_speed++; ❌ Compilation error
    }

    void changeColor(string new_color) {
        color = new_color;
    }
};
```

---

## 📌 Const Object

A `const` object cannot modify any member variables or call non-const member functions.

### ✅ Example:

```cpp
int main() {
    const Car car1("Nissan", "Black");
    car1.print();
    // car1.changeColor("Red"); ❌
    // car1.current_speed++; ❌
}
```

---

## 📌 Pointer to Const Object

You can prevent modification of the object via pointer.

### ✅ Syntax

```cpp
const Type* ptr = &value;
```

### ✅ Example:

```cpp
const Car* car2 = new Car("Audi", "Black");
car2->print();
// car2->changeColor("Red"); ❌
// car2->current_speed++; ❌
```

---

## 📌 Const Pointer

A `const` pointer means the address stored in the pointer cannot be changed, but the data it points to can be modified.

### ✅ Syntax

```cpp
Type* const ptr = &value;
```

### ✅ Example:

```cpp
Car* const car2 = new Car("Audi", "Black");
car2->print();
car2->changeColor("Red");
car2->current_speed++;
// car2 = &car1; ❌ Cannot change pointer address
```

---

## 📌 Const Pointer to Const Object

Both the address and the object are immutable.

### ✅ Example:

```cpp
const Car* const car2 = new Car("Audi", "Black");
car2->print();
// car2->changeColor("Red"); ❌
// car2->current_speed++; ❌
// car2 = &car1; ❌
```

---

Using `const` effectively improves your code’s robustness, safety, and readability.
