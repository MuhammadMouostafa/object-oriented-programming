# ⚙️ Operator Overloading

In C++, operator overloading allows us to define custom behavior for operators when applied to user-defined types like classes or structs. This makes code involving custom types more intuitive and readable.

---

## 🤔 Why Use Operator Overloading?

* To make user-defined types behave like built-in types.
* Improve readability and usability of your classes.
* Allow intuitive syntax for complex operations (e.g., matrix addition, complex numbers).

---

## 🧩 Rules & Guidelines

* ✅ You can only overload **existing operators**.
* ❌ You **cannot create** new operators.
* ❌ Some operators **cannot** be overloaded: 
    - `sizeof`
    - `typeid`
    - Scope resolution `::`
    - Class member access operators (`.` and `*`)
    - Ternary or conditional operators (`?` and `:`)

---

## 🛠️ Syntax for Operator Overloading

Operator overloading can be done:

* **As a member function**
* **As a non-member friend function**

---

## 1. 🔢 Arithmetic Binary Operators

**Operators:** `+`, `-`, `*`, `/`, `%`
**Overloading:** Member or friend function

### Example: `+` Operator as Member Function

```cpp
class Point {
public:
    int x, y;
    Point(int x = 0, int y = 0) : x(x), y(y) {}

    Point operator+(const Point& other) const {
        return Point(x + other.x, y + other.y);
    }

    void display() const {
        std::cout << "(" << x << ", " << y << ")\n";
    }
};

int main() {
    Point p1(2, 3), p2(4, 5);
    Point result = p1 + p2;
    result.display(); // (6, 8)
}
```

---

## 2. ➕ Compound Assignment Operators

**Operators:** `+=`, `-=`, `*=`, `/=`, `%=`
**Overloading:** Must be a member function

### Example: `+=` Operator

```cpp
class Point {
public:
    int x, y;
    Point(int x = 0, int y = 0) : x(x), y(y) {}

    Point& operator+=(const Point& other) {
        x += other.x;
        y += other.y;
        return *this;
    }

    void display() const {
        std::cout << "(" << x << ", " << y << ")\n";
    }
};

int main() {
    Point p1(1, 2), p2(3, 4);
    p1 += p2;
    p1.display(); // (4, 6)
}
```

---

## 3. 🔢 Arithmetic Unary Operators

**Operators:** `++`, `--`, `-`, `!`
**Overloading:** Member function preferred

### Example: Prefix `++`

```cpp
class Point {
public:
    int x, y;
    Point(int x = 0, int y = 0) : x(x), y(y) {}

    Point& operator++() {
        ++x;
        ++y;
        return *this;
    }

    void display() const {
        std::cout << "(" << x << ", " << y << ")\n";
    }
};

int main() {
    Point p(5, 6);
    ++p;
    p.display(); // (6, 7)
}
```

---

## 4. 🧪 Comparison Operators

**Operators:** `==`, `!=`, `<`, `>`, `<=`, `>=`
**Overloading:** Member or friend function

### Example: `==` Operator as Friend Function

```cpp
class Point {
public:
    int x, y;
    Point(int x = 0, int y = 0) : x(x), y(y) {}

    friend bool operator==(const Point& a, const Point& b) {
        return a.x == b.x && a.y == b.y;
    }
};

int main() {
    Point p1(3, 4), p2(3, 4);
    std::cout << (p1 == p2) << "\n"; // 1 (true)
}
```

---

## 5. 📄 Stream Operators

**Operators:** `<<`, `>>`
**Overloading:** Friend function only

### Example: Output `<<` Operator

```cpp
#include <iostream>
class Point {
public:
    int x, y;
    Point(int x = 0, int y = 0) : x(x), y(y) {}

    friend std::ostream& operator<<(std::ostream& os, const Point& p) {
        os << "(" << p.x << ", " << p.y << ")";
        return os;
    }
};

int main() {
    Point p(2, 3);
    std::cout << p << "\n"; // (2, 3)
}
```

---

## 6. 🔢 Assignment Operator

**Operator:** `=`
**Overloading:** Member function only

### Example: Assignment Operator

```cpp
class Point {
public:
    int x, y;
    Point(int x = 0, int y = 0) : x(x), y(y) {}

    Point& operator=(const Point& other) {
        if (this != &other) {
            x = other.x;
            y = other.y;
        }
        return *this;
    }

    void display() const {
        std::cout << "(" << x << ", " << y << ")\n";
    }
};

int main() {
    Point p1(7, 8), p2;
    p2 = p1;
    p2.display(); // (7, 8)
}
```

---

## 7. 🔍 Subscript Operator

**Operator:** `[]`
**Overloading:** Member function only

### Example: `[]` to access x or y

```cpp
class Point {
    int coords[2];
public:
    Point(int x = 0, int y = 0) {
        coords[0] = x;
        coords[1] = y;
    }

    int& operator[](int index) {
        return coords[index];
    }

    void display() const {
        std::cout << "(" << coords[0] << ", " << coords[1] << ")\n";
    }
};

int main() {
    Point p(10, 20);
    p[0] = 5;
    p[1] = 15;
    p.display(); // (5, 15)
}
```

---

## 8. 📞 Function Call Operator

**Operator:** `()`
**Overloading:** Member function only

### Example: Call Point as function

```cpp
class Point {
public:
    int x, y;
    Point(int x = 0, int y = 0) : x(x), y(y) {}

    void operator()() const {
        std::cout << "Point: (" << x << ", " << y << ")\n";
    }
};

int main() {
    Point p(3, 4);
    p(); // Point: (3, 4)
}
```

---

## ✅ Summary Table

| Category      | Operators               | Overload Method  |
| ------------- | ----------------------- | ---------------- |
| Arithmetic    | `+`, `-`, `*`, `/`, `%` | Member / Friend  |
| Compound      | `+=`, `-=`, etc.        | Member only      |
| Unary         | `++`, `--`, `-`, `!`    | Member preferred |
| Comparison    | `==`, `!=`, `<`, etc.   | Member / Friend  |
| Assignment    | `=`                     | Member only      |
| Stream        | `<<`, `>>`              | Friend only      |
| Subscript     | `[]`                    | Member only      |
| Function call | `()`                    | Member only      |

