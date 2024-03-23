# Inheritance
The capability of a class to derive properties and characteristics from another class is called Inheritance. Inheritance is one of the most important features of Object-Oriented Programming. 

Inheritance is a feature or a process in which, new classes are created from the existing classes. The new class created is called “derived class” or “child class” and the existing class is known as the “base class” or “parent class”. The derived class now is said to be inherited from the base class.

When we say derived class inherits the base class, it means, the derived class inherits all the properties of the base class, without changing the properties of base class and may add new features to its own. These new features in the derived class will not affect the base class. The derived class is the specialized class for the base class.

### Sub Class
The class that inherits properties from another class is called Subclass or Derived Class. 
### Super Class
The class whose properties are inherited by a subclass is called Base Class or Superclass. 



# Why and when to use inheritance?

Consider a group of vehicles. You need to create classes for Bus, Car, and Motorcycle. The methods getFuelAmount(), getTankCapacity(), will be the same for all three classes.

If we create these classes avoiding inheritance then we have to write all of these functions in each of the three classes as  below: 

```
class Car
{
public:
    double tank_capacity, fuel_amount;
    double getFuelAmount() { return fuel_amount; }
    double getTankCapacity() { return tank_capacity; }
};

class Bus
{
public:
    double tank_capacity, fuel_amount;
    double getFuelAmount() { return fuel_amount; }
    double getTankCapacity() { return tank_capacity; }
};

class Motorcycle
{
public:
    double tank_capacity, fuel_amount;
    double getFuelAmount() { return fuel_amount; }
    double getTankCapacity() { return tank_capacity; }
};
```




You can clearly see that the above process results in duplication of the same code 3 times. This increases the chances of error and data redundancy.
To avoid this type of situation, inheritance is used. If we create a class Vehicle and write these three functions in it and inherit the rest of the classes from the vehicle class, then we can simply avoid the duplication of data and increase re-usability. Look at the below diagram in which the three classes are inherited from vehicle class:

![](/assets/images/inheritance.png)

Using inheritance, we have to write the functions only one time instead of three times as we have inherited the rest of the three classes from the base class (Vehicle).
Implementing inheritance in C++: For creating a sub-class that is inherited from the base class we have to follow the below syntax. 

### Derived Classes:
A Derived class is defined as the class derived from the base class.
### Syntax: 
```
class  <derived_class_name> : <access-specifier> <base_class_name>
{
        //body
}
```

Where
class      — keyword to create a new class
derived_class_name   — name of the new class, which will inherit the base class
access-specifier  — either of private, public or protected. If neither is specified, PRIVATE is taken as default
base-class-name  — name of the base class

> Note: A derived class doesn’t inherit access to private data members. However, it does inherit a full parent object, which contains any private members which that class declares.


### Source Code

```
#include <iostream>

using namespace std;


class Vehicle
{
public:
    double tank_capacity, fuel_amount;
    double getFuelAmount() { return fuel_amount; }
    double getTankCapacity() { return tank_capacity; }
};

class Car : public Vehicle
{

};

class Bus: public Vehicle
{

};

class Motorcycle: public Vehicle
{

};

int main()
{
    Car car;
    car.fuel_amount=40;
    Bus bus;
    bus.fuel_amount=60;
    Motorcycle motorcycle;
    motorcycle.fuel_amount=20;
}
```

