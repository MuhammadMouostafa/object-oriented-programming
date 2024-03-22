# Static Member Variable
Static data members are class members that are declared using static keywords. A static member has certain special characteristics which are as follows:

- Only one copy of that member is created for the entire class and is shared by all the objects of that class, no matter how many objects are created.
- It is initialized before any object of this class is created, even before the main starts.
- It is visible only within the class, but its lifetime is the entire program.

### Syntax:
```
// declaration inside the class
static data_type data_member_name;
// definitoin outside the class
data_type class_name::data_member_name = initial_value;
```

### See the following example:

```
#include <iostream>

using namespace std;

class Car
{
public:
    static int number_of_cars;
    Car()
    {
        number_of_cars++;
        cout << number_of_cars << endl;
    }
};
int Car::number_of_cars = 0;

int main()
{
    Car car1;
    Car car2;
    Car car3;
}
```

# Static Member Function
Static Member Function in a class is the function that is declared as static because of which function attains certain properties as defined below:

- A static member function is independent of any object of the class. 
- A static member function can be called even if no objects of the class exist.
- A static member function can also be accessed using the class name through the scope resolution operator.
- A static member function can access static data members and static member functions inside or outside of the class.
- Static member functions have a scope inside the class and cannot access the current object pointer.
- You can also use a static member function to determine how many objects of the class have been created.

### The reason we need Static member function:
- Static members are frequently used to store information that is shared by all objects in a class. 
- For instance, you may keep track of the quantity of newly generated objects of a specific class type using a static data member as a counter. This static data member can be increased each time an object is generated to keep track of the overall number of objects.

### See the following example:

```
#include <iostream>

using namespace std;

class Car
{
public:
    std::string type, color;
    static int instructions_print_count;
    Car(string type, string color):
    type(type),
    color(color)
    {}

    static void printDrivingInstructions()
    {
        // can change static member variables
        instructions_print_count ++;
        cout<<"This is the " << instructions_print_count << " time to print the instructions" <<endl;
        
        // can't change non static member variables
        // color++;

        cout<< "Instruction 1" << endl;
        cout<< "Instruction 2" << endl;
        cout<< "Instruction 3" << endl;

    }
};

int Car::instructions_print_count = 0;


int main()
{
    Car::printDrivingInstructions();
    Car::printDrivingInstructions();
    Car::printDrivingInstructions();
}

```

> Note: Access member varaibles/function is another usage of scop resolution operator.