# Static Variable
Static variables have the property of preserving their value even after they are out of their scope! Hence, a static variable preserves its previous value in its previous scope and is not initialized again in the new scope. 

### Syntax:
```
static data_type var_name = var_value;
```

```
#include <iostream>

using namespace std;

void fun1()
{
    static int x=0;
    x++;
    cout<<x<<endl;
}


int main()
{
    fun1();
    fun1();
    fun1();
}
```


# Static Data Member
Static data members are class members that are declared using static keywords. A static member has certain special characteristics which are as follows:

- Only one copy of that member is created for the entire class and is shared by all the objects of that class, no matter how many objects are created.
- It is initialized before any object of this class is created, even before the main starts.
- It is visible only within the class, but its lifetime is the entire program.

### Syntax:
```
static data_type data_member_name;
```

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

# Static member Function
static member function has two diffrents
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