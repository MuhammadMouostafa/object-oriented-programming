# Overloading Binary Operator



### using member function

```
#include <iostream>
using namespace std;

class Point
{
public:
    double x, y;
    Point(double x, double y): x(x), y(y) {}
    Point operator +(const Point & other){
        return Point(x + other.x, y + other.y);
    }  
};

int main() {
    Point p1(1,2);
    Point p2(3,4);
    Point p3= p1 + p2;
    cout<<p3.x<<" "<<p3.y<<endl;
}

```


### usinf global function