---
layout: post
title: User-Defined Type
date: 2021-12-29 21:09:00
author: "phjung1"
header-img: "#"
tags:

- cpp
- A Tour of C++

---

# User-Defined Types

## Introduciton

We call the types that can be built from the fundamental types (§1.4), the const modifier (§1.6), and the declarator operators (§1.7) built-in types. C++’s set of built-in types and operations is rich, but deliberately low-level. They directly and efficiently reflect the capabilities of conventional computer hardware. However, they don’t provide the programmer with high-level facilities to conveniently write advanced applications. Instead, C++ augments the built-in types and operations with a sophisticated set of abstraction mechanisms out of which programmers can build such high-level facilities

The C++ abstraction mechanisms are primarily designed to let programmers design and implement their own types, with suitable representations and operations, and for programmers to simply and elegantly use such types. Types built out of other types using C++’s abstraction mechanisms are called user-defined types. They are referred to as classes and enumerations. User defined types can be built out of both built-in types and other user-defined types. Most of this book is devoted to the design, implementation, and use of user-defined types. User-defined types are often preferred over built-in types because they are easier to use, less error-prone, and typically as efficient for what they do as direct use of built-in types, or even faster.

The rest of this chapter presents the simplest and most fundamental facilities for defining and using types. Chapters 4–7 are a more complete description of the abstraction mechanisms and the programming styles they support. Chapters 8–15 present an overview of the standard library, and since the standard library mainly consists of user-defined types, they provide examples of what can be built using the language facilities and programming techniques presented in Chapters 1–7.

## Structures

The first step in building a new type is often to organize the elements it needs into a data structure, a struct:

```cpp
struct Vector {
int sz; // number of elements
double∗ elem; // pointer to elements
};
```

This first version of Vector consists of an int and a double∗

A variable of type Vector can be defined like this:

```cpp
Vector v;
```

However, by itself that is not of much use because v’s elem pointer doesn’t point to anything. For it to be useful, we must give v some elements to point to. For example, we can construct a Vector like this:

```cpp
void vector_init(Vector& v, int s)
{
v.elem = new double[s]; // allocate an array of s doubles
v.sz = s;
}
```

That is, v’s elem member gets a pointer produced by the new operator and v’s sz member gets the number of elements. The & in Vector& indicates that we pass v by non-const reference (§1.7); that way, vector_init() can modify the vector passed to it

The new operator allocates memory from an area called the free store (also known as dynamic memory and heap). Objects allocated on the free store are independent of the scope from which they are created and ‘‘live’’ until they are destroyed using the delete operator (§4.2.2). A simple use of Vector looks like this:

```cpp
double read_and_sum(int s)
// read s integers from cin and return their sum; s is assumed to be positive
{
Vector v;
vector_init(v,s); // allocate s elements for v
for (int i=0; i!=s; ++i)
cin>>v.elem[i]; // read into elements

double sum = 0;
for (int i=0; i!=s; ++i)
sum+=v.elem[i]; // compute the sum of the elements
return sum;
}
```

There is a long way to go before our Vector is as elegant and flexible as the standard-library vector. In particular, a user of Vector has to know every detail of Vector’s representation. The rest of this chapter and the next two gradually improve Vector as an example of language features and techniques. Chapter 11 presents the standard-library vector, which contains many nice improvements.

I use vector and other standard-library components as examples

- to illustrate language features and design techniques, and

- to help you learn and use the standard-library components

Don’t reinvent standard-library components such as vector and string; use them.

We use . (dot) to access struct members through a name (and through a reference) and −> to access struct members through a pointer. For example:

```cpp
void f(Vector v, Vector& rv, Vector∗ pv)
{
int i1 = v.sz; // access through name
int i2 = rv.sz; // access through reference
int i3 = pv−>sz; // access through pointer
}
```
