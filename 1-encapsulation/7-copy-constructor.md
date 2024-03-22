# Copy Constructors
A Copy Constructor creates a new object, which is an exact copy of the existing object. The compiler provides a default Copy Constructor to all the classes.


# When is the copy constructor called?
- When an object is constructed based on another object of the same class. 
    > Car car1;
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
    > Car car("Nissan",280);
    >
    > return car1;   // Copy constructor called here
    >
    > }
- When the compiler generates a temporary object.
    > Car car1 = Car("Nissan",280);   // Copy constructor called here


> Note: In all the previous examples the compiler will creates the default copy constructor to perform shallow copy at compile time and to make deep copy we need to override the copy constructor.


# Default Copy Contructor
The default copy constructor created by the compiler to make shallow copy.

```
#include <iostream>
using namespace std;


class Car {
private:

    std::string type, color;
    int *current_speed;
    int max_speed;

public:

    Car(string type, string color, int max_speed):
    type(type),
    color(color),
    max_speed(max_speed),
    current_speed(new int(0))
    {}

    void accelerate()
    {                 
        if(*current_speed < max_speed)
        {
            (*current_speed) ++;
        }
    }

    void changeColor(string new_color)
    {
        color = new_color;
    }
    void print()
    {
        cout<< type << ", " << color << ", " << max_speed << ", "
        << *current_speed << ", " << current_speed << endl;
    }

    ~Car()
    {
        // If you try to delete the pointer an exception will happen
        // delete current_speed;

        // now we have memory leak
    }

};

int main()
{
    Car car1("Nissan", "white", 280);
    Car car2(car1);

    car1.print();
    car2.print();
    cout<<endl;

    car1.changeColor("Black");
    // this will change current_speed in both cars
    car1.accelerate();


    car1.print();
    car2.print();
    cout<<endl;

    return 0;
}
```

# User Defined Copy Constructor
Overload the copy constructor to make deep copy.

```
#include <iostream>
using namespace std;


class Car {
private:

    std::string type, color;
    int *current_speed;
    int max_speed;

public:

    Car(string type, string color, int max_speed):
    type(type),
    color(color),
    max_speed(max_speed),
    current_speed(new int(0))
    {}

    Car(const Car& other)
    {
        type = other.type;
        color = other.color;
        max_speed = other.max_speed;
        current_speed = new int;
        *current_speed = *other.current_speed;
    }

    void accelerate()
    {                 
        if(*current_speed < max_speed)
        {
            (*current_speed) ++;
        }
    }

    void changeColor(string new_color)
    {
        color = new_color;
    }
    void print()
    {
        cout<< type << ", " << color << ", " << max_speed << ", "
        << *current_speed << ", " << current_speed << endl;
    }

    ~Car()
    {
        // If you try to delete the pointer an exception will happen
        // delete current_speed;

        // now we have memory leak
    }

};

int main()
{
    Car car1("Nissan", "white", 280);
    Car car2(car1);

    car1.print();
    car2.print();
    cout<<endl;

    car1.changeColor("Black");
    // this will change current_speed in car1 only
    car1.accelerate();


    car1.print();
    car2.print();
    cout<<endl;

    return 0;
}
```