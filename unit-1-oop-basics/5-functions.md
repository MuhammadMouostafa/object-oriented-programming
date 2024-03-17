# Function Overloading
> All the rules of the global functions overloading will be applied to member functions.

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



# Const Function
If the function not planed change any data member then it will be good to be const function

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

    void print() const
    {
        cout<< type << ", " << max_speed << ", " << current_speed << endl;
        // if try to change any member variable, you will get compilation error
        // current_speed++;
    }
};

int main()
{
    Car car1("Nissan", 280); 
    car1.print();
  
    return 0;
}
```