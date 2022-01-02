---
layout: post
title: User-Defined Type -2
date: 2021-12-30 20:51:00
author: "phjung1"
header-img: "#"
tags:

- cpp
- A Tour of C++

---

# User-Defined Types -2



## Classes

Having the data specified separately from the operations on it has advantages, 

such as the ability to use the data in arbitrary ways. 

However, a tighter connection between the representation and the operations is needed for a user-defined type to have all the properties expected of a ‘‘real type.’’ In particular, 

we often want to keep the representation inaccessible to users so as to ease use, guarantee consistent use of the data, and allow us to later improve the representation. To do that we have to distinguish between the interface to a type (to be used by all) and its implementation (which has access to the otherwise inaccessible data).

The language mechanism for that is called a class. A class has a set of members, which can be data, function, or type members. The interface is defined by the public members of a class, and private members are accessible only through that interface. For example:



```cpp
class Vector {
public:
Vector(int s) :elem{new double[s]}, sz{s} { } // constr uct a Vector
double& operator[](int i) { return elem[i]; } // element access: subscripting
int size() { return sz; }
private:
double∗ elem; // pointer to the elements
int sz; // the number of elements
};
```

Given that, we can define a variable of our new type Vector:

```cpp
Vector v(6); // a Vector with 6 elements
```

We can illustrate a Vector object graphically:

