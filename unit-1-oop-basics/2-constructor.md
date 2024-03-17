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



# Default Constructor
A default constructor is a constructor that doesn’t take any argument. It has no parameters. It is also called a zero-argument constructor.

```
#include <iostream>

using namespace std;

class Car
{
public:
    Car()   // Constructor
    {
      cout << "New Object of Car class created" << endl;
    }
};


int main()
{
    Car car1;// Declare Object car1 from class Car (this will call the constructor)
    return 0;
}


```
 > NOTE: If no constructor created, the compiler create default constructor that do nothing class_name(){}.

# Parameterized Constructors
Constructors can also take parameters (just like regular functions), which can be useful for setting initial values for attributes.

> All the rules of the global functions default arguments will be applied to these parameters.

```
#include <iostream>

using namespace std;

class Car
{
public:
    std::string type;
    int current_speed, max_speed;
    Car(string in_type, int in_max_speed)   // Constructor take two parameters
    {
      type = in_type;
      max_speed = in_max_speed;
      current_speed = 0;
    }
    void print()
    {
        cout<< type << ", " << max_speed << ", " << current_speed << endl;
    }
};

int main()
{
    Car car1("Nissan", 280);//Declare Object car1 from class Car(this will call the constructor)
    car1.print();
    return 0;
}
```

# Many constructors in one class
```
#include <iostream>

using namespace std;

class Car
{
public:
    std::string type;
    int current_speed, max_speed;
    Car(string in_type, int in_max_speed)   // Constructor take two parameters
    {
      type = in_type;
      max_speed = in_max_speed;
      current_speed = 0;
    }
    Car(string in_type)                    // Constructor take one parameter
    {
      type = in_type;
      max_speed = 280;
      current_speed = 0;
    }
    void print()
    {
        cout<< type << ", " << max_speed << ", " << current_speed << endl;
    }
};

int main()
{
    Car car1("Nissan", 250);  // Will call constructor Car(string in_type, int in_max_speed)
    Car car2("Audi");         // Will call constructor Car(string in_type)
    
    car1.print();
    car2.print();
    
    return 0;
}

```

# Default Arguments with Parameterized Constructor
Just like normal functions, we can also define default values for the arguments of parameterized constructors.'
> All the rules of the default arguments will be applied to these parameters.

```
#include <iostream>

using namespace std;

class Car
{
public:
    std::string type;
    int current_speed, max_speed;
    Car(string in_type, int in_max_speed = 280)   // Constructor take one/two parameters
    {
      type = in_type;
      max_speed = in_max_speed;
      current_speed = 0;
    }
    
    // Avoid to create another constructor take one paramiter, it will get compilation error
    /*
    Car(string in_type)                    
    {
      type = in_type;
      max_speed = 280;
      current_speed = 0;
    }
    */
    void print()
    {
        cout<< type << ", " << max_speed << ", " << current_speed << endl;
    }
};

int main()
{
    Car car1("Nissan", 250);  // Will call constructor Car(string in_type, int in_max_speed)
    Car car2("Audi");         // Will call constructor Car(string in_type)
    
    car1.print();
    car2.print();
    
    return 0;
}
```

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