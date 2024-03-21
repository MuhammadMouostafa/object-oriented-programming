# Access Modifiers

```
class MyClass {
private:
    int private_int;
    void privateFunction()
    {
        // can access and private/public member data/function
    }
public:
    int public_int;
    void publicFunction()
    {
        // can access and private/public member data/function
    }
};

```





Its better to hide member as much as possible.

# Check this class and think about what not needed to be public

```
class Car {
public:
    std::string type, color;
    int current_speed, max_speed;
    bool right_signal_stat, left_signal_stat;

    Car(string type, string color, int max_speed):
    type(type),
    color(color),
    max_speed(max_speed),
    current_speed(0),
    right_signal_stat(false),
    left_signal_stat(false)
    {}

    void accelerate()
    {                 
        if(current_speed < max_speed)
        {
            current_speed ++;
        }
    }

    void slowDown()
    {                     
        if (current_speed > 0)
        {
            current_speed --;
        }
    }
    void stop()
    {                     
        current_speed = 0;
    }

    void turnRightSignalOn()
    {
        right_signal_stat = true;
    }

    void turnRightSignalOff()
    {
        right_signal_stat = false;
    }

    void turnLeftSignalOn()
    {
        left_signal_stat = true;
    }

    void turnLeftSignalOff()
    {
        left_signal_stat = false;
    }

    void turnRight()
    {
        turnRightSignalOn();

        // do some thing to turn the car right

        turnRightSignalOff();
    }

    void turnLeft()
    {
        turnLeftSignalOn();

        // do some thing to turn the car left

        turnLeftSignalOff();
    }

    void  openAirBags()
    {
        // open air bags
    }

    void accidentHappened()
    {
        stop();
        openAirBags();
    }

};

```

# Check it after hide

```
class Car {
private:

    std::string type, color;
    int current_speed, max_speed;
    bool right_signal_stat, left_signal_stat;

    void turnRightSignalOn()
    {
        right_signal_stat = true;
    }

    void turnRightSignalOff()
    {
        right_signal_stat = false;
    }

    void turnLeftSignalOn()
    {
        left_signal_stat = true;
    }

    void turnLeftSignalOff()
    {
        left_signal_stat = false;
    }

    void  openAirBags()
    {
        // open air bags
    }

    void accidentHappened() // it called when sensors detect an accident
    {
        stop();
        openAirBags();
    }

public:

    Car(string type, string color, int max_speed):
    type(type),
    color(color),
    max_speed(max_speed),
    current_speed(0),
    right_signal_stat(false),
    left_signal_stat(false)
    {}

    void accelerate()
    {                 
        if(current_speed < max_speed)
        {
            current_speed ++;
        }
    }

    void slowDown()
    {                     
        if (current_speed > 0)
        {
            current_speed --;
        }
    }
    void stop()
    {                     
        current_speed = 0;
    }
    

    void turnRight()
    {
        turnRightSignalOn();

        // do some thing to turn the car right

        turnRightSignalOff();
    }

    void turnLeft()
    {
        turnLeftSignalOn();

        // do some thing to turn the car left

        turnLeftSignalOff();
    }

};
```



# Setters and Getters

Modify Car class make the use able use the current_spedd and use/change the color


```
class Car {
private:

    std::string type, color;
    int current_speed, max_speed;
    bool right_signal_stat, left_signal_stat;

    void turnRightSignalOn()
    {
        right_signal_stat = true;
    }

    void turnRightSignalOff()
    {
        right_signal_stat = false;
    }

    void turnLeftSignalOn()
    {
        left_signal_stat = true;
    }

    void turnLeftSignalOff()
    {
        left_signal_stat = false;
    }

    void  openAirBags()
    {
        // open air bags
    }

    void accidentHappened() // it called when sensors detect an accident
    {
        stop();
        openAirBags();
    }

public:

    Car(string type, string color, int max_speed):
    type(type),
    color(color),
    max_speed(max_speed),
    current_speed(0),
    right_signal_stat(false),
    left_signal_stat(false)
    {}

    void accelerate()
    {                 
        if(current_speed < max_speed)
        {
            current_speed ++;
        }
    }

    void slowDown()
    {                     
        if (current_speed > 0)
        {
            current_speed --;
        }
    }
    void stop()
    {                     
        current_speed = 0;
    }
    

    void turnRight()
    {
        turnRightSignalOn();

        // do some thing to turn the car right

        turnRightSignalOff();
    }

    void turnLeft()
    {
        turnLeftSignalOn();

        // do some thing to turn the car left

        turnLeftSignalOff();
    }

    int getCurrentSpeed()
    {
        return current_speed;
    }

    string getColor()
    {
        return color;
    }

    void setColor(string new_color)
    {
        color = new_color;
    }
    
};
```

> NOTE: Avoid to creater setters and getters when it not needed




