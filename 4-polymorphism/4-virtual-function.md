# Virtual Function
A virtual function is a member function that is declared in the base class using the keyword virtual and is re-defined (Overridden) in the derived class. It tells the compiler to perform late binding where the compiler matches the object with the right called function and executes it during the runtime. This technique falls under Runtime Polymorphism.


A virtual function (also known as virtual methods) is a member function that is declared within a base class and is re-defined (overridden) by a derived class. When you refer to a derived class object using a pointer or a reference to the base class, you can call a virtual function for that object and execute the derived class’s version of the method.

- Virtual functions ensure that the correct function is called for an object, regardless of the type of reference (or pointer) used for the function call.
- They are mainly used to achieve Runtime polymorphism.
- Functions are declared with a virtual keyword in a base class.
- The resolving of a function call is done at runtime.


# Rules for Virtual Functions
The rules for the virtual functions in C++ are as follows:

- Virtual functions cannot be static.
- A virtual function can be a friend function of another class.
- Virtual functions should be accessed using a pointer or reference of base class type to       achieve runtime polymorphism.
- The prototype of virtual functions should be the same in the base as well as the derived class.
- They are always defined in the base class and overridden in a derived class. It is not mandatory for the derived class to override (or re-define the virtual function), in that case, the base class version of the function is used.
- A class may have a virtual destructor but it cannot have a virtual constructor.



### See the following example
```
#include <iostream>

using namespace std;


class A
{
public:
    virtual void  f1()  { cout << "A::f1" << endl; } // virtual 
    void f2()           { cout << "A::f2" << endl; }
    void f3()           { cout << "A::f3" << endl; }
};
class B : public A
{
public:
    void f1()           { cout << "B::f1" << endl; } // virtual 
    virtual void f2()   { cout << "B::f2" << endl; } // virtual 
    void f3()           { cout << "B::f3" << endl; } 
};
class C : public B
{
public:
    void f1()           { cout << "C::f1" << endl; } // virtual 
    void f2()           { cout << "C::f2" << endl; } // virtual 
    virtual void  f3()  { cout << "C::f3" << endl; } // virtual 
};
class D : public C
{
public:
    void  f1()          { cout << "D::f1" << endl; } // virtual 
    void  f2()          { cout << "D::f2" << endl; } // virtual 
    void  f3()          { cout << "D::f3" << endl; } // virtual 
};


int main()
{
    A* obj1 = new C();
    obj1->f1();
    obj1->f2();
    obj1->f3();

    cout<<endl;

    B* obj2 = new C();
    obj2->f1();
    obj2->f2();
    obj2->f3();

    cout<<endl;

    C* obj3 = new D();
    obj3->f1();
    obj3->f2();
    obj3->f2();

}
```



# pure vertiual function
