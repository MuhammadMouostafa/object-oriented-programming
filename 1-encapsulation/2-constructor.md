# Constructor
A constructor in C++ is a special method (has no return type) that is automatically called by the compiler when an object of a class is instantiated.

> To create a constructor, use the same name as the class, followed by parentheses ():

# Types of Constructor in C++
Constructors can be classified based on in which situations they are being used.
### There are 4 types of constructors in C++:

- Default Constructor
- Parameterized Constructor
- Copy Constructor
- Move Constructor
- Delegating Constructor
- Explicit Constructor



# 1. Default Constructor
A constructor that takes no parameters (or all parameters have default values). It is automatically called when you create an object without arguments

```
#include <iostream>

using namespace std;

class Car
{
public:
    Car()   // Default Constructor
    {
      cout << "Default constructor called!" << endl;
    }
};

int main()
{
    Car car1;  // Calls default constructor
}


```
 > NOTE: If no constructor created, the compiler create default constructor that do nothing class_name(){}.

# 2. Parameterized Constructors
A constructor that takes one or more parameters to initialize the object with specific values.

> All the rules of the global functions default arguments will be applied to these parameters.

```
#include <iostream>

using namespace std;

class Car
{
public:
    string type;
    Car(string in_type)   // Parameterized constructor
    {
      type = in_type;
      cout<< "Parameterized constructor called with car type " << type << endl;
    }
};

int main()
{
    Car car1("Nissan"); // Calls parameterized constructor
}
```

### Many constructors in one class
```
#include <iostream>

using namespace std;

class Car
{
public:
    std::string type;
    Car()   // Default Constructor
    {
      cout << "Default constructor called!" << endl;
    }
    Car(string in_type)   // Parameterized constructor
    {
      type = in_type;
      cout<< "Parameterized constructor called with car type " << type << endl;
    }
};

int main()
{
  Car car1;  // Calls default constructor
  Car car2("Nissan"); // Calls parameterized constructor
}

```

# 3. Copy Constructors
Creates a new object by copying an existing object. Used when passing objects by value or explicitly copying.

```
#include <iostream>
using namespace std;

class Car {
public:
    string type;
    
    Car(string in_type)   // Parameterized constructor
    {
      type = in_type;
      cout<< "Parameterized constructor called with car type " << type << endl;
    }

    Car(const Car& other)
    {
      type = other.type;
      cout<< "Copy constructor called" << endl;
    }
};
```

### When the copy Constructor will be called?
- When an object is constructed based on another object of the same class. 
    > Car car1("Nissan");
    >
    > Car car2 = car1;  // Copy constructor called here
    >
    > Car car3(car1);   // Copy constructor called here

- When an object of the class is passed (to a function) by value as an argument. 
    > fun(Car car1)   // Copy constructor called when this function called
    >
    > {
    > 
    > }
- When an object of the class is returned by value. 
    > Car creatCar()
    >
    > {
    >
    > Car car1("Nissan");
    >
    > return car1;   // Copy constructor called here
    >
    > }
- When the compiler generates a temporary object.
    > Car car1 = car1("Nissan");   // Copy constructor called here


> The compiler provides a default Copy Constructor to all the classes if not created.


> Note: In all the previous examples the compiler will creates the default copy constructor to perform shallow copy at compile time and to make deep copy we need to override the copy constructor.

> Avoid the last two cases because of Return Value Optimization (RVO), which is a part of the C++ standard. The compiler is allowed to avoid the copy (or move) operation in certain scenarios for performance reasons, even if this would mean that the copy (or move) constructor and the destructor won’t get called and in our case shllow copy will happen even if we create copy constructor, so its based on the compiler.

## Shallow copy vs Deep copy
The default copy constructor created by the compiler to make shallow copy.

todoooooo


# 4. Move Constructor (C++11+)
Transfers ownership of resources from a temporary (rvalue) object, improving performance.

todooo

# 5. Delegating Constructor (C++11+)
A constructor that calls another constructor in the same class to avoid repeating code.
```
#include <iostream>

using namespace std;

class Car
{
public:
    std::string type;
    Car()   // Delegating Constructor
    {
      cout << "Default constructor called!" << endl;
      Car("Toyota");
    }
    Car(string in_type)   // Parameterized constructor
    {
      type = in_type;
      cout<< "Parameterized constructor called with car type " << type << endl;
    }
};

int main()
{
  Car car1;  // Calls default constructor
  Car car2("Nissan"); // Calls parameterized constructor
}
```

# 6. Explicit Constructor

todooo
expalin emplicit and explicit


# Characteristics of Constructors

- The name of the constructor is the same as its class name.
- Constructors are mostly declared in the public section of the class though they can be declared in the private section of the class.
- Constructors do not return values; hence they do not have a return type.
- A constructor gets called automatically when we create the object of the class.
- Constructors can be overloaded.
- A constructor can not be declared virtual.
- A constructor cannot be inherited.
- The addresses of the Constructor cannot be referred to.
- The constructor makes implicit calls to new and delete operators during memory allocation.