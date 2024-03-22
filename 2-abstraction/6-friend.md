# Friend Class
A friend class can access private and protected members of other classes in which it is declared as a friend. It is sometimes useful to allow a particular class to access private and protected members of other classes.

We can declare a friend class in C++ by using the friend keyword.

### Syntax:
```
friend class class_name;    // declared in the base class
```

### See the following example:

```
class A {
private:
    int a;
public:
    A() { a = 0; }
    friend class B; // Friend Class
};

class B {
public:
    void showA(A& x) {
        // Since B is friend of A, it can access private members of A
        std::cout << "A::a=" << x.a;
    }
};
```



# Friend Function
Like a friend class, a friend function can be granted special access to private and protected members of a class in C++. They are the non-member functions that can access and manipulate the private and protected members of the class for they are declared as friends.

### A friend function can be:

- A global function
- A member function of another class


# Global Function as Friend Function

### Syntax:
```
friend return_type function_name (arguments);
```

### See the following example:

```
class A {
private:
    int a;
public:
    A() { a = 0; }
    friend void showA(A&); // Friend Global Function
};

void showA(A& x) {
    // Since showA() is friend of A, it can access private members of A
    std::cout << "A::a=" << x.a;
}

```

# Member Function of Another Class as Friend Function

### Syntax:
```
friend return_type class_name::function_name (arguments);
```

### See the following example:

```
class A {
private:
    int a;
public:
    A() { a = 0; }
    friend void B::showA(A&); // Friend Member Function
};

class B {
public:
    void showA(A& x) {
        // Since showA() is friend of A, it can access private members of A
        std::cout << "A::a=" << x.a;
    }
};

```