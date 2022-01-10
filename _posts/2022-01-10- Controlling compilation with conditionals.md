---
layout: post
title: Controlling compilation with conditionals
date: 2022-01-10 01:27:00
author: "phjung1"
header-img: "#"
tags:

- Cmake

---

# Controlling compilation with conditionals

The code for this recipe is available at [cmake-cookbook/chapter-01/recipe-04 at v1.0 · dev-cafe/cmake-cookbook · GitHub](https://github.com/dev-cafe/cmake-cookbook/tree/v1.0/chapter-01/recipe-04) and has a C++ example. The recipe is valid with CMake version 3.5 (and higher) and has been tested on GNU/Linux, macOS, and Windows.

so far, we have looked at fairly simple proejcts, where the execution flow for Cmake was linear: from a set of source files to a single executable, possibly via static or shared libaries. TO ensure complete control over the execution flow of all the steps involved in building a project, configuration, compilation,and linkage, Cmake offers its own language. In this recipem we will explore the use of the conditional construct if-else ifelse--endif.

The CMake language is fairly large and consists of basic control constructs, CMake-specific commands, and infrastructure for modularly extending the language with new functions. A complete overview can be found online here: [cmake-language(7) &mdash; CMake 3.22.1 Documentation](https://cmake.org/cmake/help/latest/manual/cmake-language.7.html).

## How to do it

Let us start with the same source code as for the previous recipe. We want to be able to toggle between two behaviors:

1. Build Message.hpp and Message.cpp into a library, static or shared, and then link the resulting library into the hello-world executable.

2. Build Message.hpp, Message.cpp, and hello-world.cpp into a single executable, without producing the library.

Let us construct CMakeLists.txt to achieve this:

1. We start out by defining the minimum CMake version, project name, and supported language:
   
    cmake_minimum_required(VERSION 3.5 FATAL_ERROR)
   
    project(recipe-04 LANGUAGES CXX)

2. We introduce a new variable, USE_LIBRARY. This is a logical variable and its value will be set to OFF. We also print its value for the user:
   
    set(USE_LIBRARY OFF)
   
    message(STATUS 'Compile sources into a library? ${USE_LIBRARY}')

3. Set the BUILD_SHARED_LIBS global variable, defined in CMake, to OFF. Calling add_library and omitting the second argument will build a static library:
   
    set(BUILD_SHARED_LIBS OFF)

4. We then introduce a variable, _sources, listing Message.hpp and Message.cpp:
   
    list(APPEND _sources Message.hpp Message.cpp)

5. We then introduce an if-else statement based on the value of USE_LIBRARY. If the logical toggle is true, Message.hpp and Message.cpp will be packaged into a library:
   
    if(USE_LIBRARY)
   
   # add_library will create a static library
   
   # since BUILD_SHARED_LIBS is OFF
   
      add_library(message ${_sources})
   
      add_executable(hello-world hello-world.cpp)
   
      target_link_libraries(hello-world message)
    else()
      add_executable(hello-world hello-world.cpp ${_sources})
    endif()

6. We can again build with the same set of commands. Since USE_LIBRARY is set to OFF, the hello-world executable will be compiled from all sources. This can be verified by running the objdump -x command on GNU/Linux.

## How it works

We have introduced two variables: USE_LIBRARY and BUILD_SHARED_LIBS. Both of them have been set to OFF. As detailed in the CMake language documentation, true or false values can be expressed in a number of ways:

- A logical variable is true if it is set to any of the following: 1, ON, YES, TRUE, Y, or a non-zero number.
- A logical variable is false if it is set to any of the following: 0, OFF, NO, FALSE, N, IGNORE, NOTFOUND, an empty string, or it ends in the suffix -NOTFOUND.

The USE_LIBRARY variable will toggle between the first and the second behavior. BUILD_SHARED_LIBS is a global flag offered by CMake. Remember that the add_library command can be invoked without passing the STATIC/SHARED/OBJECT argument. This is because, internally, the BUILD_SHARED_LIBS global variable is looked up; if false or undefined, a static library will be generated.

This example shows that it is possible to introduce conditionals to control the execution flow in CMake. However, the current setup does not allow the toggles to be set from outside, that is, without modifying CMakeLists.txt by hand. In principle, we want to be able to expose all toggles to the user, so that configuration can be tweaked without modifying the code for the build system. We will show how to do that in a moment.

The () in else() and endif() may surprise you when starting to read and write CMake code. The historical reason for these is the ability to indicate the scope. For instance, it is possible instead to use if(USE_LIBRARY) ... else(USE_LIBRARY) ... endif(USE_LIBRARY) if this helps the reader. This is a matter of taste.

When introducing the _sources variable, we have indicated to readers of this code that this is a local variable that should not be used outside the current scope by prefixing it with an underscore
