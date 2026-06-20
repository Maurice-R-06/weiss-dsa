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
