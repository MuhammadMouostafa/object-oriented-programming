# Function Overloading
When there are multiple functions with the same name but different parameters, then the functions are said to be overloaded, hence this is known as Function Overloading. Functions can be overloaded by changing the number of arguments or/and changing the type of arguments. In simple terms, it is a feature of object-oriented programming providing many functions that have the same name but distinct parameters when numerous tasks are listed under one function name. There are certain Rules of Function Overloading that should be followed while overloading a function.

```
#include <iostream>

using namespace std;

class Car
{
public:
    std::string type;
    int current_speed, max_speed;
    Car(string in_type, int in_max_speed)
    {
      type = in_type;
      max_speed = in_max_speed;
      current_speed = 0;
    }

    void update(string in_type, int in_max_speed)
    {
      type = in_type;
      max_speed = in_max_speed;
    }
    void update(string in_type)
    {
      type = in_type;
    }
};

int main()
{
    Car car1("Nissan", 250); 
    cout<< car1.type << ", " << car1.max_speed << ", " << car1.current_speed << endl;
    car1.update("Audi", 280);
    cout<< car1.type << ", " << car1.max_speed << ", " << car1.current_speed << endl;
    car1.update("Volvo");
    cout<< car1.type << ", " << car1.max_speed << ", " << car1.current_speed << endl;
    
    return 0;
}
```

# Function Default Parameter
```
#include <iostream>

using namespace std;

class Car
{
public:
    std::string type;
    int current_speed, max_speed;
    Car(string in_type, int in_max_speed)
    {
      type = in_type;
      max_speed = in_max_speed;
      current_speed = 0;
    }

    void update(string in_type, int in_max_speed = 280)
    {
      type = in_type;
      max_speed = in_max_speed;
    }
    // Don't create function with one string paramiter, it will get compilation error
    /*
    void update(string in_type)
    {
      type = in_type;
    }
    */
};

int main()
{
    Car car1("Nissan", 280); 
    cout<< car1.type << ", " << car1.max_speed << ", " << car1.current_speed << endl;
    car1.update("Audi", 250);
    cout<< car1.type << ", " << car1.max_speed << ", " << car1.current_speed << endl;
    car1.update("Volvo");
    cout<< car1.type << ", " << car1.max_speed << ", " << car1.current_speed << endl;
    
    return 0;
}
```

