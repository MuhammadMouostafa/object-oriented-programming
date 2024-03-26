# Operator Overloading
In C++, Operator overloading is a compile-time polymorphism. It is an idea of giving special meaning to an existing operator in C++ without changing its original meanin
For example, we can make use of the addition operator (+) for string class to concatenate two strings. We know that the task of this operator is to add two operands. So a single operator ‘+’, when placed between integer operands, adds them and when placed between string operands, concatenates them. 





# Difference between Operator Functions and Normal Functions
Operator functions are the same as normal functions. The only differences are, that the name of an operator function is always the operator keyword followed by the symbol of the operator, and operator functions are called when the corresponding operator is used.

# Operators that can be Overloaded in C++

- Unary operators
- Binary operators
- Special operators ( [ ], (), etc)



|Operators that can be overloaded|Examples|
|---|---|
|Binary Arithmetic|	+, -, *, /, %|
|Unary Arithmetic| 	+, -, ++, —|
|Assignment|	=, +=,*=, /=,-=, %=|
|Bitwise|	& , \| , << , >> , ~ , ^|
|De-referencing|	(->)|
|Dynamic memory allocation, De-allocation|	New, delete |
|Subscript|	[ ]|


# Operators that can't be Overloaded in C++

- sizeof
- typeid
- Scope resolution (::)
- Class member access operators (.(dot), .* (pointer to member operator))
- Ternary or conditional (?:)


# Important Points about Operator Overloading 
## For operator overloading to work
At least one of the operands must be a user-defined class object.

## Assignment Operator
Compiler automatically creates a default assignment operator with every class. The default assignment operator does assign all members of the right side to the left side and works fine in most cases (this behavior is the same as the copy constructor).


## Conversion Operator
We can also write conversion operators that can be used to convert one type to another type. 
Overloaded conversion operators must be a member method. Other operators can either be the member method or the global method.

## Any constructor that can be called with a single argument
works as a conversion constructor, which means it can also be used for implicit conversion to the class being constructed.
