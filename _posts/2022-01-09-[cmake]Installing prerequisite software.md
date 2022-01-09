---
layout: post
title: [Cmake] Installing prerequisite software
date: 2022-01-09 21:26:00
author: "phjung1"
header-img: "#"
tags:

- Cmake
- clear linux

---

# Installing prerequisite software

An alternative to running the book recipes in a container is to install the dependencies directly on the host operating system. For this, we have assembled a minimal toolstack that can be used as a basic starting point for all of our recipes. You will have to install the following:

1. Cmake

2. Language-specific tools, that is , the compilers

3. Build autumation tools

4. Python

We weill also detail how to install the additional dependencies required by some of the recipes.

this post for "clear linux" distribution

## Cmake

installed

    cmake --version

## Compilers

installed

    g++ version

## Build-automation tools

    swupd bundle-add  dev-utils

## python

installed

    python --version

### **pipenv**

pipenv run to directly execute a command within the isolated environment.

    sudo pip install --user pip pipenv --upgrade

### conda

    sudo swupd bundle-add  conda

## Additional software

Some reciptes will require additional software, which will be covered in the following sections

### BLAS and LAPCK

The BLAS (**Basic Linear Algebra Subprograms**) are routines that provide standard building blocks for performing basic vector and matrix operations. ... Because the BLAS are efficient, portable, and widely available, they are commonly used in the development of high quality linear algebra software, LAPACK for example.

    sudo swupd bundle-add  openblas

### OpenMPI

MPI is a **standard library for performing parallel processing using a distributed memory model**. The Ruby, Owens, and Pitzer clusters at OSC can use the OpenMPI implementation of the Message Passing Interface (MPI).

    sudo swupd bundle-add  devpkg-openmpi

### eigen

Eigen provides **native CMake support** which allows the library to be easily used in CMake projects.

    sudo swupd bundle-add  devpkg-eigen

### boost

Boost is a set of libraries for the C++ programming language that provides support for tasks and structures such as linear algebra, pseudorandom number generation, multithreading, image processing, regular expressions, and unit testing. It contains 164 individual libraries

    swupd bundle-add  runtime-libs-boost

### Cross-compilers

**MinGW** ("Minimalist GNU for Windows"), formerly **mingw32**, is a [free and open source](https://en.wikipedia.org/wiki/Free_and_open-source_software "Free and open-source software") [software development](https://en.wikipedia.org/wiki/Software_development "Software development") environment to create [Microsoft Windows](https://en.wikipedia.org/wiki/Microsoft_Windows "Microsoft Windows") applications. The development of the MinGW project has been [forked](https://en.wikipedia.org/wiki/Software_forking "Software forking") with the creation in 2005–2008 of an alternative project called [Mingw-w64](https://en.wikipedia.org/wiki/Mingw-w64 "Mingw-w64").

    sudo swupd bundle-add  c-basic-mingw

### ZeroMQ, pkg-config, UUID, and Doxygen

  Doxygen

Documentation system for C++, Java, IDL, Python, and PHP. 

    sudo swupd bundle-add  doxygen
