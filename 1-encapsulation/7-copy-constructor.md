# Copy Constructors
A Copy Constructor creates a new object, which is an exact copy of the existing object. The compiler provides a default Copy Constructor to all the classes.


# When is the copy constructor called?
- When an object is constructed based on another object of the same class. 
    > Car car1;
    >
    > Car car2 = car1;
    >
    > Car car3(car1);
- When an object of the class is passed (to a function) by value as an argument. 
    > fun(Car car1)
    > {
    > 
    > }
- When an object of the class is returned by value. 
    > Car creatCar()
    > {
    >
    > return Car("Nissan",280);
    >
    > }
- When the compiler generates a temporary object.
    > Car car1 = Car("Nissan",280);


# Default Copy Constructor

### Syntax:
```
class-name (class-name &){}
```

# Copy Constructor 
# Temporary Object