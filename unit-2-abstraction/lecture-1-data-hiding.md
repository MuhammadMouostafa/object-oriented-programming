# Data Hiding

## Public
can be accessed inside and outisde the class
## Private
can be accessed inside the class only
## Protected
Will explaine it in the future




## prevent data curreption from users

```
#include<iostream>
#include<string>

class Car {
private:
    std::string type;
    int current_speed = 0, max_speed;
public:
    Car(string type, int max_speed):
    type(type),
    max_speed(max_speed),
    current_speed(0)
    {}

    void accelerate() { 
        if(current_speed<in_max_speed){
            current_speed ++;
        }
    }

    void brake() {
        if (current_speed > 0){
            current_speed ++;
        }
    }
};


int main() {
    
    Car car1("Nissan",280);
    Car car2("Audi",270);
    Car car3("Volvo", 250);

    
    // accelerate car1 current_speed by one
    car1.accelerate();

    car1.accelerate();
    car1.accelerate();

    cout<<car1.current_speed<<endl;

    // break; car1 current_speed by one
    car1.break();

    cout<<car1.current_speed<<endl;
    


    return 0;
}


```


## provide what and hide how