# Scope resolution operator ::
one of it usages it writing the member function definition outside the class
```
class Car {
private:

    std::string type, color;
    int current_speed, max_speed;

public:
    
    Car(string type, string color, int max_speed);

};

Car::Car(string type, string color, int max_speed):
    type(type),
    color(color),
    max_speed(max_speed),
    current_speed(0)
    {}

```


# What vs How

separate the class in to files 
- Header file has declaration (what)
- Source file has implementation (how) 


# Header file
> car.hpp

```

# pragma once

class Car {
private:

    std::string type, color;
    int current_speed, max_speed;

    void  openAirBags();
    void accidentHappened();

public:

    Car(string type, string color, int max_speed);
    void accelerate();
    void slowDown();
    void stop();
    int getCurrentSpeed();
    string getColor();
    void setColor(string new_color);
    
};

```

# Source file
> car.cpp

```
#include "car.hpp"

Car::Car(string type, string color, int max_speed):
    type(type),
    color(color),
    max_speed(max_speed),
    current_speed(0)
    {}

void  Car::openAirBags()
{
    // open air bags
}

void Car::accidentHappened()
{
    stop();
    openAirBags();
}

void Car::accelerate()
{                 
    if(current_speed < max_speed)
    {
        current_speed ++;
    }
}

void Car::slowDown()
{                     
    if (current_speed > 0)
    {
        current_speed --;
    }
}
void Car::stop()
{                     
    current_speed = 0;
}

int Car::getCurrentSpeed()
{
    return current_speed;
}

string Car::getColor()
{
    return color;
}

void Car::setColor(string new_color)
{
    color = new_color;
}

```
