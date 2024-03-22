# Destructor
An Destructor in C++ is a special method (has no return type) that is automatically called by the compiler when the scope of an object of the class ends. 

> To create a destructor, use ~ followed by the same name as the class, followed by parentheses ():

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
    ~Car()   // Destructor
    {
      cout << "The scope of Object of Car class ended" << endl;
    }
};

int main() {
    Car car1; // Declare Object car1 from class Car (this will call the constructor)
    return 0;
} // The scode of Object car1 of class Car ended(this will call the destructor)

```

# Destructor calling ordere
Destructor is called in the reverse order of its constructor invocation.
```
#include <iostream>

using namespace std;

class Car
{
    string type;
public:
    Car(string in_type)
    {
        type = in_type;
        cout << "Car created of type "<< type << endl;
    }
    ~Car()
    {
        cout << "Car destroyed of type " << type << endl;
    }
};

int main() {
    Car car1("Nissan"); 
    Car car2("Audi"); 
    Car car3("Volvo"); 
    
    return 0;
}
```

# Dealocate the memory
T avoid memory leak, delete all pointers in the class.

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
    ~Car()   // Destructor
    {
      cout << "The scope of Object of Car class ended" << endl;
    }
};

class Game
{
public:
  Car *arr;
  public:
    Game()   // Constructor
    {
      arr = new Car[4];
    }
    ~Game()   // Destructor
    {
      delete[] arr;
      arr = nullptr;
    }
};

int main() {
    Game game;
}
```

# Characteristics of Destructors in C++

- Destructor is invoked automatically by the compiler when its corresponding constructor goes out of scope and releases the memory space that is no longer required by the program.
- Destructor neither requires any argument nor returns any value therefore it cannot be overloaded.
- Destructor  cannot be declared as static and const;
- Destructor should be declared in the public section of the program.
- Destructor is called in the reverse order of its constructor invocation.