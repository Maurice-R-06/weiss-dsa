# Parameter Passing
In many languages, the passing of all parameters is via call-by-value: the actual argument is copied into the formal parameter. However, params in C++ could be large and complex, so this could be inefficient.
Additionally, sometimes it is desirable to alter the value being passed in. Thus, C++ has 4 ways of passing parameters.
To see the reason why call-by-value is not sufficient, consider the three function declarations below:
```cpp
double average( double a, double b);      // returns average of a and b
void swap( double a, double b );          // swaps a and b; wrong parameter types
string randomItem( vector<string> arr );  // returns a random item in arr; inefficient
```

Here, average illustrates an ideal use of call-by-value as we don't ever want to change x and y, so copying them into a and b is fine.
This doesn't work for swap though, since swap(x, y); would guarantee that no matter how swap is implemenetd, x and y will remai unchanged.
Thus, we need to declare swap like this:
```cpp
void swap( double & a, double & b );  // swaps a and b; correct parameter types
```

With this signature, a is a synonym for x and b is a synonym for y. This is known as call-by-reference or call-by-lvalue-reference more precisely.

In the third declaration above, we are coppying the inputted vector of strings whenever we call it, but that isn't necessary since really we only need to get a random int between 0 and the size of the inputted array - 1.
Thus, we can do:
```cpp
string randomItem( const vector<string> & arr );
```
Here, const is saying we won't change the elements of the inputted vector. & arr is saying that arr will be the same object as the inputted vector (giving it a name we can use in the function).
This could be much more efficient because we won't have to copy anything this way. Thi is known as call-by-reference-to-a-constant in C++ or call-by-constant reference.

A general rule:
1) Call-by-value is appropriate for small objects that shouldn't be altered by the function
2) Call-by-constant-reference is appropriate for large objects that should not be altered by the function and are expensive to copy.
3) Call-by-reference is appropriate for all objects that may be altered by the function.

Because C++11 adds rvalue reference, there is a fourth way to pass parameters: call-byrvalue-reference. 
The central concept is that since an rvalue stores a temporary about to be destroyed, an expression such as x = rval can be implemented by a move instead of a copy. This is often easier than copying and might just 
involve a simple pointer change. x = y can be a copy if y is an lvalue, but a move if y is an rvalue. This gives a primary use case of overloading a function based on wheteher a parameter is an lvalue or rvalue, i.e.
```cpp
string randomItem( const vector<string> & arr );    // returns random item in lvalue arr
string randomItem( vector<string> && arr );         // returns random item in rvalue arr

vector<string> v { "hello", "world" };
cout << randomItem( v ) << endl;                    // invokes lvalue method
cout << randomItem( { "hello", "world" } ) >> endl; // invokes rvalue method
```
