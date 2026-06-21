# Pointers
A pointer variable is a variable storing the adderess where another object resides. It's the funamental mechanism in many data structures. Writing code with pointers can make some operations much more
memory and time efficient, like insertion in the middle of an array (with pointers we can design it so we don't have to copy the whole thing).
In order to illustrate the operations that apply to pointers, below we rewrite some code from section 1.4.

```cpp
int main()
{
    IntCell *m; // declaring that m is a pointer, so it has the memory address where an IntCell will live

    m = new IntCell{ 0 }; // 'new' asks OS for memory and creates the object there, IntCell{ 0 } creates IntCell object while passing 0 as initialValue, 'm =' stores memory address of object to m
    m->write( 5 );       // The -> operator is how to access a method or variable on a pointer to an object. "Go to object m is pointing at and call its write method with 5."
    cout << "Cell contents: " << m->read() << endl;

    delete m;
    return 0;
}
```

Generally, lines 3 and 5 should be combined to make sure that m is assigned to a value before being used. 
In C++ 'new' returns a pointer to the newly created object.

## Garbage Collection and delete
In some languages, when an object is no longer referenced, it is subject to automatic garbage collection, i.e. the programmer doesn't have to worry about it. C++ doesn't have this, which means that the delete operation
must be used otherwise memory consumed by a pointer is lost until the program terminates. This is known as a memory leak. One rule is not to use new when an automatic variable can be used instead, since automatic
variables will die at the end of a function since they're local. Thus, when pointers are used and then not needed anymore they need to be deleted with 'delete' (such as above).

## Assignment and Comparison of Pointers
Assignment and comparison of pointer variables is based on the value of the pointer, i.e. the memory address that it stores. Thus, if two pointers point to two objects that are copies of eachother and thus the same,
the pointers will still not be considered equal.
Also, if lhs and rhs are pointers of compatible types, then lhs=rhs makes lhs point to the same object that rhs points to.

## Accessing Members of an Object through a Pointer
If a pointer variable points at a class type, then a (visible) member of the object being pointed at can bne accessed via the '->' operator, as shown in the example above, with write
Also, the address-of operator '&' is usefu; as it returns the memory location where an object resides
