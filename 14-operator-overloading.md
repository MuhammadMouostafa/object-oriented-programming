# 📘 Operator Overloading

In C++, **operator overloading** allows you to redefine the behavior of built-in operators when used with user-defined types like classes.

---

## 🔹 Why Use Operator Overloading?

* Makes user-defined types behave like built-in types
* Enhances code readability and usability
* Provides syntactic sugar to simplify object manipulations

---

## ⚙️ General Syntax of Operator Overloading
There are two main ways to overload the `+` operator:


### ✅ 1. Member Function (Inside the Class)

Used when the left-hand operand must be a class object. Has access to private members.


#### ✅ Example
```cpp
class Point {
private:
    int x, y;
public:
    Point(int x = 0, int y = 0) : x(x), y(y) {}

    Point operator+(const Point& other) const {
        return Point(x + other.x, y + other.y);
    }
};
```

### ✅ 2. Non-Member Function (Outside the Class)

- Useful when You want symmetry for operations like `a + b` where `a` might `not` be an object of the class (e.g. `int + Point`)
- Must be made `friend` if `private` members need to be accessed

#### ✅ Example with Public Members
```cpp
class Point {
public:
    int x, y;
    Point(int x = 0, int y = 0) : x(x), y(y) {}
};

Point operator+(const Point& a, const Point& b) {
    return Point(a.x + b.x, a.y + b.y);
}

```
#### ✅ Example with Private Members (Using Friend)
```cpp
class Point {
private:
    int x, y;
public:
    Point(int x = 0, int y = 0) : x(x), y(y) {}

    friend Point operator+(const Point& a, const Point& b);
};

Point operator+(const Point& a, const Point& b) {
    return Point(a.x + b.x, a.y + b.y);
}
```

### 👇 Usage and Behavior
```cpp
Point p1(1, 2), p2(3, 4);
Point p3 = p1 + p2; // p3: (4, 6)
```

---

# 📊 Categorization of Overloadable Operators

## A. Operators That Can Be Overloaded as Member or Non-member Functions
These operators support both forms because they work with two operands and don't depend on being called from the class instance.

### 🔹 Arithmetic Operators

`+`, `-`, `*`, `/`, `%`

#### ✅ Example
```cpp
Point operator+(const Point& other) const {
    return Point(x + other.x, y + other.y);
}
```

#### 👇 Usage and Behavior
```cpp
Point a(2, 3), b(4, 1);
Point c = a + b;  // (6, 4)
```

### 🔹 Bitwise Operators

`&`, `|`, `^`, `~`, `<<`, `>>`

#### ✅ Example
```cpp
Point operator&(const Point& other) const {
    return Point(x & other.x, y & other.y);
}
```

#### 👇 Usage and Behavior
```cpp
Point a(3, 6), b(1, 4);
Point c = a & b;  // (1, 4)
```

### 🔹 Relational Operators

`==`, `!=`, `<`, `>`, `<=`, `>=`

#### ✅ Example
```cpp
bool operator==(const Point& other) const {
    return x == other.x && y == other.y;
}
```

#### 👇 Usage and Behavior
```cpp
Point a(1, 2), b(1, 2);
std::cout << (a == b);  // 1 (true)
```

### 🔹 Logical Operators

`&&`, `||`, `!`

#### ✅ Example
```cpp
bool operator!() const {
    return x == 0 && y == 0;
}
```

#### 👇 Usage and Behavior
```cpp
Point p(0, 0);
if (!p) std::cout << "Point is at origin\n";
```

### 🔹 Increment / Decrement Operators

`++`, `--` (both prefix and postfix)

#### ✅ Example
```cpp
// Prefix ++p
Point& operator++() {
    ++x;
    ++y;
    return *this;
}

// Postfix p++
Point operator++(int) {
    Point temp = *this;
    ++x;
    ++y;
    return temp;
}
```

#### 👇 Usage and Behavior
```cpp
Point p1(1, 1);
Point p2(1, 1);

std::cout << ++p1 << std::endl;  // Outputs: (2, 2); p1 becomes (2, 2)
std::cout << p2++ << std::endl;  // Outputs: (1, 1); p2 becomes (2, 2)
```

### 🔹 Compound Assignment Operators

