# Static Members in C++

## Static Member Variable

A static member variable is shared across all objects of the class. It belongs to the class, not to any specific object.

### Key Points

* Only one copy exists, shared by all objects.
* Initialized before any object is created.
* Lifetime is the entire duration of the program.
* Accessible using member functions or the class name via the scope resolution operator.

### Syntax

```cpp
class ClassName {
    static data_type variable_name; // Declaration inside class
};

// Definition outside class

data_type ClassName::variable_name = initial_value;

// Access
ClassName::variable_name;
```

### Example

```cpp
#include <iostream>
using namespace std;

class Car {
public:
    static int number_of_cars;

    Car() {
        number_of_cars++;
        cout << "Number of cars: " << number_of_cars << endl;
    }
};

int Car::number_of_cars = 0;

int main() {
    Car car1;
    Car car2;
    Car::number_of_cars++; // direct access via class name
    Car car3;
}
```

## Static Member Function

A static member function is not tied to any object and can be called without an instance of the class.

### Key Points

* Can be called without creating an object.
* Only accesses static data members or other static functions.
* Cannot access `this` pointer or non-static members.
* Called using class name or other static functions.

### When to Use

* To access or modify shared static data.
* Useful for utility functions or global behavior tied to a class.

### Example

```cpp
#include <iostream>
using namespace std;

class Car {
public:
    string type, color;
    static int instructions_print_count;

    Car(string type, string color) : type(type), color(color) {}

    static void printDrivingInstructions() {
        instructions_print_count++;
        cout << "Instructions printed: " << instructions_print_count << " times." << endl;
        cout << "1. Fasten seat belt.\n2. Check mirrors.\n3. Drive safely.\n" << endl;

        // Cannot access non-static members like 'color' here.
    }
};

int Car::instructions_print_count = 0;

int main() {
    Car::printDrivingInstructions();
    Car::printDrivingInstructions();
    Car::printDrivingInstructions();
}
```
