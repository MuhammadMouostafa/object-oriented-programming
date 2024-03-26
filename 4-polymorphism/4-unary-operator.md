# Overloading Unary Operator

### Using member function

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
};

int main() {
    Point p(1,2);
    cout<<p.x<<" "<<p.y<<endl;
    p=-p;
    cout<<p.x<<" "<<p.y<<endl;
}
```


### Using global function

```

```