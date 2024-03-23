# function Overriding
```
#include <iostream>

using namespace std;


class Vehicle
{
public:
    int current_speed = 0;
    void accelerate()
    {
        current_speed++;
    }
    
};

class Car : public Vehicle
{
public:
    void accelerate()
    {
        current_speed+=2;
    }
};

class Bus: public Vehicle
{
    
};


int main()
{
    Car car;
    car.accelerate();
    cout<<car.current_speed <<endl;
    Bus bus;
    bus.accelerate();
    cout<<bus.current_speed <<endl;
}
```