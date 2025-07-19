# Encapsulation

Encapsulation is a fundamental concept in object-oriented programming (OOP) that refers to the bundling of data and methods that operate on that data within a single unit, which is achieved by class or struct..
> NOTE: I will use class only in this tutorial

![](/assets/images/encapsulation.png)

# Class
A Class is a user-defined data type, which holds its own data members and member functions, which can be accessed and used by creating an instance of that class.

- ## Attributes (Data members or member variables)
    Data members are the data variables that define the properties of the objects of a Class.

- ## Methods (Member functions)
    Member functions are the functions manipulate these data members and define the properties of the objects of a Class.


# Object
An Object is an instance of a class created with specifically defined data.
> Objects can correspond to real-world objects.

![](/assets/images/class-vs-object.png)



# Lets take this example:

If you need to create a program that manage a list of cars,

where each car has three attributes
- type
- max_speed 
- current_speed

And two actions 

- accelerate to increase the current_speed by one 
- break to decrease the current_speed by one.

You can write this program and it will works fine


## Solution 1: without class

```
#include<iostream>
using namespace std;

int main() 
{
    string car1_type = "Nissan";
    int car1_max_speed = 280;
    int car1_current_speed = 0;

    // accelerate car1
    car1_current_speed++;
    car1_current_speed++;
    car1_current_speed++;

    cout << "Current speed of car1: " << car1_current_speed << std::endl;

    // slow down car1
    car1_current_speed--;

    cout << "Current speed of car1: " << car1_current_speed << std::endl;

    //   The remaining implementation to do for many cars

    return 0;
}

```

## Solution 2: with class has data member but hasn't member functions

```
#include <iostream>
#include <string>

using namespace std;

class Car                               // Class name
{                                       // Start of class body 
public:                                 // Access specifier
    std::string type;                   // Member variable
    int current_speed, max_speed;       // Member variables
};                                      // The end of the class body

int main() 
{
    Car car1; // Declare Object car1 from class Car
    car1.type = "Nissan";
    car1.max_speed = 280;
    car1.current_speed = 0;

    // Accelerate car1
    car1.current_speed++;
    car1.current_speed++;
    car1.current_speed++;

    cout << "Current speed of car1: " << car1.current_speed << std::endl;

    // Slow down car1
    car1.current_speed--;

    cout << "Current speed of car1: " << car1.current_speed << std::endl;

    // Remaining implementation for other cars...

    return 0;
}

```

### Example explained

- The class keyword is used to create a class called Car.
- The public keyword is an access specifier, which specifies that members (attributes and methods) of the class are accessible from outside the class. You will learn more about access specifiers later.
- Inside the class, there are three attributes:
    - Two integer variables: max_speed and current_speed.
    - One string variable: type.
- The class body is enclosed within curly braces {}.
- The class definition concludes with a semicolon ;.
- To define object write "class_name.object_name".
- To access data member write "object_name.data_member_name".
## Solution 3: with class has member vairables amd member functions

```
#include <iostream>
#include <string>

using namespace std;

class Car 
{                                       // Class name
public:                                 // Access specifier
    std::string type;                   // Member variable
    int current_speed, max_speed;       // Member variables

    void accelerate()                   // Member function
    {                 
        if(current_speed < max_speed)
        {
            current_speed ++;
        }
    }

    void slowDown()                     // Member function
    {                     
        if (current_speed > 0)
        {
            current_speed --;
        }
    }
    void stop()                         // Member function
    {                     
        current_speed = 0;
    }

    void print()
    {
        cout << "Current speed of car1: " << current_speed << std::endl;
    }
};


int main()
{
    Car car1; // Declare Object car1 from class Car
    car1.type = "Nissan";
    car1.max_speed = 280;
    car1.current_speed = 0;

    // accelerate car1
    car1.accelerate();

    car1.accelerate();
    car1.accelerate();

    car1.print();

    // slow down car1
    car1.slowDown();
    car1.print();

    // stop car1
    car1.stop();
    car1.print();

    //   The remaining implementation to do for many cars

    return 0;
}

```

### Example explained

- Inside the class, there are three methods:
    - accelerate() : that increment the current_speed by one but not exeeds the max_speed.
    - slowDown() : that decrease the current_speed by one but not less than zero.
    - stop() : that set the current_speed to zero.
- To access member function write "object_name.member_function_name".
> Note: When a class is defined, no memory is allocated but when it is instantiated (i.e. an object is created) memory is allocated.