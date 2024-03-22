# Shallow Copy

In shallow copy, an object is created by simply copying the data of all variables of the original object. This works well if none of the variables of the object are defined in the heap section of memory. If some variables are dynamically allocated memory from heap section, then the copied object variable will also reference the same memory location.
This will create ambiguity and run-time errors, dangling pointer. Since both objects will reference to the same memory location, then change made by one will reflect those change in another object as well. Since we wanted to create a replica of the object, this purpose will not be filled by Shallow copy. 



# Deep Copy
In Deep copy, an object is created by copying data of all variables, and it also allocates similar memory resources with the same value to the object. In order to perform Deep copy, we need to explicitly define the copy constructor and assign dynamic memory as well, if required. Also, it is required to dynamically allocate memory to the variables in the other constructors, as well.


![](/assets/images/shallow-vs-deep-copy.png)


# When Shallow Copy Happen
- ### Default Copy Constructor
- ### Default assignment operator

'''
Car car1;

// Copy Constructor
Car car2 = car1;

// Default assignment operator
Car car3;
car3 = car1;
'''

> Note: C++ compiler implicitly creates a copy constructor and overloads assignment operator in order to perform shallow copy at compile time.


# Difference between Shallow and Deep copy

| Shallow Copy | Deep Copy |
| --- | --- |
|When we create a copy of object by copying data of all member variables as it is, then it is called shallow copy|When we create an object by copying data of another object along with the values of memory resources that reside outside the object, then it is called a deep copy|
|A shallow copy of an object copies all of the member field values.|Deep copy is performed by implementing our own copy constructor.|
|In shallow copy, the two objects are not independent|It copies all fields, and makes copies of dynamically allocated memory pointed to by the fields|
|It also creates a copy of the dynamically allocated objects|If we do not create the deep copy in a rightful way then the copy will point to the original, with disastrous consequences.|
|Shallow Copy stores the references of objects to the original memory address.|Deep copy stores copies of the object’s value.|
|Shallow Copy reflects changes made to the new/copied object in the original object.|Deep copy doesn’t reflect changes made to the new/copied object in the original object.|
|Shallow Copy stores the copy of the original object and points the references to the objects.|Deep copy stores the copy of the original object and recursively copies the objects as well.|
|A shallow copy is faster.|Deep copy is comparatively slower.|




