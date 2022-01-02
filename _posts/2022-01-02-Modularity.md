---
layout: post
title: Modularity1
date: 2022-01-02 23:12:00
author: "phjung1"
header-img: "#"
tags:

- cpp
- A Tour of C++

---

# Modularity

## Introduction

A C++ program consists of many separately developed parts, such as functions (§1.2.1), userdefined types (Chapter 2), class hierarchies (§4.5), and templates (Chapter 6). The key to managing this is to clearly define the interactions among those parts. The first and most important step is to distinguish between the interface to a part and its implementation. At the language level, C++ represents interfaces by declarations. A declaration specifies all that’s needed to use a function or a type. 

For example:

```cpp
double sqrt(double); // the square root function takes a double and returns a double
class Vector {
public:
Vector(int s);
double& operator[](int i);
int size();

private:
double∗ elem; // elem points to an array of sz doubles
int sz;
};
```

The key point here is that the function bodies, the function definitions, are ‘‘elsewhere.’’ For this example, we might like for the representation of Vector to be ‘‘elsewhere’’ also, but we will deal with that later (abstract types; §4.3). The definition of sqr t() will look like this:

```cpp
double sqrt(double d) // definition of sqrt()
{
// ... algorithm as found in math textbook ...
}
```

For Vector, we need to define all three member functions:

```cpp
Vector::Vector(int s) // definition of the constructor
:elem{new double[s]}, sz{s} // initialize members
{
}
double& Vector::operator[](int i) // definition of subscripting
{
return elem[i];
}
int Vector::siz e() // definition of size()
{
return sz;
}
```

We must define Vector’s functions, but not sqr t() because it is part of the standard library. Howev er, that makes no real difference: a library is simply ‘‘some other code we happen to use’’ written with the same language facilities we use. 

There can be many declarations for an entity, such as a function, but only one definition
