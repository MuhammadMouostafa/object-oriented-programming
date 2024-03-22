# Const Function
If the function not planed change any data member then it will be good to be const function

```
#include <iostream>

using namespace std;

class Car
{
public:
    std::string type, color;
    int current_speed;
    Car(string type, string color):
    type(type),
    color(color),
    current_speed(0)
    {}

    void print() const
    {
        cout<< type << ", " << color << " " << current_speed << endl;
        // if try to change any member variable, you will get compilation error
        // current_speed++;
    }
    void changeColor(string new_color)
    {
        color = new_color;
    }
};


int main()
{
    Car car1("Nissan", "Black"); 
    car1.print();
    car1.changeColor("Red");
    car1.current_speed++;
    car1.print();


    Car* car2 = new Car("Audi", "Black");
    car2->print();
    car2->changeColor("Red");
    car2->current_speed++;
    car2->print();
}

```

# Const Object

```
int main()
{
    const Car car1("Nissan", "Black"); 
    car1.print();
    // car1.changeColor("Red");    // we can't call any non const member function
    // car1.current_speed++;        // we can't change any member vairable


    const Car* car2 = new Car("Audi", "Black");
    car2->print();
    // car2->changeColor("Red");   // we can't call any non const member function
    // car2->current_speed++;      // we can't change any member vairable
}
```




# Const Pointer

```
int main()
{
    Car car1("Nissan", "Black"); 
    

    Car* const car2 = new Car("Audi", "Black");
    car2->print();
    car2->changeColor("Red");
    car2->current_speed++;
    car2->print();
    // car2 = &car1;      // we can't change the value of const pointer
}
```





# const member variables


# static

## static data member

## static member function