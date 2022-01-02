---
layout: post
title: cpp_programs
date: 2021-12-24 20:40:00
author: "phjung1"
header-img: "#"
tags:

- cpp

---

# Programs

c++ is a compiled language. For a program to run, its source text has to be processed bu a com-piler, producing object files, which are c

ombined by a linker yielding an executable program. A C++ program typically consists of many source code files (usually simply called source files)

![](https://raw.githubusercontent.com/phjung1/imageUploader/main/2021/12/24-20-44-45-2021-12-24-20-44-43-image.png)

An executable program is created for a specific hardware/system combination;

 it is not portable, say, from a Mac to a Windows PC. When we talk about portability of C++ programs, we usually mean portability of source code; 

that is, the source code can be successfully compiled and run on a variety of systems. 

The ISO C++ standard defines two kinds of entities:

 • Core language features, such as built-in types (e.g., char and int) and loops (e.g., for-statements and while-statements) 

• Standard-library components, such as containers (e.g., vector and map) and I/O operations (e.g., << and getline())

The standard-library components are perfectly ordinary C++ code provided by every C++ implementation. 

That is, the C++ standard library can be implemented in C++ itself and is (with very minor uses of machine code for things such as thread context switching). 

This implies that C++ is sufficiently expressive and efficient for the most demanding systems programming tasks. C++ is a statically typed language. 

That is, the type of every entity (e.g., object, value, name, and expression) must be known to the compiler at its point of use. 

The type of an object determines the set of operations applicable to it.