![](https://raw.githubusercontent.com/phjung1/imageUploader/main/2021/12/30-20-55-37-2021-12-30-20-55-34-image.png)

Basically, the Vector object is a ‘‘handle’’ containing a pointer to the elements (elem) and the number of elements (sz). The number of elements (6 in the example) can vary from Vector object to Vector object, and a Vector object can have a different number of elements at different times (§4.2.3). However, the Vector object itself is always the same size. This is the basic technique for handling varying amounts of information in C++: a fixed-size handle referring to a variable amount of data ‘‘elsewhere’’ (e.g., on the free store allocated by new; §4.2.2). How to design and use such objects is the main topic of Chapter 4.



Here, the representation of a Vector (the members elem and sz) is accessible only through the interface provided by the public members: Vector(), operator[](), and siz e(). The read_and_sum() example from §2.2 simplifies to:



```cpp
double read_and_sum(int s)
{
Vector v(s); // make a vector of s elements
for (int i=0; i!=v.siz e(); ++i)
cin>>v[i]; // read into elements
double sum = 0;
for (int i=0; i!=v.siz e(); ++i)
sum+=v[i]; // take the sum of the elements
return sum;
}
```

A member ‘‘function’’ with the same name as its class is called a constructor, that is, a function used to construct objects of a class. So, the constructor, Vector(), replaces vector_init() from §2.2. Unlike an ordinary function, a constructor is guaranteed to be used to initialize objects of its class. Thus, defining a constructor eliminates the problem of uninitialized variables for a class.



Vector(int) defines how objects of type Vector are constructed. In particular, it states that it needs an integer to do that. That integer is used as the number of elements. The constructor initializes the Vector members using a member initializer list:



```cpp
:elem{new double[s]}, sz{s}
```

That is, we first initialize elem with a pointer to s elements of type double obtained from the free store. Then, we initialize sz to s. Access to elements is provided by a subscript function, called operator[]. It returns a reference to the appropriate element (a double& allowing both reading and writing).



The siz e() function is supplied to give users the number of elements. Obviously, error handling is completely missing, but we’ll return to that in §3.5. Similarly, we did not provide a mechanism to ‘‘give back’’ the array of doubles acquired by new; §4.2.2 shows how to use a destructor to elegantly do that.



There is no fundamental difference between a struct and a class; a struct is simply a class with members public by default. For example, you can define constructors and other member functions for a struct.



## Unions

A union is a struct in which all members are allocated at the same address so that the union occupies only as much space as its largest member. Naturally, a union can hold a value for only one member at a time. For example, consider a symbol table entry that holds a name and a value. The value can either be a Node∗ or an int:



```cpp
enum Type { ptr, num }; // a Type can hold values ptr and num (§2.5)
struct Entry {
string name; // str ing is a standard-librar y type
Type t;
Node∗ p; // use p if t==ptr
int i; // use i if t==num
};
void f(Entry∗ pe)
{
if (pe−>t == num)
cout << pe−>i;
// ...
}
```

The members p and i are never used at the same time, so space is wasted. It can be easily recovered by specifying that both should be members of a union, like this:

```cpp
union Value {
Node∗ p;
int i;
};
```

The language doesn’t keep track of which kind of value is held by a union, so the programmer must do that:

```cpp
struct Entry {
string name;
Type t;
Value v; // use v.p if t==ptr; use v.i if t==num
};

void f(Entry∗ pe)
{
if (pe−>t == num)
cout << pe−>v.i;
// ...
}
```

Maintaining the correspondence between a type field (here, t) and the type held in a union is errorprone. To avoid errors, we can enforce that correspondence by encapsulating the union and the type field in a class and offer access only through member functions that use the union correctly. At the application level, abstractions relying on such tagged unions are common and useful. The use of ‘‘naked’’ unions is best minimized.



of ‘‘naked’’ unions is best minimized. The standard library type, variant, can be used to eliminate most direct uses of unions. A variant stores a value of one of a set of alternative types (§13.5.1). For example, a variant<Node∗,int> can hold either a Node∗ or an int. Using variant, the Entr y example could be written as:



```cpp
struct Entry {
string name;
variant<Node∗,int> v;
};
void f(Entry∗ pe)
{
if (holds_alternative<int>(pe−>v)) // does *pe hold an int? (see §13.5.1)
cout << get<int>(pe−>v); // get the int
// ...
}
```

For many uses, a variant is simpler and safer to use than a union



## Enumerations

In addition to classes, C++ supports a simple form of user-defined type for which we can enumerate the values:



```cpp
enum class Color { red, blue , green };
enum class Traffic_light { green, yellow, red };
Color col = Color::red;
Traffic_light light = Traffic_light::red;
```

Note that enumerators (e.g., red) are in the scope of their enum class, so that they can be used repeatedly in different enum classes without confusion. For example, Color::red is Color’s red which is different from Traffic_light::red.



Enumerations are used to represent small sets of integer values. They are used to make code more readable and less error-prone than it would have been had the symbolic (and mnemonic) enumerator names not been used.



The class after the enum specifies that an enumeration is strongly typed and that its enumerators are scoped. Being separate types, enum classes help prevent accidental misuses of constants. In particular, we cannot mix Traffic_light and Color values:



```cpp
Color x = red; // error : which red?
Color y = Traffic_light::red; // error : that red is not a Color
Color z = Color::red; // OK
```

Similarly, we cannot implicitly mix Color and integer values:

```cpp
int i = Color::red; // error : Color ::red is not an int
Color c = 2; // initialization error: 2 is not a Color
```

```cpp
Color x = Color{5}; // OK, but verbose
Color y {6}; // also OK
```

By default, an enum class has only assignment, initialization, and comparisons (e.g., == and <; §1.4) defined. However, an enumeration is a user-defined type, so we can define operators for it:



```cpp
Traffic_light& operator++(Traffic_light& t) // prefix increment: ++
{
switch (t) {
case Traffic_light::green: return t=Traffic_light::yellow;
case Traffic_light::yellow: return t=Traffic_light::red;
case Traffic_light::red: return t=Traffic_light::green;
}
}
Traffic_light next = ++light; // next becomes Traffic_light::green
```

If you don’t want to explicitly qualify enumerator names and want enumerator values to be ints (without the need for an explicit conversion), you can remove the class from enum class to get a ‘‘plain’’ enum. The enumerators from a ‘‘plain’’ enum are entered into the same scope as the name of their enum and implicitly converts to their integer value. For example:



```cpp
enum Color { red, green, blue };
int col = green;
```

Here col gets the value 1. By default, the integer values of enumerators start with 0 and increase by one for each additional enumerator. The ‘‘plain’’ enums hav e been in C++ (and C) since the earliest days, so even though they are less well behaved, they are common in current code.
