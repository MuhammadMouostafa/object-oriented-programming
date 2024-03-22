# Data members Initialization in declaration

```
#include <iostream>

using namespace std;

class Car
{
public:
    std::string type = "Nissan";
    int current_speed = 0, max_speed = 280;

    void print()
    {
        cout<< type << ", " << max_speed << ", " << current_speed << endl;
    }
};

int main()
{
    Car car1;
    car1.print();
    return 0;
}
```

# Data members Initialization in initializer list

```
#include <iostream>

using namespace std;

class Car
{
public:
    std::string type;
    int current_speed, max_speed;
    Car(string in_type, int in_max_speed):
    type(in_type),
    max_speed(in_max_speed),
    current_speed(0)
    {
      // do some thing
    }
    void print()
    {
        cout<< type << ", " << max_speed << ", " << current_speed << endl;
    }
};

int main()
{
    Car car1("Nissan", 280);
    car1.print();
    return 0;
}
```

# Constructor call another in initializer list
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
    Car(string in_type)  :Car(in_type,280)
    {
    }
    void print()
    {
        cout<< type << ", " << max_speed << ", " << current_speed << endl;
    }
};

int main()
{
    Car car1("Nissan", 250);  // Will call constructor Car(string in_type, int in_max_speed)
    Car car2("Audi");         // Will call constructor Car(string in_type)
    
    car1.print();
    car2.print();
    
    return 0;
}
```