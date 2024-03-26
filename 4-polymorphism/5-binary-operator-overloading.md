# Binary Operator Overloading using member function

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
    Point operator +(const Point & other){
        return Point(x + other.x, y + other.y);
    }  
};

int main() {
    Point p1(1,2);
    Point p2(3,4);
    Point p3= p1 + p2;
    p3.print();
}
```


# Binary Operator Overloading using global function
The left operand of the operation must be the class that has the function

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

Point operator +(const Point &p1, const Point &p2){
    return Point(p1.x + p2.x, p1.y + p2.y);
}

int main() {
    Point p1(1,2);
    Point p2(3,4);
    Point p3= p1 + p2;
    p3.print();
}
```

# Binary Operator Overloading using friend function
Friend function used to access private members.

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
    friend Point operator +(const Point &p1, const Point &p2);
};

Point operator +(const Point &p1, const Point &p2){
    return Point(p1.x + p2.x, p1.y + p2.y);
}

int main() {
    Point p1(1,2);
    Point p2(3,4);
    Point p3= p1 + p2;
    p3.print();
}
```


# Binary Operator Overloading When the class is the right operand;

### This can be achived using global friend function

```
#include <iostream>
using namespace std;

class Point
{
public:
    double x, y;
    Point(double x, double y): x(x), y(y) {}
    void print(){
        cout<<"("<<x<<" , "<<y<<")";
    } 
    friend void operator << (ostream &out, const Point &p);
};

void operator << (ostream &out, const Point &p){
    out << "(" << p.x << " , " << p.y << ")" << endl;
} 

int main() {
    Point p1(1,2);
    cout<<p1;
}
```

### To print many variables you need to return the stream

```
#include <iostream>
using namespace std;

class Point
{
public:
    double x, y;
    Point(double x, double y): x(x), y(y) {}
    void print(){
        cout<<"("<<x<<" , "<<y<<")";
    } 
    friend ostream& operator << (ostream &out, const Point &p);
};

ostream& operator << (ostream &out, const Point &p){
    out << "(" << p.x << " , " << p.y << ")" << endl;
    return out;
} 

int main() {
    Point p1(1,2);
    cout<< p1 << endl;
}
```