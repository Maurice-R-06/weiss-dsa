# Lvalues, Rvalues, and References
In addition to pointers, C++ has references. Two of these reference types include rvalue reference and lvalue reference.

An lvalue is an expression that identifies a non-temporary object. An rvalue is an expression that identifies a temporary object or is a value not associated with any object. As examples:

```cpp
vector<string> arr( 3 );
const int x = 2;
int y;
...
int z = x + y;
string str = "foo";
vector<string> *ptr = &arr;
```

The only rvalues here are 2, "foo", x + y, str.substr(0,1). 2 and "foo" are rvalues because they are literals, while x + y is an rvalue because its value is temporary, i.e. it is stored somewhere prior to being assigned to z.
Intuitively, if a function call computes an expression whose value does not exist prior to the call and does not exist once the call is finished unless it is copied sommewhere, it is likely an rvalue.
Here are some examples of things that can and can't be done with lvalue references (which are declared by placing an & after some type)
```cpp
string str = "hell";
string & rstr = str;                 // rstr is another name for str
rstr += 'o'                          // changes str to "hello"
bool cond = (&str == &rstr);         // true; str and rstr are same object
string & bad1 = "hello";             // illeagl: "hello" is not a modifiable lvalue
string & bad2 = str + "";            // illegal: str+"" is not an lvalue
string & sub = str.substr( 0, 4 );   // illegal: str.substr( 0, 4 ) is not an lvalue
```

A quick identification test for an lvalue vs rvalue is whether you can take its address with &. Intuitively, if you can take the address with & it is likely an lvalue and otherwise is is likely an rvalue.

Here are some examples of things that can and can't be done with rvalue references (which are declared by placing && after some type)
```cpp
string str = "hell";
string && bad1 = "hello";   // Legal
string && bad2 = str + "";  // Legal
string && sub = sub.substr( 0, 4 ); // Legal
```

This is really just useful for getting rid of unwanted data and making copying easier so it becomes O(1) rather than O(n)

## lvalue references use #1: aliasing complicated names
The simplest use of lvalue references is to use a local reference variable solely for the purpose of renaming an object that is known by a complicated expression.

## lvalue reference use #2: range for loops
If we did something like this it wouldn't work because x assuems a copy of each value in the vector:
```cpp
for( auto x : arr )
  ++x;
```

However, this would actually work
```cpp
for( auto & x : arr )
  ++x;
```

## lvalue references use #3: avoiding a copy
Suppose we have a function findMax that returns the largets value in a vector or other large collection. Then, given a vector arr, if we invoke findMax, we would naturally write
```cpp
auto x = findMax( arr );
```

Notice that the result is that x will be a copy of the largest value in arr. If we need a copy for some reason that is fine, however in many instances we only need the value and will not make any changes to x.
In such cases, it would be more efficient to declare that x is another name for the largest value in arr, and hence we would declare x to be a reference (auto will deduce const-ness)
```cpp
auto & x = findMax( arr );
```

Note that this means that findMax would also specify a return type that indicates a reference variable.
Also, note that auto always yields a fresh copy unless explicitly asking for a reference with &.
