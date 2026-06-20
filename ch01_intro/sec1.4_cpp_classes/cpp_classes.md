# C++ Classes
## Basic class Syntax
A class in C++ consists of $\textbf{members}$, which can be data or functions. Functions are then called $\textbf{member functions}$ or often $\textbf{methods}$. 
Each instance of a class is an object. Each object contains the data components specified in the class, which are acted on by methods.

An example of a class is below, IntCell. The IntCell class has a single data member named storedValue and has four methods, two of which are read and write and the other two are constructors.

Notice the two labels public and private in IntCell. These labels determine the visiblity of class members. Here, everything except the storedValue data member is public, while storedValue is private. 
A member that is public may be accessed by any method in $\textbf{any}$ class, while a member that's private may only be accessed by methods in its class.
It is typical for data members to be declared private, restricting access to internal details of the class, while methods for general use are public. This is known as $\textbf{information hiding}$.

One reason making data members private is nice is that if your methods are good the user won't be able to change your data to something you don't want (like putting in a date that doesn't exist for a date data member).

Methods can be private as well, when they are only used for internal use and shouldn't be accessed by user.

The second thing we see are two $\textbf{constructors}$. A constructor is a method that describes how an instance of the class is constructed.
If no constructor is explicitly define, one that initializes the data memers using language defaults is automatically generated.
In the example we see, if we were to create an instance of IntCell then if we pass no arguments storedValue will become 0 and the first constructor will run automatically. If we pass one argument then the second constructor will run automatically and storedValue will be updated accordingly.


```cpp
/**
* A class for simulating an integer memory cell
*/
class IntCell
{
  public:
    /**
    * Construct the IntCell.
    * Initial value is 0.
    */
    IntCell()
      { storedValue = 0; }

    /**
    * Construct the IntCell.
    * Initial value is initialValue.
    */
    IntCell ( int initialValue )
    { storedValue = initialValue; }

    /**
    * Return the stored value.
    */
    int read()
      { return storedValue; }

    /**
    * Change the stored value to x.
    */
    void write( int x )
      { storedValue = x; }
  
  private:
    int storedValue;
};
```

This can be improved on. The IntCell constructor illustrates the $\textbf{default parameter}$. Thus, we can make the constructor accept an optional parameter intialValue, which will default to 0 if not provided.

## Initialization List
The IntCell constructor uses an initialization list prior to its body. The initialization list is used to intialize the data members dierctly. This saves time in the case where data members are class types with complex initializations. 
This is also required in some cases, such as when a data member is const (not changeable after object has been constructed), then the data member can only be initalized in the initialization list. 
Another case where it is required is if the data member is itself a class type that doesn't have a zero-parameter constructor.

```cpp
class IntCell
{
  public:
    explicit IntCell( int initialValue = 0 )
      : storedValue{ initialValue } { }
    int read() const
      { return storedValue; }
    void write( int x )
      { storedValue = x; }
  
  private:
    int storedValue;
};
```

The use of 'explicit' here makes it so C++ doesn't allow implicit type conversions of int where IntCell is expected by passing the int to the IntCell.
The use of const here makes it so C++ knows that read() won't manipulate data.

All one-parameter constructors should e made explicit.
A member function that examines but doesn't change state of its object is called an accessor.
A member function that changes the state is a mutator.
In C++ we can mark each member function as being accessor or mutator, which is important part of design process. Making a member function an accessor means using the const, like in read() in the code above.


## Separation of Interface and Implementation

In C++ it's common to separate class interface from implementation.
The interface lists the class and its members (data and functions). The implementation provides implementations of the functions. 

### Preprocessor Commands
The interface is usually places in a file ending with .h. Source code requiring knowledge of interface must #include the interface file. Source code needs knowledge of interface almost always. The reason for this is that using the interface will help the compiler catch some errors when we compile the source code, such as type checking. Then, the implementation file is compiled separately and they are combined at the end by a linker. The reason the implementation file is compiled separately is that it allows for us to change the source code and not have to compile the implementation code of the class again, which would be a waste of memory.The linker then uses the compiled implementation code again when linking the code together. 
Also, one issue could be if we have two files that include a class interface and then one includes the other, then we could include the interface twice. This is bad, sometimes illegal, so to prevent it we include something at the beginning of our interface files.

#### IntCell class interface in file IntCell.h (interface file)
```cpp
#ifndef IntCell_H // This is saying if IntCell_H is not defined, which will be true if the interface file hasn't been included yet and false if it has
#define IntCell_H // Then, if the above is true, we will define it here. Otherwise, we don't define it again to save memory

class IntCell
{
  public:
    explicit IntCell( int initialValue = 0 );
    int read() const;
    void write( int x );
  
  private:
    int storedValue;
};
#endif
```

#### IntCell class implementation in file IntCell.cpp (implementation file)
```cpp
#include "IntCell.h"

/**
 * Construct the IntCell with initialValue
 */
IntCell::IntCell( int initialValue ) : storedValue{ initialValue }
{
}

/**
 * Return the stored value
 */
int IntCell::read() const
{
    return storedValue;
}

/**
 * Store x.
 */
void IntCell::write( int x )
{
    storedValue = x;
}
```

In the code above, when we use the :: we are telling C++ that we are referring to the things that have already been declared in our interface file.
The :: is called the scope resolution operator. The syntax is ClassName::member and each member function must identify the class that it is part of.
Also, the signature of an implemented member function must match the signature listed in the class interface. This include whether a member function is an accessor (const)
Note that default params are specified in the interface only (such as setting initialValue to 0 by default).
Note that objects are declaed like primitive types, i.e. IntCell obj1;   or IntCell obj2( 12 );


## vector and string
C++ standard defines two classes: vector and string. vector is intended to replace built-in C++ array (which sucks). And also default C++ strings suck. They don't have nice properties like checking that an index is valid or being able to check that two strings are equal with ==.
Thus, we use STL. STL treats arrays and strings as first-class objects. vector knows how big it is. string objects can be compared with ==. <. etc. They can both be copied with =.

```cpp
#include <iostream>
#include <vector>

int main()
{
    vector<int> squares( 100 );

    for( int i = 0; i < squares.size(); ++i )
      squares[ i ] = i * i;

    for (int i = 0; i < squares.size(); ++i )
      cout << i << " " << squares[ i ] << endl; // cout in C++ is to print and << with iostream means 'send to' so we are sending things to cout, which pushes them to terminal

    return 0;
}
```

In more recent versions of C++ we can initialize vectors like this:
```cpp
vector<int> daysInMonth = { 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31 };
```
We could also do the same thing without the =
But this can become a little weird because if we have something like
```cpp
vector<int> daysInMonth { 12 };
```
Does this mean that the vector should be size 12 or should have on element with value 12? Turns out it is the latter. If we wanted the former we would have to use () rather than {} with everything else the same

Also, strings have a length method and str1==str2 is true if the value of the strings are the same.

Also, we have a range in newer C++, so we can do stuff like:
```cpp
int sum = 0;
for( int x : squares )
  sum += x;
```
and this will add the elements of the vector called squares, i.e. x takes on the elements of squares. We can also replaces the type int in int x here with auto, i.e. auto x : squares and it will infer the type.
