# Unary Operator Overloading using member function

```
#include <iostream>
using namespace std;

class Point
{
public:
    double x, y;
    Point(double x, double y): x(x), y(y) {}
    Point operator -(){
        return Point(-x, -y);
    }
    void print(){
        cout<<"("<<x<<" , "<<y<<")"<<endl;
    }
};


int main() {
    Point p(1,2);
    p.print();
    p=-p;
    p.print();
}
```


# Unary Operator Overloading using  global function

```
#include <iostream>
using namespace std;

class Point
{
public:
    double x, y;
    Point(double x, double y): x(x), y(y) {}
    void print(){
        cout<<"("<<x<<" , "<<y<<")"<<endl;
    }
};

Point operator -(const Point &p){
    return Point( -p.x, -p.y);
}

int main() {
    Point p(1,2);
    p.print();
    p=-p;
    p.print();
}
```

# Unary Operator Overloading using friend function
Friend frun used to access private members.

```
#include <iostream>
using namespace std;

class Point
{
double x, y;
public:
    Point(double x, double y): x(x), y(y) {}
    void print(){
        cout<<"("<<x<<" , "<<y<<")"<<endl;
    }
    friend Point operator -(const Point &p);
};

Point operator -(const Point &p){
    return Point( -p.x, -p.y);
}

int main() {
    Point p(1,2);
    p.print();
    p=-p;
    p.print();
}
```
