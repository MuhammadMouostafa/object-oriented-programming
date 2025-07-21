# ⚙️ Operator Overloading in C++

Operator overloading in C++ allows you to redefine the way operators work for user-defined types (classes). It helps make class objects behave more like fundamental types, making code more intuitive and readable.

---

## ✅ Why Overload Operators?

* To allow intuitive syntax with user-defined types.
* To make classes behave more like built-in types.
* To increase code readability and maintainability.

---

## 🧩 Which Operators Can Be Overloaded?

Almost all operators can be overloaded, including:

* Arithmetic: `+`, `-`, `*`, `/`, `%`
* Comparison: `==`, `!=`, `<`, `>`, `<=`, `>=`
* Assignment: `=`, `+=`, `-=`, etc.
* Unary: `++`, `--`, `-`, `!`
* Stream: `<<`, `>>`

> ❌ Operators like `::`, `.`, `.*`, `sizeof`, and `?:` cannot be overloaded.

---

## 🛠️ Syntax for Operator Overloading

Operator overloading can be done:

* **As a member function**
* **As a non-member friend function**

### Member Function Syntax:

```cpp
class MyClass {
public:
    MyClass operator+(const MyClass& other) const {
        MyClass result;
        // ... logic
        return result;
    }
};
```

### Friend Function Syntax:

```cpp
class MyClass {
    int value;
public:
    MyClass(int val) : value(val) {}

    friend MyClass operator+(const MyClass& a, const MyClass& b);
};

MyClass operator+(const MyClass& a, const MyClass& b) {
    return MyClass(a.value + b.value);
}
```

---

## 🔄 Example: Complex Number Addition

```cpp
#include <iostream>
using namespace std;

class Complex {
private:
    float real, imag;

public:
    Complex(float r = 0, float i = 0) : real(r), imag(i) {}

    // Overload '+' using member function
    Complex operator+(const Complex& other) const {
        return Complex(real + other.real, imag + other.imag);
    }

    void display() const {
        cout << real << "+" << imag << "i" << endl;
    }
};

int main() {
    Complex c1(3, 2), c2(1, 7);
    Complex c3 = c1 + c2;  // Uses overloaded '+'
    c3.display();          // Output: 4+9i
    return 0;
}
```

---

## ⏫ Overloading Unary Operators

```cpp
class Counter {
    int value;
public:
    Counter(int v) : value(v) {}

    // Prefix increment
    Counter& operator++() {
        ++value;
        return *this;
    }

    // Postfix increment
    Counter operator++(int) {
        Counter temp = *this;
        ++value;
        return temp;
    }
};
```

---

## 🧪 Overloading Comparison Operators

```cpp
class Box {
    int length;
public:
    Box(int l) : length(l) {}

    bool operator==(const Box& b) const {
        return length == b.length;
    }
};
```

---

## 📥 Overloading Stream Operators

```cpp
#include <iostream>
using namespace std;

class Point {
    int x, y;
public:
    Point(int a = 0, int b = 0) : x(a), y(b) {}

    friend ostream& operator<<(ostream& os, const Point& p);
    friend istream& operator>>(istream& is, Point& p);
};

ostream& operator<<(ostream& os, const Point& p) {
    os << "(" << p.x << ", " << p.y << ")";
    return os;
}

istream& operator>>(istream& is, Point& p) {
    is >> p.x >> p.y;
    return is;
}
```

---

## 🧾 Summary Table

| Operator      | Overloadable | Member | Friend |
| ------------- | ------------ | ------ | ------ |
| +, -, \*, /   | ✅            | ✅      | ✅      |
| =             | ✅            | ✅      | ❌      |
| <<, >>        | ✅            | ❌      | ✅      |
| \[]           | ✅            | ✅      | ❌      |
| ()            | ✅            | ✅      | ❌      |
| ->            | ✅            | ✅      | ❌      |
| ::, ., sizeof | ❌            | ❌      | ❌      |

---

## ⚠️ Best Practices

* Overload only where it makes logical sense.
* Maintain intuitive behavior (e.g., don't make `+` do subtraction).
* Use `const` correctness.
* Use `friend` only when necessary (e.g., for symmetric binary operators).

---

## 📌 Conclusion

Operator overloading in C++ empowers classes to behave like built-in types, enhancing code readability and usability. With great power comes great responsibility—use it wisely!
