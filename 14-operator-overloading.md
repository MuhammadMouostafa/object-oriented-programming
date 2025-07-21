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
* ❌ Some operators **cannot** be overloaded.

## 📑 Categories of Overloadable Operators

## 1. 🧮 Arithmetic Operators

**Operators:** `+`, `-`, `*`, `/`, `%`
**Overloading:** Member or friend function
* Must return a new object for immutable behavior.

### ✅ Example: `+` operator as a **member function**

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

**Operators:** `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `|=`, `^=`, `<<=`, `>>=`
**Overloading:** Must be a member function

### ✅ Example: `+=` as a **member function**

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
## 4. ⚖️ Comparison Operators

**Operators:** `==`, `!=`, `<`, `>`, `<=`, `>=`
**Overloading:** Member or friend function

* Typically return `bool`
* Often implemented as **friend**.

### ✅ Example: `==` as a **friend function**

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

### 6. 🔁 Assignment Operator

**Operator:** `=`
**Overloading:** Member function only

### ✅ Example: `=` as a **member function**

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

## 9. 👈 Pointer-like Operator

**Operator:** `->`
**Overloading:** Member function only
* Used in smart pointers

#### ✅ Example: `->` as a **member function**

```cpp
class Wrapper {
    Point* ptr;
public:
    Wrapper(Point* p) : ptr(p) {}
    Point* operator->() { return ptr; }
};

int main() {
    Point* raw = new Point(5, 6);
    Wrapper w(raw);
    int x = w->x; // Access Point's x
}
```

---
## 10. 🧹 Memory Management Operators

**Operators:** `new`, `delete`, `new[]`, `delete[]`

* Overload as **static** or **global** functions

#### ✅ Example: Overloading `new` and `delete`

```cpp
class Point {
public:
    int x, y;
    void* operator new(size_t size) {
        std::cout << "Custom new\n";
        return ::operator new(size);
    }

    void operator delete(void* ptr) {
        std::cout << "Custom delete\n";
        ::operator delete(ptr);
    }
};

int main() {
    Point* p = new Point();
    delete p;
}
```

---


### 11. 🎯 Logical Operators

**Operators:** `&&`, `||`, `!`

* Usually return `bool`

#### ✅ Example: `!` as a **member function**

```cpp
class Point {
public:
    int x, y;
    bool operator!() const {
        return x == 0 && y == 0;
    }
};

int main() {
    Point p;
    if (!p) {
        // point is origin
    }
}
```

### 6. 🧠 Bitwise Operators

**Operators:** `&`, `|`, `^`, `~`, `<<`, `>>`

#### ✅ Example: `&` as a **friend function**

```cpp
class Point {
public:
    int x, y;
    Point(int x = 0, int y = 0) : x(x), y(y) {}
    friend Point operator&(const Point& a, const Point& b) {
        return Point(a.x & b.x, a.y & b.y);
    }
};

int main() {
    Point p1(6, 3), p2(4, 1);
    Point result = p1 & p2;
}
```

---

## ❌ Non-Overloadable Operators

| Operator                    | Reason                           |
| --------------------------- | -------------------------------- |
| `.`                         | Member access (fixed behavior)   |
| `.*`                        | Member pointer access            |
| `::`                        | Scope resolution                 |
| `?:`                        | Ternary operator                 |
| `sizeof`                    | Compile-time operator            |
| Casts (`static_cast`, etc.) | Use conversion functions instead |

---

## ✅ Summary Table

| Category            | Operators                  | Overloadable As           |                 |             |
| ------------------- | -------------------------- | ------------------------- | --------------- | ----------- |
| Arithmetic          | `+`, `-`, `*`, `/`, `%`    | Member / Friend           |                 |             |
| Compound Assignment | `+=`, `-=`, etc.           | Member only               |                 |             |
| Unary               | `++`, `--`, `-`, `!`       | Member preferred          |                 |             |
| Comparison          | `==`, `!=`, `<`, `>`, etc. | Member / Friend           |                 |             |
| Logical             | `&&`, \`                   |                           | `, `!\`         | Member only |
| Bitwise             | `&`, \`                    | `, `^`, `\~`, `<<`, `>>\` | Member / Friend |             |
| Assignment          | `=`                        | Member only               |                 |             |
| Stream I/O          | `<<`, `>>`                 | Friend only               |                 |             |
| Subscript           | `[]`                       | Member only               |                 |             |
| Function Call       | `()`                       | Member only               |                 |             |
| Pointer-like        | `->`                       | Member only               |                 |             |
| Memory Management   | `new`, `delete`, etc.      | Static / Global           |                 |             |