`+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `|=`, `^=`, `<<=`, `>>=`

#### ✅ Example
```cpp
Point& operator+=(const Point& other) {
    x += other.x;
    y += other.y;
    return *this;
}
```

#### 👇 Usage and Behavior
```cpp
Point p(1, 2), q(3, 4);
p += q;    // (4, 6)
p.print();
```

### 🔹 Subscript Operator `[]`

#### ✅ Example
```cpp
int& operator[](int index) {
    return (index == 0) ? x : y;
}
```

#### 👇 Usage and Behavior
```cpp
Point p(10, 20);
p[0] = 5;   // x becomes 5
p[1] = 7;   // y becomes 7
std::cout << p[0] << std::endl; // Outputs: 5
std::cout << p[1] << std::endl; // Outputs: 7
```

### 🔹 Function Call Operator `()`

#### ✅ Example
```cpp
void operator()() const {
    std::cout << "Point(" << x << ", " << y << ")\n";
}
```

#### 👇 Usage and Behavior
```cpp
Point p(1, 2);
p();  // Outputs: "Point: (1, 2)"
```

### 🔹 Comma Operator `,`

#### ✅ Example
```cpp
Point operator,(const Point& other) {
    return other;  // returns rightmost argument
}
```

#### 👇 Usage and Behavior
```cpp
Point a(1, 2), b(3, 4);
Point c = (a, b);  // returns b
```

## B. Operators That Must Be Overloaded as Member Functions
These operators require a class instance to be on the left-hand side, or they perform operations that directly affect the class's internal behavior.

### 🔹 Assignment Operator `=`

#### ✅ Example
```cpp
Point& operator=(const Point& other) {
    x = other.x;
    y = other.y;
    return *this;
}
```

#### 👇 Usage and Behavior
```cpp
Point p1(7, 8), p2;
p2 = p1; // p2 now is (7, 8)
```

### 🔹 Member Access Operator `->`

#### 📌 Purpose
The `->` operator is used to access members via a pointer.
It is often overloaded in smart pointer or proxy classes.

#### 🧠 Concept
- Overloading `->` allows your class to behave like a pointer.
- The returned object must itself support `.` or `->`.

### ✅ Overload Declaration
```cpp
ClassType* operator->();
```

#### ✅ Example
```cpp
#include <iostream>

class MyClass {
public:
    void greet() {
        std::cout << "Hello from MyClass!" << std::endl;
    }
};

class SmartPointer {
    MyClass* ptr;

public:
    SmartPointer(MyClass* p) : ptr(p) {}

    MyClass* operator->() {
        return ptr;
    }
};
```

#### 👇 Usage and Behavior
```cpp
int main() {
    MyClass real;
    SmartPointer sp(&real);
    sp->greet();  // Output: Hello from MyClass!
}
```

### 🔹 Memory Management Operators

**Operators:** `new`, `delete`, `new[]`, `delete[]`

These operators can be overloaded to customize how memory is allocated and deallocated for objects of a class.

* Must be overloaded as `static` member functions.
* Cannot be non-member functions.

---

#### ✅ Declaration & Implementation

```cpp
class MyClass {
public:
    int x;

    MyClass(int val) : x(val) {
        std::cout << "Constructor called\n";
    }

    ~MyClass() {
        std::cout << "Destructor called\n";
    }

    // Overload operator new
    static void* operator new(size_t size) {
        std::cout << "Custom new for size: " << size << std::endl;
        return ::operator new(size); // Use global new
    }

    // Overload operator delete
    static void operator delete(void* ptr) {
        std::cout << "Custom delete\n";
        ::operator delete(ptr); // Use global delete
    }

    // Overload operator new[]
    static void* operator new[](size_t size) {
        std::cout << "Custom new[] for size: " << size << std::endl;
        return ::operator new[](size);
    }

    // Overload operator delete[]
    static void operator delete[](void* ptr) {
        std::cout << "Custom delete[]\n";
        ::operator delete[](ptr);
    }
};
```

#### 👇 Usage and Behavior

```cpp
int main() {
    MyClass* obj = new MyClass(10);     // Triggers overloaded new
    delete obj;                          // Triggers overloaded delete

    MyClass* arr = new MyClass[2]{ {1}, {2} }; // Triggers overloaded new[]
    delete[] arr;                                 // Triggers overloaded delete[]
    return 0;
}
```


---

## C. Operators That Must Be Overloaded as Non-member Functions
These operators cannot be members because the left-hand operand is not an object of your class.

### 🔹 Stream Insertion / Extraction `<<`, `>>`

#### ✅ Example
```cpp
std::ostream& operator<<(std::ostream& os, const Point& pt) {
    os << "(" << pt.x << ", " << pt.y << ")";
    return os;
}

std::istream& operator>>(std::istream& is, Point& pt) {
    is >> pt.x >> pt.y;
    return is;
}
```

#### 👇 Usage and Behavior
```cpp
Point p;
std::cin >> p;
std::cout << p;
```

---

## ❌ D. Operators That Cannot Be Overloaded  
These operators are part of the core language syntax and behavior. C++ does not allow them to be overloaded.

- `::` (Scope resolution)  
- `.` (Member access)  
- `.*` (Pointer-to-member access)  
- `?:` (Ternary conditional)  
- `sizeof`  
- `typeid`  
- `alignof`  
- `noexcept`  
- `static_cast`  
- `dynamic_cast`  
- `reinterpret_cast`  
- `const_cast`  
- `co_await`


